# Addendum (2025–2026): Resource Guide — Iterative Depth (v2, with production fine-tuning)

**Created/updated:** 2026-10-10

This file extends `genai-resources-v2.md`. We checked the resources on 2026-10-10. Each item is from 2025 or 2026 and is not in the original file. We include only official lab or vendor reports, peer-reviewed papers, papers from strong labs, widely used tools, and courses from established providers. Items from 2026 have the tag **[2026]**.

---

# Pass 1, Pass 2, and Pass 3 (shared with v1)

Sections Pass 1 to "Practical notes" in `genai-resources-v2.md` are the same as in `genai-resources.md`. The new items and the outdated-resource table for those sections are in `genai-resources-addendum-2025-2026.md`. Read that file for these sections. This file does not repeat them.

This file adds items only for the section "Unit 5 extension — Fine-tuning for production".

---

# Unit 5 extension — Fine-tuning for production

## Tool roles

- **Hugging Face TRL — "LoRA Without Regret" recipe**
  Hugging Face. TRL documentation. 2025.
  https://huggingface.co/docs/trl/main/en/lora_without_regret
  TRL now has a page that turns the Thinking Machines LoRA findings into `SFTTrainer` and `GRPOTrainer` settings. It shows the base layer (TRL) with current LoRA defaults. Unsloth and Axolotl sit on top of it.

## Stage 1 — Decide if you need to fine-tune

- **Qwen3 Technical Report**
  Qwen Team, Alibaba. arXiv. May 2025.
  https://arxiv.org/abs/2505.09388
  Qwen3 has dense models from 0.6B to 32B and MoE models. The small sizes fit a free T4 for the lab. The Unsloth fine-tuning guide now uses Qwen3 in its examples. Use the report to pick a base model size before you decide to fine-tune.

- **Gemma 3 Technical Report**
  Gemma Team, Google DeepMind. arXiv. Mar 2025.
  https://arxiv.org/abs/2503.19786
  Gemma 3 has open models from 1B to 27B, with up to 128K context. It is a second base-model family for a "same data, two bases" comparison.

## Stage 3 — Reproducible training

- **LoRA Without Regret**
  John Schulman and Thinking Machines Lab. Connectionism blog. Sep 2025.
  https://thinkingmachines.ai/blog/lora/
  Controlled sweeps show when LoRA matches full fine-tuning. Apply LoRA to all layers, including MLP and MoE layers. Use a learning rate about 10 times the full fine-tuning rate. Put these values in the Axolotl YAML file and record them with each run.

## Stage 5 — Preference tuning (optional)

- **DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning**
  DeepSeek-AI. Nature, vol. 645 (2025). arXiv Jan 2025.
  https://arxiv.org/abs/2501.12948
  It shows reinforcement learning with rule-based (verifiable) rewards and the GRPO algorithm. This is now the main "what comes after SFT and DPO" topic. Teach it as a concept only, as the original file says for DPO.

- **TRL GRPOTrainer documentation**
  Hugging Face. TRL documentation. 2025.
  https://huggingface.co/docs/trl/main/en/grpo_trainer
  The reference for GRPO with reward functions in TRL. Use it if a participant asks how to try RL fine-tuning after SFT.

## Stage 4 — Evaluation

- **Demystifying Evals for AI Agents** **[2026]**
  Anthropic Engineering. Jan 2026.
  https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
  It separates capability evals from regression evals and explains pass@k and pass^k for non-deterministic outputs. Use these terms when you compare a fine-tuned adapter with the base-model baseline.

---

# Outdated or changed resources

For the shared sections (Pass 1 to Practical notes), see the table in `genai-resources-addendum-2025-2026.md`. For the Unit 5 extension:

| Original item | Status on 2026-10-10 | Action |
|---|---|---|
| Qwen 2.5 3B as the default lab base | Still works. Unsloth guide examples now use Qwen3. The original file already notes a warning on 4-bit QLoRA for Qwen 3.5. | Keep Qwen 2.5 3B as the safe default. Test a small Qwen3 model as an option before Day 6. |
| DPO as the only "after SFT" topic | RL with verifiable rewards (GRPO) is now the common next step. | Add one slide on GRPO next to DPO, using the items in Stage 5 above. |
