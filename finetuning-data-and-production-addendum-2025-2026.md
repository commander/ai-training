# Addendum (2025–2026): Fine-tuning datasets and production

**Created/updated:** 2026-10-10

This file extends `finetuning-data-and-production.md`. It lists 2025–2026 work that updates or extends that guide. The check was done on 2026-10-10. Only peer-reviewed papers, official lab reports, research from strong labs, and widely used open-source tools are included. Items from 2026 are marked **[2026]**.

---

## Part A and "How much data?" — small, high-quality datasets

**s1: Simple test-time scaling**
Niklas Muennighoff, Zitong Yang, Weijia Shi, et al. (Stanford University and others). arXiv, January 2025.
<https://arxiv.org/abs/2501.19393>
The authors select 1,000 questions with reasoning traces (s1K) by three criteria: difficulty, diversity, and quality. SFT on this small set gives a strong reasoning model from Qwen2.5-32B-Instruct. This extends the LIMA result in the original guide from instruction following to reasoning. The three selection criteria are a good checklist for Part D.

**LIMO: Less is More for Reasoning**
Yixin Ye, Zhen Huang, Yang Xiao, et al. COLM 2025. arXiv, February 2025.
<https://arxiv.org/abs/2502.03387>
SFT on a small, carefully selected set gives 63.3% on AIME24 and 95.6% on MATH500. It uses about 1% of the training data of earlier methods. The authors state that a small set is enough when the base model already has the domain knowledge from pretraining. This supports the "behavior, not facts" rule in Part A.

## Part B — new open datasets and data recipes

**OpenThoughts: Data Recipes for Reasoning Models**
Etash Guha, Ryan Marten, Sedrick Keh, et al. (OpenThoughts team: Stanford, UC Berkeley, and others). arXiv, June 2025.
<https://arxiv.org/abs/2506.04178>
The team runs more than 1,000 controlled experiments on each step of its synthetic data pipeline. The result is OpenThoughts3 (1.2M examples, with QwQ-32B as the teacher). The OpenThoughts3-7B model trained on it beats DeepSeek-R1-Distill-Qwen-7B by 15 to 20 points on AIME 2025, LiveCodeBench, and GPQA Diamond. Use this paper as a reference for Part C, Step 6 (synthetic generation): it measures which pipeline choices have an effect.

**Olmo 3**
Team Olmo (Allen Institute for AI). Technical report, arXiv, December 2025.
<https://arxiv.org/abs/2512.13961> · Example dataset: <https://huggingface.co/datasets/allenai/Dolci-Think-SFT-7B>
Fully open 7B and 32B models. AI2 releases every stage, checkpoint, and data point, including the post-training data (the "Dolci" SFT, DPO, and RL sets). This is a complete, open example of SFT → preference tuning → RL with verifiable rewards. Add the Dolci datasets to the Part B table for SFT, preference, and RL labs.

## Part C, Step 3 — document parsing

**Docling: An Efficient Open-Source Toolkit for AI-driven Document Conversion**
Nikolaos Livathinos, Christoph Auer, Maksym Lysak, et al. (IBM Research). AAAI 2025 Workshop on Open-Source AI for Mainstream Use. arXiv, January 2025.
<https://arxiv.org/abs/2501.17887>
The technical report for Docling, which the original guide recommends. It describes the layout and table-structure models in the pipeline. Read it to understand which document parts can fail.

**Granite-Docling-258M**
IBM. Hugging Face model, released September 2025. Apache 2.0 license.
<https://huggingface.co/ibm-granite/granite-docling-258M>
A small vision-language model that converts a page image into structured document output (text, tables, code, formulas). It runs in Docling as an alternative to the standard pipeline. Try it on the pages that fail most often (tables and multi-column pages).

## Part E, Stage 4 — SFT vs RL and forgetting

**SFT Memorizes, RL Generalizes: A Comparative Study of Foundation Model Post-training**
Tianzhe Chu, Yuexiang Zhai, Jihan Yang, et al. arXiv, January 2025.
<https://arxiv.org/abs/2501.17161>
On rule and visual variants of the same task, RL with an outcome reward generalizes to new variants. SFT tends to memorize the training data. The paper also finds that SFT is still necessary before RL, because SFT makes the output format stable.

**RL's Razor: Why Online Reinforcement Learning Forgets Less**
Idan Shenfeld, Jyothish Pari, Pulkit Agrawal (MIT). arXiv, September 2025.
<https://arxiv.org/abs/2509.04259>
At the same new-task performance, RL keeps more prior ability than SFT. The amount of forgetting follows the KL divergence between the fine-tuned model and the base model, measured on the new task. This gives a simple metric to add to the regression eval in Stage 4.

