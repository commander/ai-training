# QLoRA fine-tuning on decoder-only models

A guide to move from LoRA on FLAN-T5 (encoder-decoder) to QLoRA on Gemma, Qwen, or Llama (decoder-only).

## What QLoRA is

QLoRA is LoRA plus a 4-bit quantized base model. The base weights are stored in 4 bits and dequantized only during the forward pass. The adapter stays in 16 or 32 bits. This lets you train a 7B model on a 16 GB GPU.

The QLoRA paper adds three parts to standard LoRA:

- **NF4 (4-bit NormalFloat).** A data type made for normally distributed weights.
- **Double quantization.** The quantization constants are also quantized, which saves more memory.
- **Paged optimizers.** They move optimizer state to CPU memory during memory peaks, so the run does not fail.

---

## 1. Install the packages

```python
%pip install -q bitsandbytes peft transformers accelerate datasets evaluate rouge_score
```

For gated models (Gemma, Llama), accept the license on the model page first, and then log in:

```python
from huggingface_hub import login
login()
```

## 2. Load the model in 4 bits

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, BitsAndBytesConfig
from peft import LoraConfig, TaskType, get_peft_model, prepare_model_for_kbit_training

model_id = "Qwen/Qwen3-1.7B"   # or google/gemma-3-1b-it, meta-llama/Llama-3.2-1B-Instruct

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",            # the format from the QLoRA paper
    bnb_4bit_use_double_quant=True,       # quantizes the quantization constants too
    bnb_4bit_compute_dtype=torch.float16, # bfloat16 on an A100; the T4 needs float16
)

tokenizer = AutoTokenizer.from_pretrained(model_id)
if tokenizer.pad_token is None:
    tokenizer.pad_token = tokenizer.eos_token

model = AutoModelForCausalLM.from_pretrained(
    model_id, quantization_config=bnb_config, device_map="auto")

model = prepare_model_for_kbit_training(model, use_gradient_checkpointing=True)
model.config.use_cache = False
```

`prepare_model_for_kbit_training` casts the layer norms to float32, enables gradient checkpointing, and makes the input embeddings require gradients. Without it, training fails or gives `NaN` values.

Set `use_cache = False` during training. Gradient checkpointing and the key-value cache are not compatible.

## 3. Change the LoRA config

```python
lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,   # not SEQ_2_SEQ_LM
    r=16,
    lora_alpha=32,
    lora_dropout=0.05,
    bias="none",
    target_modules=["q_proj", "k_proj", "v_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
)
peft_model = get_peft_model(model, lora_config)
peft_model.print_trainable_parameters()
```

The module names are different in these models. T5 uses `q` and `v`. Gemma, Qwen, and Llama use `q_proj`, `k_proj`, and so on. To see the real names of any model:

```python
print({n.split(".")[-1] for n, _ in model.named_modules() if "proj" in n})
```

`target_modules="all-linear"` also works. It selects every linear layer automatically.

## 4. Tokenize differently

This is the biggest change. A decoder-only model has no separate label sequence. The prompt and the summary go into one sequence. Then you mask the prompt part with `-100`, so the loss covers only the summary.

```python
def tokenize_function(example):
    input_ids_list, labels_list = [], []

    for dialogue, summary in zip(example["dialogue"], example["summary"]):
        messages = [{"role": "user",
                     "content": f"Summarize the following conversation.\n\n{dialogue}"}]
        prompt = tokenizer.apply_chat_template(
            messages, tokenize=False, add_generation_prompt=True)

        prompt_ids = tokenizer(prompt, add_special_tokens=False).input_ids
        answer_ids = tokenizer(summary + tokenizer.eos_token,
                               add_special_tokens=False).input_ids

        ids = (prompt_ids + answer_ids)[:1024]
        labels = ([-100] * len(prompt_ids) + answer_ids)[:1024]

        input_ids_list.append(ids)
        labels_list.append(labels)

    return {"input_ids": input_ids_list, "labels": labels_list}

tokenized_datasets = dataset.map(
    tokenize_function, batched=True, remove_columns=dataset["train"].column_names)
```

`apply_chat_template` inserts the special tokens that the instruction-tuned model expects. Each model family uses a different format, so do not write the template by hand.

## 5. Training arguments

```python
from transformers import Trainer, TrainingArguments, DataCollatorForSeq2Seq

args = TrainingArguments(
    output_dir="/content/qlora-out",
    per_device_train_batch_size=1,
    gradient_accumulation_steps=8,      # effective batch size 8
    learning_rate=2e-4,
    num_train_epochs=1,
    warmup_ratio=0.03,
    lr_scheduler_type="cosine",
    optim="paged_adamw_8bit",           # the optimizer from the QLoRA paper
    fp16=True,                          # bf16=True on an A100
    gradient_checkpointing=True,
    logging_steps=50,
    eval_strategy="steps", eval_steps=500,
    save_strategy="steps", save_steps=500,
    save_total_limit=2,
    load_best_model_at_end=True,
    metric_for_best_model="loss",
    report_to="none",
)

trainer = Trainer(
    model=peft_model,
    args=args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=DataCollatorForSeq2Seq(tokenizer, padding=True, label_pad_token_id=-100),
)
trainer.train()
peft_model.save_pretrained("/content/qlora-adapter")
```

`DataCollatorForSeq2Seq` also works here. It pads `input_ids` with the pad token and `labels` with `-100`.

Set `eval_steps` and `save_steps` to about 500. Small values make the run much slower.

## 6. Generation is different

The output contains the prompt. You must remove it.

```python
peft_model.config.use_cache = True   # turn the cache on again for generation

