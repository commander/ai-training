# Addendum (2025–2026): Context rot in long-context LLMs

**Created/updated:** 2026-10-10

This file extends `context-rot.md`. We checked for new work on 2026-10-10. The list contains only peer-reviewed papers, papers from strong labs, official lab reports, and recognized benchmarks. It does not repeat items that the original file lists.

## New papers

**Context Length Alone Hurts LLM Performance Despite Perfect Retrieval**
Yufeng Du, Minyang Tian, Srikanth Ronanki, Subendhu Rongali, Sravan Bodapati, Aram Galstyan, Azton Wells, Roy Schwartz, Eliu A. Huerta, Hao Peng. Findings of EMNLP 2025. October 2025.
https://arxiv.org/abs/2510.05381
The authors test 5 LLMs on math, QA, and code tasks. Accuracy drops by 13.9% to 85% as input length increases, even when retrieval of the evidence is perfect. The drop occurs when the extra tokens are whitespace, and also when the extra tokens are masked. This result separates "length degradation" from "distractor" effects in the Chroma report. The authors also give a simple fix: tell the model to recite the evidence first, then solve the task.

**Positional Biases Shift as Inputs Approach Context Window Limits**
Blerta Veseli, Julian Chibane, Mariya Toneva, Alexander Koller (Saarland University, MPI). COLM 2025. August 2025.
https://arxiv.org/abs/2508.07479
The authors measure input length as a fraction of each model's context window. The "lost in the middle" U-curve is strongest when the input fills up to about 50% of the window. Above 50%, primacy bias becomes weaker and recency bias stays. Performance then depends on the distance of the fact from the end of the input. See "Changes to earlier advice".

**Recursive Language Models**
Alex L. Zhang, Tim Kraska, Omar Khattab (MIT CSAIL). arXiv preprint. December 2025.
https://arxiv.org/abs/2512.24601
The model does not read the full long prompt in one pass. The prompt is a variable in a Python REPL. The model writes code to inspect and split the prompt, and it calls itself on the parts. The authors present this method as a direct answer to context rot. It handles inputs up to two orders of magnitude larger than the model window.

**Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models (ACE)**
Qizheng Zhang et al. (Stanford University, SambaNova Systems, UC Berkeley). ICLR 2026. October 2025 (arXiv).
https://arxiv.org/abs/2510.04618
ACE keeps agent context as a "playbook" that it updates in small steps: generation, reflection, and curation. The paper names "context collapse": repeated LLM rewrites of a long context lose detail over time. This is a second failure mode for long agent runs, next to the length effect in the original file.

## New benchmarks and datasets

**LongBench v2: Towards Deeper Understanding and Reasoning on Realistic Long-context Multitasks**
Yushi Bai et al. (Tsinghua University, Zhipu AI). ACL 2025. December 2024 (arXiv), July 2025 (ACL).
https://arxiv.org/abs/2412.15204
The benchmark has 503 hard multiple-choice questions. Contexts go from 8k to 2M words, in six task types. Human experts get 53.7% under a 15-minute limit. It tests reasoning over long input, not only retrieval, so it is a harder check than NIAH and RULER.

**HELMET: How to Evaluate Long-Context Language Models Effectively and Thoroughly**
Howard Yen et al. (Princeton University, Intel). ICLR 2025.
https://arxiv.org/abs/2410.02694
HELMET has seven application task types: synthetic recall, long-document QA, summarization, many-shot ICL, RAG, re-ranking, and generation with citations. The authors show that NIAH-style synthetic scores do not predict downstream results well. Use HELMET in place of NIAH alone when you compare models.

**OpenAI MRCR and Graphwalks**
OpenAI. Released with GPT-4.1. April 2025.
https://huggingface.co/datasets/openai/mrcr
https://huggingface.co/datasets/openai/graphwalks
MRCR hides several identical requests in a long synthetic chat. The model must find a specified instance, for example "the second poem about tapirs". Graphwalks gives a graph as an edge list and asks for a BFS or parent search. Both go up to 1M tokens. Many 2025–2026 model reports use them as long-context evals.

**HELM Long Context leaderboard**
Yifan Mai, Johan Attia (Stanford CRFM). September 2025.
https://crfm.stanford.edu/2025/09/29/helm-long-context.html
This is a reproducible leaderboard. It uses RULER SQuAD, RULER HotPotQA, ∞Bench En.MC, ∞Bench En.Sum, and OpenAI-MRCR. At launch, the best MRCR score was only 0.256. This result agrees with the Chroma finding that all models degrade at long input.

## Official guidance

**Effective context engineering for AI agents**
Anthropic Applied AI team. Anthropic Engineering blog. September 2025.
https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
Anthropic uses the term "context rot" and treats context as a limited "attention budget". The post gives methods to keep context small: compaction, structured note-taking, just-in-time retrieval with tools, and sub-agents. It is the main vendor guide that turns the context-rot findings into agent design rules.

## Reference for an item in the original file

The original file lists NoLiMa without a citation. The full reference is: **NoLiMa: Long-Context Evaluation Beyond Literal Matching**, Ali Modarressi, Hanieh Deilamsalehy, Franck Dernoncourt, Trung Bui, Ryan A. Rossi, Seunghyun Yoon, Hinrich Schütze (LMU Munich, Adobe Research). ICML 2025. https://arxiv.org/abs/2502.05167. Code: https://github.com/adobe-research/NoLiMa.

## Changes to earlier advice

1. **Claim:** "Models use facts near the start and end of a long prompt much better than facts in the middle (the U-shaped curve)." **Update:** Veseli et al. (COLM 2025) show that the U-curve applies mainly when the input fills up to about 50% of the window. Near the window limit, facts close to the end of the input are used best. Put the most important facts at the end of very long prompts, not only at the start.
2. **Claim:** Distractors make context rot worse. **Update:** Du et al. (EMNLP Findings 2025) show that length alone reduces accuracy, with no distractors and with masked padding. Removing irrelevant text does not fully solve the problem. Shorten the input, or make the model recite the evidence before it answers.
3. **Claim (implied):** Context rot is a property of the model that you can only measure. **Update:** 2025–2026 work treats it as a design constraint. Anthropic's context-engineering guide, Recursive Language Models, and ACE give practical controls: compaction, sub-agents, programmatic access to long input, and incremental context updates.
4. **Reading order:** Add Du et al. after the Chroma report. Add Veseli et al. after Liu et al. Use HELMET or LongBench v2, not NIAH alone, to measure effective context length.
