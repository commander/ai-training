# Addendum (2025–2026): Statistics for AI/ML, with a focus on LLM, RAG, and agent evaluation

**Created/updated:** 2026-10-10

This addendum extends `statistics.md`. That file lists statistics books, math for embeddings, statistics for model evaluation, RAG and retrieval evaluation, LLM and agent evaluation, and (in "Pass 3") some 2025–2026 papers on eval noise and agent reliability. This addendum adds other 2025 and 2026 work on the same topics. It does not repeat the items in "Pass 3". The check date is 2026-10-10.

## New papers: statistics for benchmark design and model comparison

- **Signal and Noise: A Framework for Reducing Uncertainty in Language Model Evaluation.** Heineman, Hofmann, Magnusson et al. (Allen Institute for AI and University of Washington). NeurIPS 2025. https://arxiv.org/abs/2508.13144
  The paper defines "signal" (how well a benchmark separates better and worse models) and "noise" (how much a score changes between near training steps). A higher signal-to-noise ratio gives more reliable decisions from small experiments. Three fixes help: use a continuous metric such as perplexity instead of accuracy, remove noisy subtasks, and average scores over checkpoints. It adds a design view to Miller (2024) and Wang (Dec 2025).

- **Fluid Language Model Benchmarking.** Hofmann, Heineman, Magnusson et al. (Allen Institute for AI and partners). COLM 2025. https://arxiv.org/abs/2509.11106
  This method uses item response theory (IRT) from psychometrics. It selects test items for each model, as in computerized adaptive testing. On MMLU it gives higher validity and less variance with fifty times fewer items than random sampling. Use it when a full eval run costs too much.

- **The Leaderboard Illusion.** Singh, Nan, Wang et al. (Cohere Labs and other institutions). NeurIPS 2025, Datasets and Benchmarks Track. https://arxiv.org/abs/2504.20879
  The paper finds selection bias in Chatbot Arena. Some providers test many private variants and publish only the best score. Closed models also get more battles than open models. Read it with Zheng et al. (2023) in the original file, which introduced Chatbot Arena.

## New papers: valid statistics with LLM labels

- **Can Unconfident LLM Annotations Be Used for Confident Conclusions?** Gligorić, Zrnic, Lee, Candès, Jurafsky (Stanford University). NAACL 2025. https://arxiv.org/abs/2408.15204
  The paper gives "Confidence-Driven Inference". It uses the LLM's confidence to select which items need human labels. The estimates and confidence intervals stay statistically valid, and the method needs more than 25% fewer human labels. It extends the prediction-powered inference that ARES uses (original file).

## New papers and benchmarks: RAG and retrieval evaluation

- **The Great Nugget Recall: Automating Fact Extraction and RAG Evaluation with Large Language Models.** Pradeep, Thakur, Upadhyay et al. (University of Waterloo and partners). SIGIR 2025. https://arxiv.org/abs/2504.15068
  This paper gives the AutoNuggetizer framework from the TREC 2024 RAG Track. An LLM extracts atomic facts ("nuggets") and checks if each answer contains them. Fully automatic scores agree strongly with human scores at the system level, but less at the single-topic level. It is an alternative to the RAGAS metrics in the original file.

- **MMTEB: Massive Multilingual Text Embedding Benchmark.** Enevoldsen, Chung, Kerboua et al. (MTEB community). ICLR 2025. https://arxiv.org/abs/2502.13595
  MMTEB expands MTEB (Muennighoff et al., 2022, in the original file) to more than 500 tasks in more than 250 languages. It also gives smaller benchmark subsets that keep model rankings but cost less to run.

## New benchmarks: agent evaluation

- **τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment.** Barres, Dong, Ray, Si, Narasimhan (Sierra). arXiv, Jun 2025. https://arxiv.org/abs/2506.07982 . Code: https://github.com/sierra-research/tau2-bench
  This is the next version of τ-bench in the original file. Both the agent and the simulated user use tools in a shared environment (for example, a telecom domain). It keeps the pass^k reliability metric, and it separates reasoning errors from communication errors.

- **Holistic Agent Leaderboard: The Missing Infrastructure for AI Agent Evaluation.** Kapoor, Stroebl, Kirgis et al. (Princeton University and partners). arXiv, Oct 2025. https://arxiv.org/abs/2510.11977
  HAL runs 21,730 agent rollouts over 9 models and 9 benchmarks, and it reports cost with accuracy. It found that higher reasoning effort decreased accuracy in most runs. It also releases 2.5 billion tokens of agent logs for analysis. It adds cost and log-based checks to the agent surveys in the original file.

## Changes to earlier advice

- **Claim in the original:** Zheng et al. (2023) explain when an LLM judge agrees with humans, and Chatbot Arena is a reference ranking.
  **Update:** The Leaderboard Illusion (NeurIPS 2025) shows that Arena scores can have selection bias. Do not use one public leaderboard rank as the only evidence. Run your own paired tests on your own task data.

- **Claim in the original:** τ-bench (Yao et al., 2024) introduces pass^k.
  **Update:** τ²-bench (2025) is the current version. Use it for new agent work, and keep the pass^k metric.

- **Claim in the original (implied in the recommended order):** Measure accuracy and add error bars.
  **Update:** Signal and Noise (NeurIPS 2025) shows that for decisions from small experiments, a continuous metric (for example, perplexity or bits-per-byte) can be more reliable than accuracy. Fluid Benchmarking (COLM 2025) shows that adaptive item selection can decrease variance. Use these methods together with the error-bar methods of Miller (2024).

- **Claim in the original:** MTEB (2022) is the benchmark for embedding models.
  **Update:** MMTEB (ICLR 2025) replaces it as the main multilingual benchmark, and the MTEB leaderboard now uses MMTEB benchmarks.
