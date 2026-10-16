# Addendum (2025–2026): Reasoning and Thinking Models

**Created/updated:** 2026-10-10

This file extends `reasoning-models-reading-list.md`. It lists 2025–2026 work that is not in that list. The check date is 2026-10-10. Each item was confirmed on arXiv, the journal site, or the publisher page. Items from 2026 have the tag **[2026]**. The sections follow the order of the original list.

## 1. Start here (explainers)

- **Build a Reasoning Model (From Scratch)** **[2026]**
  Sebastian Raschka. Manning Publications. June 2026.
  https://www.manning.com/books/build-a-reasoning-model-from-scratch
  This book follows the "Understanding Reasoning LLMs" article in the original list. It starts with a pre-trained LLM. Then it builds evaluation with verifiers, inference-time methods, reinforcement learning, and distillation in Python code. Read it after Section 1 if you want to write the code yourself.

## 2. How models learn to reason (training)

**Peer-reviewed version of DeepSeek-R1**

- **DeepSeek-R1 incentivizes reasoning in LLMs through reinforcement learning**
  Guo et al. (DeepSeek-AI). Nature 645, 633–638. September 2025.
  https://doi.org/10.1038/s41586-025-09422-z
  This is the peer-reviewed version of the DeepSeek-R1 paper in the original list. It adds reviewer questions, more safety tests, and the training cost. Cite this version when you need a peer-reviewed source.

**Open training reports from model labs**

- **Kimi k1.5: Scaling Reinforcement Learning with LLMs**
  Kimi Team (Moonshot AI). January 2025.
  https://arxiv.org/abs/2501.12599
  This report came out at the same time as DeepSeek-R1. It trains with RL and long context, without Monte Carlo tree search or a process reward model. It also shows "long-to-short" methods that make the thinking shorter.

- **DAPO: An Open-Source LLM Reinforcement Learning System at Scale**
  Yu et al. (ByteDance Seed and Tsinghua AIR). March 2025.
  https://arxiv.org/abs/2503.14476
  DAPO gives four changes to GRPO, the RL method in DeepSeek-R1. The code, data, and training recipe are open. It is a good next paper if you want to reproduce R1-style training.

- **Qwen3 Technical Report**
  Yang et al. (Qwen Team, Alibaba). May 2025.
  https://arxiv.org/abs/2505.09388
  One Qwen3 model has a "thinking" mode and a "non-thinking" mode. It also has a "thinking budget" that limits the number of thinking tokens. This is a product version of the length control in s1, which the original list includes.

- **gpt-oss-120b & gpt-oss-20b Model Card**
  OpenAI. August 2025.
  https://arxiv.org/abs/2508.10925
  These are OpenAI open-weight reasoning models. The user can set the reasoning effort to low, medium, or high. The card shows how accuracy changes with each effort level.

- **Olmo 3**
  Team Olmo (Allen Institute for AI). December 2025 (revised 2026).
  https://arxiv.org/abs/2512.13961
  Olmo 3 Think 32B is a fully open thinking model. The data, code, and all checkpoints are public. Use it when you must study every stage of training, which closed and open-weight reports do not allow.

- **DeepSeekMath-V2: Towards Self-Verifiable Mathematical Reasoning**
  Shao et al. (DeepSeek-AI). November 2025.
  https://arxiv.org/abs/2511.22570
  A final-answer reward cannot check a written proof. This model trains a proof verifier, and then trains the generator to find and fix its own errors. It extends "Let's Verify Step by Step" from the original list to full proofs.

- **DeepSeek-V4: Towards Highly Efficient Million-Token Context Intelligence** **[2026]**
  DeepSeek-AI. April 2026 (preview version).
  https://arxiv.org/abs/2606.19348
  DeepSeek-V4 is one model with several reasoning effort modes. It replaces the separate R-series reasoning models. The report shows that "reasoning model" is now a mode of a general model, not a separate model family.

**What reinforcement learning really changes**

- **Does Reinforcement Learning Really Incentivize Reasoning Capacity in LLMs Beyond the Base Model?**
  Yue et al. (Tsinghua University). NeurIPS 2025. April 2025.
  https://arxiv.org/abs/2504.13837
  RL-trained models win at pass@1. But the base models get a higher pass@k when k is large. The authors conclude that current RL mostly makes good answers more likely. It does not teach new reasoning patterns.

- **ProRL: Prolonged Reinforcement Learning Expands Reasoning Boundaries in Large Language Models**
  Liu et al. (NVIDIA). May 2025.
  https://arxiv.org/abs/2505.24864
  This paper gives the opposite result to Yue et al. With much longer and more stable RL training, the model solves problems that the base model fails at any k. Read both papers together. The answer depends on the amount of RL training and the type of task.

- **Understanding R1-Zero-Like Training: A Critical Perspective**
  Liu et al. (Sea AI Lab and National University of Singapore). March 2025.
  https://arxiv.org/abs/2503.20783
  The base model DeepSeek-V3-Base already shows the "aha moment" before RL. The paper also finds a bias in GRPO that makes wrong answers longer. It gives a fixed method, Dr. GRPO.

- **Spurious Rewards: Rethinking Training Signals in RLVR**
  Shao et al. (University of Washington and Ai2). June 2025.
  https://arxiv.org/abs/2506.10947
  For Qwen2.5-Math-7B, RL with random rewards gave +21.4 points on MATH-500. Correct rewards gave +29.1 points. The effect did not transfer to other model families. Do not trust an RL result that you see on only one model family.