## Part E, Stage 5 — RL with verifiable rewards (after SFT)

**DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning**
DeepSeek-AI. arXiv, January 2025. Published in *Nature* 645, 633–638 (September 2025).
<https://arxiv.org/abs/2501.12948>
The report that made GRPO with rule-based (verifiable) rewards the main post-training method for reasoning. It also shows that SFT on reasoning traces from a large model (distillation) gives strong small models. Both methods are now standard steps after SFT.

**DAPO: An Open-Source LLM Reinforcement Learning System at Scale**
Qiying Yu, Zheng Zhang, Ruofei Zhu, et al. (ByteDance Seed and others). arXiv, March 2025.
<https://arxiv.org/abs/2503.14476>
Four changes to GRPO, for example decoupled clip ranges and dynamic sampling. The team releases the training code (built on verl) and data. In TRL, the DAPO loss is now the default `loss_type` of `GRPOConfig`.

**Understanding R1-Zero-Like Training: A Critical Perspective (Dr. GRPO)**
Zichen Liu, Changyu Chen, Wenjun Li, et al. (Sea AI Lab and others). arXiv, March 2025.
<https://arxiv.org/abs/2503.20783>
The paper shows that the original GRPO loss has a length bias: it makes incorrect answers longer. Dr. GRPO removes this bias. It is available as `loss_type="dr_grpo"` in TRL. If response length grows during an RL run, check this first.

**Group Sequence Policy Optimization (GSPO)**
Chujie Zheng, Shixuan Liu, Mingze Li, et al. (Alibaba Qwen team). arXiv, July 2025.
<https://arxiv.org/abs/2507.18071>
GSPO uses sequence-level importance ratios and clipping, not token-level. It makes RL on MoE models more stable, and the Qwen team used it for later Qwen3 models. In TRL, set `importance_sampling_level="sequence"` in `GRPOConfig`.

**On-Policy Distillation**
Kevin Lu and others at Thinking Machines Lab. Thinking Machines blog (research report), October 2025.
<https://thinkingmachines.ai/blog/on-policy-distillation/>
The student model generates its own answers, and a teacher model scores each token (reverse KL). This combines the on-policy property of RL with the dense signal of SFT. It is a lower-cost alternative to RL when a strong teacher model is available. TRL supports this method with its GKD trainer.

**TRL v1.0 and `GRPOTrainer`**
Hugging Face. **[2026]** v1.0.0 released 31 March 2026 (v1.15 in October 2026).
Blog: <https://huggingface.co/blog/trl-v1> · GRPO docs: <https://huggingface.co/docs/trl/main/en/grpo_trainer>
TRL is the base layer in the original "Tool roles" table. v1.0 starts semantic versioning: stable trainers are in `trl`, and new methods are in `trl.experimental`. The release adds asynchronous GRPO (generation on a separate vLLM server) and reward functions that can log extra metrics. `GRPOTrainer` supports the GRPO variants above through `loss_type` and `importance_sampling_level`. Thinking Machines ("LoRA Without Regret", September 2025, <https://thinkingmachines.ai/blog/lora/>) shows that LoRA with a low rank matches full fine-tuning for RL, so a GRPO lab with LoRA on one GPU is realistic.

---

## Changes to earlier advice

1. **Claim (Stage 5): "Preference tuning (optional) … Teach this as 'what comes after SFT.' Do not build a lab for it."**
   Since DeepSeek-R1 (2025), the main step after SFT is often RL with verifiable rewards (GRPO and its variants), not only DPO. The suggested lab task ("incident text → JSON triage record") has rewards that a program can check: valid JSON, correct fields, and correct labels. With TRL `GRPOTrainer` or Unsloth and LoRA, a short GRPO lab on a 1B–4B model is now practical. Add RL with verifiable rewards to Stage 5.

2. **Claim (Part A table): preference tuning uses `prompt`, `chosen`, `rejected`.**
   This is still correct for DPO. RL with verifiable rewards needs a different format: a `prompt` and the data the reward function needs (for example, the correct answer). There is no `chosen` or `rejected` text. Add a row for this format.

3. **Claim (How much data?): "The LIMA paper got strong results with 1,000 carefully selected examples."**
   s1 and LIMO (COLM 2025) show the same result for reasoning tasks with about 1,000 or fewer examples. LIMO states that this works only when the base model already has the domain knowledge from pretraining. This supports the "behavior, not facts" rule.

4. **Claim (Stage 4): "Regression eval: `lm-evaluation-harness` checks if the model lost general ability."**
   Keep this check. RL's Razor (2025) adds a simple metric: the KL divergence between the fine-tuned and base model on the new task. A larger KL predicts more forgetting. Also, RL fine-tuning tends to forget less than SFT at the same task score.
