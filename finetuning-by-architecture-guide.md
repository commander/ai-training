# Fine-tuning guide: encoder-only, encoder-decoder, and decoder-only models

A companion to the QLoRA decoder-only guide. It covers the three transformer families, what each one is for, and how LoRA or QLoRA fine-tuning changes between them.

---

## Does QLoRA work for all architectures?

Yes. QLoRA is two independent parts:

- **4-bit quantization** of the frozen base weights (`BitsAndBytesConfig`). This works on any `torch.nn.Linear` layer, in any model.
- **LoRA adapters** on selected layers. This works on any linear layer too.

Neither part depends on the architecture. The QLoRA paper tested both LLaMA (decoder-only) and T5 (encoder-decoder).

QLoRA is used mostly with decoder-only models for a practical reason: quantization saves memory, and only decoder-only models are commonly large enough for that to matter. An encoder-only model is 110M–400M parameters and fits in a few hundred MB, so 4-bit adds dequantization overhead for almost no benefit. Use plain LoRA, or even full fine-tuning, for those.

**Rule of thumb:** use 4-bit below about 16 GB of free GPU memory relative to the model size. Otherwise use 16-bit with LoRA.

---

## The three families

### Encoder-only

**Examples:** BERT, RoBERTa, DeBERTa-v3, ELECTRA, ModernBERT

The model reads the whole input at once, with attention in both directions. It produces one vector for each token. There is no generation step.

**Tasks:** text classification, sentiment analysis, named entity recognition, extractive question answering, sentence embeddings for search and retrieval.

**Sizes:** 110M–400M parameters, usually.

### Encoder-decoder

**Examples:** T5, FLAN-T5, BART, mT5, Pegasus, Whisper

The encoder reads the input. The decoder writes the output, one token at a time, and attends to the encoder output. This is the family you used for DialogSum.

**Tasks:** summarization, translation, question answering that generates an answer, speech to text.

**Sizes:** 60M–11B parameters.

### Decoder-only

**Examples:** GPT, Llama, Qwen, Gemma, Mistral, Phi

One stack. The model reads the prompt and continues it, one token at a time. Attention is one-directional: a token sees only the tokens before it.

**Tasks:** text generation, chat, instruction following, and in practice almost any task, because you can write the task as text.

**Sizes:** 500M–700B parameters.

---

## What changes between them

| Item | Encoder-only | Encoder-decoder | Decoder-only |
|---|---|---|---|
| Model class | `AutoModelForSequenceClassification` | `AutoModelForSeq2SeqLM` | `AutoModelForCausalLM` |
| PEFT task type | `SEQ_CLS` or `TOKEN_CLS` | `SEQ_2_SEQ_LM` | `CAUSAL_LM` |
| Labels | a class index (an integer) | the target token IDs | the full sequence, prompt masked with `-100` |
| Target modules | `query`, `value` | `q`, `v` | `q_proj`, `v_proj` |
| Extra trained layers | the classification head | none | none |
| Data collator | `DataCollatorWithPadding` | `DataCollatorForSeq2Seq` | `DataCollatorForSeq2Seq` |
| Metric | accuracy, F1 | ROUGE, BLEU | perplexity, or a task metric |
| Pad the labels | not needed | with `-100` | with `-100` |

The LoRA config itself (`r`, `lora_alpha`, `lora_dropout`, `bias`) is the same in all three.

---

## 1. Encoder-only: text classification

```python
import numpy as np, evaluate
from transformers import (AutoModelForSequenceClassification, AutoTokenizer,
                          Trainer, TrainingArguments, DataCollatorWithPadding)
from peft import LoraConfig, TaskType, get_peft_model

model_id = "roberta-base"
tokenizer = AutoTokenizer.from_pretrained(model_id)

model = AutoModelForSequenceClassification.from_pretrained(
    model_id, num_labels=2, id2label={0: "negative", 1: "positive"})

lora_config = LoraConfig(
    task_type=TaskType.SEQ_CLS,
    r=8, lora_alpha=16, lora_dropout=0.1, bias="none",
    target_modules=["query", "value"],
    modules_to_save=["classifier"],   # IMPORTANT: train and save the new head
)
peft_model = get_peft_model(model, lora_config)
peft_model.print_trainable_parameters()

def tokenize_function(example):
    return tokenizer(example["text"], truncation=True, max_length=256)

tokenized = dataset.map(tokenize_function, batched=True)
# The label column must be named "labels" and hold integers.

accuracy = evaluate.load("accuracy")

def compute_metrics(eval_pred):
    logits, labels = eval_pred
    return accuracy.compute(predictions=np.argmax(logits, axis=-1), references=labels)

trainer = Trainer(
    model=peft_model,
    args=TrainingArguments(
        output_dir="/content/cls-out",
        learning_rate=1e-3,           # higher than for generation tasks
        per_device_train_batch_size=16,
        num_train_epochs=3,
        eval_strategy="epoch",
        save_strategy="epoch",
        load_best_model_at_end=True,
        metric_for_best_model="accuracy",
        report_to="none",
    ),
    train_dataset=tokenized["train"],
    eval_dataset=tokenized["validation"],
    data_collator=DataCollatorWithPadding(tokenizer),
    compute_metrics=compute_metrics,
)
trainer.train()
```