def summarize(model, dialogue):
    messages = [{"role": "user",
                 "content": f"Summarize the following conversation.\n\n{dialogue}"}]
    inputs = tokenizer(tokenizer.apply_chat_template(
        messages, tokenize=False, add_generation_prompt=True),
        return_tensors="pt", add_special_tokens=False).to(model.device)

    out = model.generate(**inputs, max_new_tokens=128, num_beams=1,
                         do_sample=False, pad_token_id=tokenizer.pad_token_id)
    new_tokens = out[0][inputs.input_ids.shape[1]:]   # drop the prompt
    return tokenizer.decode(new_tokens, skip_special_tokens=True)
```

## 7. Load the adapter later

```python
from peft import PeftModel

base = AutoModelForCausalLM.from_pretrained(
    model_id, quantization_config=bnb_config, device_map="auto")
model = PeftModel.from_pretrained(base, "/content/qlora-adapter", is_trainable=False)
```

Use the same `BitsAndBytesConfig` for training and for loading. A different quantization gives different results.

---

## Model choice on a T4 (16 GB)

| Model | Size | Notes |
|---|---|---|
| Qwen3-1.7B, Qwen3-4B | small | Not gated. The best first choice. |
| Llama-3.2-1B/3B-Instruct | small | Gated. You need a Hugging Face token. |
| Gemma-3-1b-it, Gemma-3-4b-it | small | Gated. Gemma can give `NaN` values in float16. |
| Gemma 4 (E2B, E4B) | small | Released April 2026. Needs a recent `transformers` version. |

Start with a 1B–4B model. A 7B model in 4 bits fits in 16 GB, but training is slow on a T4, which has no bfloat16 hardware.

---

## Checks before a long run

These are the same habits that fixed the FLAN-T5 runs.

1. **Check the loss on one batch before training.** The expected value is about 1.5–2.5. A much higher value means the labels are wrong.
2. **Check that the prompt is masked.** Print one example: `labels` must start with a long sequence of `-100`.

   ```python
   labels = tokenized_datasets["train"][0]["labels"]
   print("Masked tokens:", sum(l == -100 for l in labels), "of", len(labels))
   ```

3. **Run 50 steps first.** Use `max_steps=50`, `eval_steps=10`, `logging_steps=10`. Start the full run only when the loss decreases.
4. **Load each model into its own object.** `get_peft_model` changes the model in place.
5. **Copy the adapter to Drive after training.** Colab deletes local files when the runtime stops.

---

## Learning resources

### Papers

- **QLoRA: Efficient Finetuning of Quantized LLMs** — Dettmers et al., 2023. The source of NF4, double quantization, and paged optimizers. <https://arxiv.org/abs/2305.14314>
- **LoRA: Low-Rank Adaptation of Large Language Models** — Hu et al., 2021. The base method. <https://arxiv.org/abs/2106.09685>
- **LLM.int8()** — Dettmers et al., 2022. Background on quantization of transformers. <https://arxiv.org/abs/2208.07339>

### Official documentation

- **Hugging Face PEFT documentation.** The reference for `LoraConfig`, `get_peft_model`, and `PeftModel`. <https://huggingface.co/docs/peft>
- **Transformers quantization guide.** The reference for `BitsAndBytesConfig`. <https://huggingface.co/docs/transformers/quantization/bitsandbytes>
- **bitsandbytes documentation.** <https://huggingface.co/docs/bitsandbytes>
- **TRL `SFTTrainer` documentation.** A higher-level trainer that does the prompt masking for you. <https://huggingface.co/docs/trl>

### Tutorials

- **Making LLMs even more accessible with bitsandbytes, 4-bit quantization and QLoRA** — the official Hugging Face blog post for the QLoRA release. <https://huggingface.co/blog/4bit-transformers-bitsandbytes>
- **The original QLoRA repository** — the reference implementation, with Colab notebooks for inference and fine-tuning. <https://github.com/artidoro/qlora>
- **FSDP-QLoRA guide** — how to train a 70B model on two 24 GB GPUs. <https://huggingface.co/docs/bitsandbytes/main/en/fsdp_qlora>

### Background on data types

- **Mixed Precision Training** — Micikevicius et al., 2018. <https://arxiv.org/abs/1710.03740>
- **Revisiting BFloat16 Training** — Zamirai et al., 2020. Why small weight updates are lost in low precision. <https://arxiv.org/abs/2010.06192>
- **FP8 Formats for Deep Learning** — Micikevicius et al., 2022. <https://arxiv.org/abs/2209.05433>

---

## Summary of the differences

| Item | FLAN-T5 (LoRA) | Decoder-only (QLoRA) |
|---|---|---|
| Model class | `AutoModelForSeq2SeqLM` | `AutoModelForCausalLM` |
| Task type | `SEQ_2_SEQ_LM` | `CAUSAL_LM` |
| Quantization | none | `BitsAndBytesConfig` 4-bit |
| Extra preparation | none | `prepare_model_for_kbit_training` |
| Target modules | `q`, `v` | `q_proj`, `k_proj`, `v_proj`, ... |
| Labels | the summary alone | prompt + summary, prompt masked |
| Prompt format | plain text | `apply_chat_template` |
| Generation output | the summary alone | prompt + summary; remove the prompt |
| Optimizer | `adamw_torch` | `paged_adamw_8bit` |