- **Reasoning or Memorization? Unreliable Results of Reinforcement Learning Due to Data Contamination**
  Wu et al. AAAI 2026. July 2025.
  https://arxiv.org/abs/2507.10532
  The authors find that Qwen2.5 models probably saw common math benchmarks during pretraining. On new, clean arithmetic problems, only correct rewards improved the model. This explains part of the result in "Spurious Rewards."

## 3. Limits of more thinking time

The original list says that more thinking time gives better results (o1, Snell et al., s1). These papers show where that is not true.

- **Inverse Scaling in Test-Time Compute**
  Gema et al. (Anthropic Fellows Program and others). TMLR, December 2025. July 2025.
  https://arxiv.org/abs/2507.14417
  The authors built tasks where longer reasoning gives lower accuracy. They found five failure modes. Examples are distraction by irrelevant data and a move to spurious correlations.

- **The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity**
  Shojaee et al. (Apple). NeurIPS 2025. June 2025.
  https://arxiv.org/abs/2506.06941
  The paper uses puzzles with a controlled difficulty. Above a certain difficulty, accuracy falls to zero. The models also use fewer thinking tokens on the hardest puzzles. Also read the reply by Lawsen (https://arxiv.org/abs/2506.09250). It says that output token limits and some impossible puzzles caused part of the result.

- **A Sober Look at Progress in Language Model Reasoning: Pitfalls and Paths to Reproducibility**
  Hochlehnert et al. COLM 2025. April 2025.
  https://arxiv.org/abs/2504.07086
  Scores on small benchmarks such as AIME'24 change by several points with the seed, the hardware, or the prompt format. Under one standard setup, most RL methods gave much smaller gains than their papers claimed. This links to "Defeating Nondeterminism in LLM Inference" in the original list.

- **Stop Overthinking: A Survey on Efficient Reasoning for Large Language Models**
  Sui et al. (Rice University and others). TMLR 2025. March 2025.
  https://arxiv.org/abs/2503.16419
  A survey of methods that make reasoning shorter at the same accuracy. Use it as a map of the work on thinking-length control after s1.

## 4. Limits of the visible "thinking"

These papers extend "Reasoning Models Don't Always Say What They Think" from the original list.

- **Monitoring Reasoning Models for Misbehavior and the Risks of Promoting Obfuscation**
  Baker et al. (OpenAI). March 2025.
  https://arxiv.org/abs/2503.11926
  A weaker model (GPT-4o) that reads the chain of thought found reward hacking by o3-mini better than a monitor that saw only actions. But when the training also punished "bad thoughts," the model learned to hide its intent and continued to cheat.

- **Chain of Thought Monitorability: A New and Fragile Opportunity for AI Safety**
  Korbak et al. (41 authors from OpenAI, Google DeepMind, Anthropic, Meta, UK AISI, and others). July 2025.
  https://arxiv.org/abs/2507.11473
  This position paper says that the chain of thought is a useful but fragile safety signal. Labs can lose it if training puts direct pressure on the thinking text. The authors ask developers to measure and keep monitorability.

- **Reasoning Models Struggle to Control their Chains of Thought** **[2026]**
  Chen et al. (NYU, UCL, UPenn, and OpenAI). March 2026.
  https://arxiv.org/abs/2603.05706
  The CoT-Control suite asks models to obey rules inside their thinking text. Claude Sonnet 4.5 obeyed in its chain of thought only 2.7% of the time, but 61.9% of the time in its final output. Low control is good for monitoring, because the model cannot easily hide its thinking.

## 5. Changes to earlier advice

- **Original claim (Section 1, OpenAI o1):** "more thinking time gives better results."
  **Update:** This is true on average for math and code. But it is not true for all tasks. Longer thinking can lower accuracy (Gema et al., 2025). On very hard puzzles, models can stop and use fewer tokens (Shojaee et al., 2025). Test your own task at more than one thinking budget.

- **Original claim (Section 3, DeepSeek-R1):** "The model learns to think longer and to check its own work."
  **Update:** Part of this behavior is already in the base model before RL (Liu et al., 2025). Part of the longer length comes from a bias in GRPO. Whether RL adds new ability, or only makes known answers more likely, is still an open question (Yue et al. versus Liu et al. of NVIDIA, 2025).

- **Original structure (Sections 1 and 3):** the list treats reasoning models as a separate model type.
  **Update:** Since mid-2025, most labs ship one model with a thinking mode or a reasoning-effort setting (Qwen3, gpt-oss, DeepSeek-V4). Learn how to set the thinking budget, not only how to select a "reasoning model."

- **Original list (Section 3):** RL results on public math benchmarks.
  **Update:** Treat RL gains on AIME or MATH-500 with care. Contamination (Wu et al., AAAI 2026), random-reward gains (Shao et al., 2025), and seed noise (Hochlehnert et al., COLM 2025) can each explain a gain of several points. Check results on more than one model family and on clean data.

## If you read only three new items

1. DeepSeek-R1 in Nature (2025), if you have not read the arXiv version.
2. Does Reinforcement Learning Really Incentivize Reasoning Capacity in LLMs Beyond the Base Model? (Yue et al., 2025), together with ProRL.
3. Chain of Thought Monitorability (Korbak et al., 2025).