**The most common mistake:** leaving out `modules_to_save=["classifier"]`. The classification head is new and randomly initialized. LoRA freezes everything that is not an adapter, so without this line the head stays random and the model predicts noise. The accuracy stays near 50% and the loss does not decrease.

### Token classification (NER)

Almost the same. The changes are:

- `AutoModelForTokenClassification` and `TaskType.TOKEN_CLS`
- `DataCollatorForTokenClassification`
- One label for each token, not one for each sequence. Use `word_ids()` from the tokenizer to align the word labels to the sub-word tokens, and set `-100` for the extra sub-word pieces.
- The metric is `seqeval`, not accuracy.

### Embeddings for search

Do not use `Trainer` for this. Use the `sentence-transformers` library. It provides the contrastive losses (`MultipleNegativesRankingLoss` and others) that embedding models need. It supports PEFT adapters.

---

## 2. Encoder-decoder: summarization

This is your DialogSum setup. The full working version:

```python
import torch
from transformers import (AutoModelForSeq2SeqLM, AutoTokenizer, Trainer,
                          TrainingArguments, DataCollatorForSeq2Seq)
from peft import LoraConfig, TaskType, get_peft_model

model_id = "google/flan-t5-base"
tokenizer = AutoTokenizer.from_pretrained(model_id)
model = AutoModelForSeq2SeqLM.from_pretrained(model_id)   # float32

lora_config = LoraConfig(
    task_type=TaskType.SEQ_2_SEQ_LM,
    r=32, lora_alpha=32, lora_dropout=0.05, bias="none",
    target_modules=["q", "v"],
)
peft_model = get_peft_model(model, lora_config)

def tokenize_function(example):
    prompts = ["Summarize the following conversation.\n\n" + d + "\n\nSummary: "
               for d in example["dialogue"]]
    inputs = tokenizer(prompts, truncation=True, max_length=512)
    inputs["labels"] = tokenizer(example["summary"], truncation=True,
                                 max_length=128).input_ids
    return inputs

tokenized = dataset.map(tokenize_function, batched=True,
                        remove_columns=dataset["train"].column_names)

trainer = Trainer(
    model=peft_model,
    args=TrainingArguments(
        output_dir="/content/seq2seq-out",
        learning_rate=3e-4,
        per_device_train_batch_size=4,
        num_train_epochs=3,
        warmup_ratio=0.05,
        eval_strategy="steps", eval_steps=500,
        save_strategy="steps", save_steps=500,
        save_total_limit=2,
        load_best_model_at_end=True,
        metric_for_best_model="loss",
        remove_unused_columns=False,
        report_to="none",
    ),
    train_dataset=tokenized["train"],
    eval_dataset=tokenized["validation"],
    data_collator=DataCollatorForSeq2Seq(tokenizer, model=peft_model,
                                         label_pad_token_id=-100),
)
trainer.train()
```

**The most common mistake:** padding the labels with the pad token instead of `-100`. The loss then counts hundreds of padding positions. The loss reads about 50 instead of about 1.9, and the model learns to predict padding.

To check, compare the loss on one batch with and without the mask. If the two values differ by a large factor, your labels are wrong.

### With 4-bit quantization

Only useful for the large sizes (`flan-t5-xl`, `flan-t5-xxl`, `flan-ul2`):

```python
from transformers import BitsAndBytesConfig
from peft import prepare_model_for_kbit_training

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True, bnb_4bit_quant_type="nf4",
    bnb_4bit_use_double_quant=True, bnb_4bit_compute_dtype=torch.float16)

model = AutoModelForSeq2SeqLM.from_pretrained(
    "google/flan-t5-xl", quantization_config=bnb_config, device_map="auto")
model = prepare_model_for_kbit_training(model)
# The LoraConfig stays the same.
```

Note: T5 can produce `NaN` values in float16. Prefer a GPU that supports bfloat16 for the large T5 sizes.

---

## 3. Decoder-only: instruction fine-tuning

See the separate guide for the full version. The summary:

