# Addendum (2025–2026): QLoRA fine-tuning on decoder-only models

**Created/updated:** 2026-10-10

This file extends `qlora-decoder-only-guide.md`. It lists 2025–2026 work that updates or extends that guide. The check was done on 2026-10-10. Only official lab reports, research from strong labs, and widely used open-source tools are included. Items from 2026 are marked **[2026]**.

---

## New research on LoRA settings

**LoRA Without Regret**
John Schulman and others at Thinking Machines Lab. Thinking Machines blog (research report), September 2025.
<https://thinkingmachines.ai/blog/lora/> · TRL reproduction: <https://huggingface.co/docs/trl/main/en/lora_without_regret>
A large set of sweeps that compares LoRA with full fine-tuning on Llama 3 and Qwen3, for SFT and RL. The main results:
- LoRA on all weight matrices, and in particular the MLP and MoE layers, matches full fine-tuning when the dataset is not too large for the adapter.
- Attention-only LoRA gives worse results, also when its rank is higher to match the parameter count.
- The best LoRA learning rate is about 10 times the best full fine-tuning learning rate.
- LoRA can lose more than full fine-tuning at large batch sizes. A higher rank does not fix this.
- For RL, even rank-1 LoRA matches full fine-tuning.
These results support the original guide's choice of all seven projection modules. Hugging Face TRL has a page that reproduces the results with `SFTTrainer` and `GRPOTrainer`.

## Model reports for the models in the guide

**Qwen3 Technical Report**
An Yang, Anfeng Li, Baosong Yang, et al. (Alibaba Qwen team). arXiv, May 2025.
<https://arxiv.org/abs/2505.09388>
The report for Qwen3, the default model in the original guide. Qwen3 has one model with a "thinking" mode and a "non-thinking" mode. The chat template controls the mode with the `enable_thinking` argument. This has an effect on the tokenization in Section 4 (see "Changes to earlier advice").

**Gemma 3 Technical Report**
Gemma Team (Google DeepMind). arXiv, March 2025.
<https://arxiv.org/abs/2503.19786>
The report for Gemma 3 (1B to 27B), which is in the original model table. Gemma 3 uses a mix of local and global attention layers and supports long context.

**gpt-oss-120b and gpt-oss-20b Model Card**
OpenAI. arXiv, August 2025.
<https://arxiv.org/abs/2508.10925> · Fine-tuning recipe: <https://cookbook.openai.com/articles/gpt-oss/fine-tune-transfomers>
Open-weight MoE reasoning models from OpenAI. The MoE weights are released in MXFP4, a 4-bit format. The OpenAI Cookbook shows LoRA fine-tuning of gpt-oss-20b with Transformers, PEFT, and TRL. Unsloth reports that it trains gpt-oss-20b with QLoRA in about 14 GB of VRAM (Unsloth release notes, August 2025). This makes a 20B MoE model a possible, but slow, option for a 16 GB GPU.

## New tool versions

**Transformers v5**
Hugging Face. **[2026]** v5.0.0 released 26 January 2026.
Migration guide: <https://github.com/huggingface/transformers/blob/main/MIGRATION_GUIDE_V5.md>
Two changes affect the code in the original guide:
- `load_in_4bit` and `load_in_8bit` are removed as `from_pretrained` arguments. Use `quantization_config=BitsAndBytesConfig(...)`. The original guide already does this.
- `apply_chat_template(..., tokenize=True)` now returns a `BatchEncoding` dict, not a list of token IDs. The original guide uses `tokenize=False`, so it is not affected. Code that reads the result as a list of IDs must use `["input_ids"]`.

**bitsandbytes 0.46 to 0.50**
bitsandbytes Foundation. May 2025 to **[2026]** August 2026.
Releases: <https://github.com/bitsandbytes-foundation/bitsandbytes/releases> · Docs: <https://huggingface.co/docs/bitsandbytes>
The library that gives NF4 and the paged 8-bit optimizers. The important changes:
- 0.46 (May 2025): `torch.compile` works with 4-bit layers without graph breaks.
- 0.47 (August 2025): FSDP2 support for 4-bit parameters (`Params4bit`). This updates the FSDP-QLoRA path in the original "Tutorials" list.
- 0.48 (September 2025): Intel GPU and Intel Gaudi support. A faster 4-bit dequantization kernel on NVIDIA GPUs.
- 0.49 (December 2025): experimental AMD ROCm wheels and macOS (Apple Silicon) wheels. On Apple Silicon, 4-bit and 8-bit work but use slow implementations.
- 0.50 (**[2026]** July 2026): new 4-bit GEMM kernels for inference on CUDA and more ROCm and Apple Silicon support.
QLoRA is no longer limited to NVIDIA GPUs.

