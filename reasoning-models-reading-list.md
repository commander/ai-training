# Reading List: Reasoning and Thinking Models

This list is in a suggested reading order, from easy to difficult. The links come from knowledge up to mid-2026. Some newer work can be missing.

## 1. Start here (explainers, no math needed)

- [Understanding Reasoning LLMs](https://magazine.sebastianraschka.com/p/understanding-reasoning-llms) (Sebastian Raschka)
  A clear overview of the four main methods to build reasoning models.

- [Why We Think](https://lilianweng.github.io/posts/2025-05-01-thinking/) (Lilian Weng, 2025)
  A longer review of "thinking time" research, with links to many papers.

- [Learning to Reason with LLMs](https://openai.com/index/learning-to-reason-with-llms/) (OpenAI, 2024)
  The o1 announcement. It shows that more thinking time gives better results.

## 2. Foundation papers ("think step by step")

- [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903) (Wei et al., 2022)
  If a model writes intermediate steps, it solves more problems correctly.

- [Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916) (Kojima et al., 2022)
  The phrase "Let's think step by step" alone improves results.

- [Self-Consistency Improves Chain of Thought Reasoning](https://arxiv.org/abs/2203.11171) (Wang et al., 2022)
  Sample many reasoning paths, then select the most common answer.

## 3. How models learn to reason (training)

- [Let's Verify Step by Step](https://arxiv.org/abs/2305.20050) (Lightman et al., OpenAI, 2023)
  Give feedback on each reasoning step, not only on the final answer.

- [DeepSeek-R1](https://arxiv.org/abs/2501.12948) (DeepSeek, 2025)
  An open description of how a reasoning model is trained with reinforcement learning. The model learns to think longer and to check its own work.

- [Scaling LLM Test-Time Compute Optimally](https://arxiv.org/abs/2408.03314) (Snell et al., 2024)
  Sometimes more thinking time is better than a larger model.

- [s1: Simple Test-Time Scaling](https://arxiv.org/abs/2501.19393) (Muennighoff et al., 2025)
  A small, easy-to-follow experiment on how to control thinking length.

## 4. Limits of the visible "thinking"

- [Reasoning Models Don't Always Say What They Think](https://arxiv.org/abs/2505.05410) (Anthropic, 2025)
  The written thinking does not always show the real reason for the answer.

## Related background

- [The Curious Case of Neural Text Degeneration](https://arxiv.org/abs/1904.09751) (Holtzman et al., 2019)
  The paper that introduced nucleus (top-p) sampling.

- [Defeating Nondeterminism in LLM Inference](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/) (Thinking Machines Lab, 2025)
  Explains why temperature 0 does not give identical outputs.

## If you read only three

1. Understanding Reasoning LLMs (Raschka)
2. DeepSeek-R1 (sections 1 and 2)
3. Reasoning Models Don't Always Say What They Think (Anthropic)