```python
from transformers import AutoModelForCausalLM, BitsAndBytesConfig
from peft import LoraConfig, TaskType, get_peft_model, prepare_model_for_kbit_training

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True, bnb_4bit_quant_type="nf4",
    bnb_4bit_use_double_quant=True, bnb_4bit_compute_dtype=torch.float16)

model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen3-1.7B", quantization_config=bnb_config, device_map="auto")
model = prepare_model_for_kbit_training(model)
model.config.use_cache = False

lora_config = LoraConfig(
    task_type=TaskType.CAUSAL_LM,
    r=16, lora_alpha=32, lora_dropout=0.05, bias="none",
    target_modules=["q_proj", "k_proj", "v_proj", "o_proj",
                    "gate_proj", "up_proj", "down_proj"],
)
peft_model = get_peft_model(model, lora_config)
```

**The most common mistake:** not masking the prompt tokens in the labels. Then the model learns to generate the input dialogue as well as the summary. Training still runs and the loss decreases, so the error is easy to miss.

---

## How to find the target module names

The names differ between model families. Never copy them from another guide without checking.

```python
print({n.split(".")[-1] for n, m in model.named_modules()
       if isinstance(m, torch.nn.Linear)})
```

Common values:

| Family | Attention modules |
|---|---|
| BERT, RoBERTa, ELECTRA | `query`, `key`, `value`, `dense` |
| DeBERTa-v3 | `query_proj`, `key_proj`, `value_proj` |
| T5, FLAN-T5 | `q`, `k`, `v`, `o`, `wi_0`, `wi_1`, `wo` |
| BART, Pegasus, Whisper | `q_proj`, `k_proj`, `v_proj`, `out_proj` |
| Llama, Qwen, Gemma, Mistral | `q_proj`, `k_proj`, `v_proj`, `o_proj`, `gate_proj`, `up_proj`, `down_proj` |

`target_modules="all-linear"` selects every linear layer automatically. It is a safe default when you are not sure.

---

## Which family to choose

| Your task | Use |
|---|---|
| Classify text into fixed categories | Encoder-only. It is smaller, faster, and usually more accurate for this. |
| Extract entities or spans | Encoder-only. |
| Search or semantic similarity | Encoder-only, with `sentence-transformers`. |
| Summarize or translate, one task only | Encoder-decoder. It is efficient for this shape of problem. |
| Chat, or many tasks in one model | Decoder-only. |
| Follow free-form instructions | Decoder-only. |

A decoder-only model can do classification too, but a 400M encoder often matches or beats a 7B decoder on a fixed classification task, at a fraction of the cost.

---

## Checks that apply to all three

1. **Check the loss on one batch before training.** The expected value is about 1.5–2.5 for generation, or about `ln(num_labels)` for classification (about 0.69 for two classes).
2. **Check that the trainable parameter count is not zero.** Use `print_trainable_parameters()`. For classification, it must include the head.
3. **Run 50 steps first.** Start the full run only when the loss decreases.
4. **Load each model into its own object.** `get_peft_model` changes the model in place, so a shared object makes the "before" and "after" models identical.
5. **Save the adapter outside the runtime** after training.

---

## Resources

### Architecture background

- **Attention Is All You Need** — Vaswani et al., 2017. The original encoder-decoder transformer. <https://arxiv.org/abs/1706.03762>
- **BERT** — Devlin et al., 2018. The encoder-only design. <https://arxiv.org/abs/1810.04805>
- **T5 / "Exploring the Limits of Transfer Learning"** — Raffel et al., 2019. The text-to-text framing. <https://arxiv.org/abs/1910.10683>
- **What Language Model Architecture and Pretraining Objective Work Best for Zero-Shot Generalization?** — Wang et al., 2022. A direct comparison of the three families. <https://arxiv.org/abs/2204.05832>

### Practical

- **Hugging Face NLP Course** — chapters on fine-tuning for each task type. <https://huggingface.co/learn/nlp-course>
- **PEFT task guides** — one page for each task type, with working code. <https://huggingface.co/docs/peft/task_guides/image_classification_lora>
- **Transformers task guides** — sequence classification, token classification, summarization, causal language modeling. <https://huggingface.co/docs/transformers/tasks/sequence_classification>
- **sentence-transformers training guide** — for embedding models. <https://www.sbert.net/docs/sentence_transformer/training_overview.html>

### PEFT and quantization

- **LoRA paper** — Hu et al., 2021. <https://arxiv.org/abs/2106.09685>
- **QLoRA paper** — Dettmers et al., 2023. <https://arxiv.org/abs/2305.14314>
- **Making LLMs even more accessible with bitsandbytes, 4-bit quantization and QLoRA** — the official Hugging Face release post. <https://huggingface.co/blog/4bit-transformers-bitsandbytes>