**TRL `SFTTrainer`: completion-only and assistant-only loss**
Hugging Face. `assistant_only_loss` added in TRL v0.19 (June 2025). TRL v1.0 released **[2026]** 31 March 2026.
SFT docs: <https://huggingface.co/docs/trl/main/en/sft_trainer> · TRL v1.0 blog: <https://huggingface.co/blog/trl-v1>
The manual `-100` masking in Section 4 of the original guide is now built into `SFTTrainer`. For a prompt-completion dataset, the loss is on the completion only by default (`completion_only_loss=True`). For a chat dataset, set `assistant_only_loss=True`. This needs a chat template with `{% generation %}` markers, and TRL adds them automatically for known families such as Qwen3. Many older tutorials use `DataCollatorForCompletionOnlyLM`; the current docs do not use it. TRL v1.0 marks the start of semantic versioning for the stable trainers.

**Unsloth: gpt-oss support and Unsloth Studio**
Unsloth AI. gpt-oss fine-tuning: August 2025. Unsloth Studio (beta): **[2026]** 17 March 2026.
Repository: <https://github.com/unslothai/unsloth> · Studio: <https://unsloth.ai/docs/new/studio>
Unsloth gives QLoRA notebooks for the models in the original guide (Qwen3, Gemma 3, Gemma 4) and for gpt-oss. Unsloth Studio is an open-source web UI to train and run models, and to make datasets from PDF, CSV, and DOCX files. Use the Unsloth notebooks to compare speed and memory with the plain Transformers code in the original guide.

---

## Changes to earlier advice

1. **Claim (Section 4 and Section 6): `apply_chat_template(messages, tokenize=False, add_generation_prompt=True)` with no other arguments.**
   For Qwen3, the default is `enable_thinking=True`. A summary dataset has no reasoning text. Pass `enable_thinking=False` in training and in generation, so that both use the same template. If you do not, the prompt at inference can differ from the prompt in training.

2. **Claim (Section 6): `do_sample=False` (greedy decoding).**
   The Qwen3 model card says: "DO NOT use greedy decoding" in thinking mode, because it can cause repetition. For non-thinking mode, it suggests `temperature=0.7`, `top_p=0.8`, `top_k=20`. Greedy decoding is still useful for a repeatable ROUGE comparison. Note this trade-off when you report results.

3. **Claim (Section 4): the manual tokenize function that masks the prompt with `-100`.**
   The code is still correct and is good for teaching. For production, TRL `SFTTrainer` now does the same masking by default. Use it to remove a common source of bugs.

4. **Claim (Section 3): `r=16`, `lora_alpha=32`, all seven projection modules, `learning_rate=2e-4`.**
   "LoRA Without Regret" supports the use of all modules, and in particular the MLP modules (`gate_proj`, `up_proj`, `down_proj`). Do not reduce the targets to attention only (`q_proj`, `v_proj`) to save memory. The paper also shows that the LoRA learning rate must be about 10 times a full fine-tuning learning rate. Do not copy a full fine-tuning learning rate (for example 2e-5) into a LoRA run.

5. **Claim (Section 5): `per_device_train_batch_size=1`, `gradient_accumulation_steps=8`.**
   A small effective batch is good for LoRA. "LoRA Without Regret" shows that LoRA can lose more than full fine-tuning at large batch sizes. If you increase the effective batch size, measure the loss again.

6. **Claim (Model choice): "A 7B model in 4 bits fits in 16 GB" is the upper limit on a T4.**
   MoE models change this. Unsloth reports QLoRA on gpt-oss-20b in about 14 GB. Training is slow on a T4, but it is possible.
