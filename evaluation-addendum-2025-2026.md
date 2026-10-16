# Addendum (2025–2026): Correct Evaluation of LLM Systems

**Created/updated:** 2026-10-10

This file extends `evaluation.md`. It lists 2025–2026 work that updates the methods, benchmarks, and study list in that file. The check date is 2026-10-10. Each item was confirmed on arXiv, a conference site, or the official publisher page. Items from 2026 have the tag **[2026]**.

---

## 1. New papers on evaluation method

These papers support the four claims in Section 1 of the original file (construct validity, no contamination, isolation, reliability).

- **Measuring what Matters: Construct Validity in Large Language Model Benchmarks**
  Bean et al. (Oxford Internet Institute and others, 42 authors). NeurIPS 2025, Datasets and Benchmarks Track. November 2025.
  https://arxiv.org/abs/2511.04703
  The authors reviewed 445 LLM benchmarks. Many benchmarks do not define the skill that they measure. Many do not report statistical tests. The paper gives eight recommendations. Use it as a full reference for claim 1 ("You measured the skill you care about").

- **Position: Don't Use the CLT in LLM Evals With Fewer Than a Few Hundred Datapoints**
  Bowyer, Aitchison, Ivanova. ICML 2025 (Spotlight Position Paper). March 2025.
  https://arxiv.org/abs/2503.01747
  Intervals from the central limit theorem (CLT) are too narrow when n is small. Many LLM benchmarks have only a few hundred items, or fewer. The authors recommend other frequentist methods or Bayesian methods for small n. They give a Python library (`bayes_evals`). This updates Section 4G of the original file (see "Changes to earlier advice").

- **Signal and Noise: A Framework for Reducing Uncertainty in Language Model Evaluation**
  Heineman et al. Allen Institute for AI (Ai2). August 2025.
  https://arxiv.org/abs/2508.13144
  The paper defines two properties of a benchmark. "Signal" is how well the benchmark separates good models from bad models. "Noise" is how much the score changes between training checkpoints or seeds. A benchmark with a high signal-to-noise ratio gives more reliable decisions. This gives a number for claim 4 (reliability) in the original file.

- **A Sober Look at Progress in Language Model Reasoning: Pitfalls and Paths to Reproducibility**
  Hochlehnert et al. COLM 2025. April 2025.
  https://arxiv.org/abs/2504.07086
  On small math benchmarks such as AIME, a change of seed, hardware, or prompt format moves scores by several points. Under a standard setup, most reported gains from reinforcement learning were much smaller than the papers claimed. The paper gives a standard evaluation procedure. It is a good real example of the "do not rank models to the third decimal" rule in Section 4G.

- **The Leaderboard Illusion**
  Singh et al. (Cohere Labs and others). April 2025.
  https://arxiv.org/abs/2504.20879
  The paper studies the Chatbot Arena (LMArena) leaderboard. Some providers tested many private model variants and published only the best score. Some providers also got more data from the arena than others. This is the "multiple-hypothesis caution" in Section 2, at the scale of a public leaderboard.

- **Establishing Best Practices for Building Rigorous Agentic Benchmarks**
  Zhu et al. (25 authors from several universities and labs). July 2025.
  https://arxiv.org/abs/2507.02825
  The paper gives the Agentic Benchmark Checklist (ABC). It separates "task validity" (the task needs the skill) from "outcome validity" (the grader detects success correctly). The authors found task and grader errors in known agent benchmarks. These errors moved scores by up to 100% in relative terms. Use it with the Section 6 checklist when the system under test is an agent.

## 2. New practical guides

- **Demystifying evals for AI agents** **[2026]**
  Grace, Hadfield, Olivares, De Jonghe. Anthropic Engineering blog. January 2026.
  https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
  The post defines the parts of an agent eval: tasks, trials, graders, transcripts, outcomes, and harness. It recommends that you start with 20–50 tasks from real user failures. It also recommends code graders first, then LLM rubric graders, then human calibration. It agrees with Section 4C of the original file and adds the agent case.

- **Build a Reasoning Model (From Scratch)** **[2026]**
  Sebastian Raschka. Manning Publications. June 2026.
  https://www.manning.com/books/build-a-reasoning-model-from-scratch
  Part of the book shows how to build verifier-based evaluation for math answers in code. This is a hands-on example of the "prefer checks a script can run" rule in Section 4C.

## 3. New benchmarks and datasets

Copy the method of these benchmarks, not only the task. Section 5 of the original file gives this advice.

**General knowledge and reasoning**

- **Humanity's Last Exam (HLE)**
  Phan et al. Center for AI Safety and Scale AI (more than 1,000 contributors). January 2025.
  https://arxiv.org/abs/2501.14249
  HLE has about 2,500 expert-written questions. The authors removed questions that frontier models answered correctly at build time. It also reports calibration error, which links to the calibration item in Section 2.

**Long context**

- **NoLiMa: Long-Context Evaluation Beyond Literal Matching**
  Modarressi et al. (LMU Munich and Adobe Research). ICML 2025. February 2025.
  https://arxiv.org/abs/2502.05167
  In NoLiMa, the question and the "needle" have very little word overlap. The model must make a link, not find matching words. Scores fall sharply as context gets longer. It is a controlled design in the same family as Lost in the Middle and RULER, which the original file lists.

**Coding and software agents**

- **LiveCodeBench Pro: How Do Olympiad Medalists Judge LLMs in Competitive Programming?**
  Zheng et al. June 2025.
  https://arxiv.org/abs/2506.11928
  The benchmark adds new contest problems over time to reduce contamination. Olympiad medalists label each problem and each failure. This is a good example of a failure taxonomy (Section 4H) built by experts.

- **SWE-Bench Pro: Can AI Agents Solve Long-Horizon Software Engineering Tasks?**
  Deng et al. Scale AI. September 2025.
  https://arxiv.org/abs/2509.16941
  SWE-Bench Pro uses longer tasks and has a held-out set and a private commercial set. The aim is to reduce contamination from public GitHub code. Scores are much lower than on SWE-bench Verified.

- **Terminal-Bench: Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces** **[2026]**
  Merrill et al. ICLR 2026. January 2026.
  https://arxiv.org/abs/2601.11868
  Terminal-Bench 2.0 has 89 tasks in containers. Each task has an instruction, tests, and a human-written reference solution. Three reviewers checked each task. It shows how to build agent tasks with a grader that a script can run.

**Agents and tools**

- **τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment**
  Barres et al. Sierra. June 2025.
  https://arxiv.org/abs/2506.07982
  Both the agent and a simulated user can change the shared state with tools. The benchmark reports pass^k, which is the chance that all k trials succeed. Pass^k measures consistency, not only one lucky success.

- **BrowseComp: A Simple Yet Challenging Benchmark for Browsing Agents**
  Wei et al. OpenAI. April 2025.
  https://arxiv.org/abs/2504.12516
  The answers are short and easy to check, but hard to find on the web. This design keeps grading simple while the task stays hard.

- **Measuring AI Ability to Complete Long Software Tasks**
  Kwa et al. METR. March 2025.
  https://arxiv.org/abs/2503.14499
  The paper measures the length of tasks (in human time) that a model completes with 50% success. This "time horizon" metric lets you compare models across many years on one scale.

**Real work and rubric grading**

- **GDPval: Evaluating AI Model Performance on Real-World Economically Valuable Tasks**
  Patwardhan et al. OpenAI. October 2025.
  https://arxiv.org/abs/2510.04374
  Industry experts wrote tasks from 44 occupations. Other experts compared model output with human output in blind pairs. This is a paired comparison (Section 2) with expert judges.

- **HealthBench: Evaluating Large Language Models Towards Improved Human Health**
  Arora et al. OpenAI. May 2025.
  https://arxiv.org/abs/2505.08775
  262 physicians wrote 48,562 rubric criteria for 5,000 conversations. A model grader scores each criterion. It is a large example of the rubric and judge-agreement method in Section 4C.

**Embeddings and judges**

- **MMTEB: Massive Multilingual Text Embedding Benchmark**
  Enevoldsen et al. ICLR 2025. February 2025.
  https://arxiv.org/abs/2502.13595
  MMTEB covers more than 500 tasks in more than 250 languages, including clustering and retrieval. Use it to select the embedding model before you run UMAP and HDBSCAN (Section 3).

- **RewardBench 2: Advancing Reward Model Evaluation**
  Malik et al. Allen Institute for AI (Ai2). June 2025.
  https://arxiv.org/abs/2506.01937
  The benchmark tests reward models and LLM judges on new, unseen human prompts. Check the judge on a set like this before you trust LLM-as-judge scores (Section 2).

## 4. Changes to earlier advice

- **Original claim (Section 4G):** "Report n, and a simple interval (bootstrap is enough)."
  **Update:** For n below a few hundred, CLT-based intervals are usually too narrow (Bowyer et al., ICML 2025). For pass/fail scores with small n, use an interval made for small samples, such as a Wilson, Clopper-Pearson, or Bayesian interval. Do not assume that a simple bootstrap fixes the problem. Keep the original rule that you must report n.

- **Original claim (Section 1, claim 4):** reliability depends on "seeds, judges, splits."
  **Update:** Hochlehnert et al. (COLM 2025) and Heineman et al. (Ai2, 2025) show that seed and setup noise is often as large as the gains that papers report. Run several seeds. Report the spread. Prefer benchmarks with a high signal-to-noise ratio.

- **Original claim (Section 5):** copy the method of "BIG-bench / LiveBench."
  **Update:** Many older benchmarks are now near their maximum score for frontier models. Public leaderboards also have selection bias (Singh et al., 2025). For new work, also study benchmarks that resist contamination or are held out: HLE, LiveCodeBench Pro, SWE-Bench Pro, and Terminal-Bench.

- **Original scope (Sections 4 and 6):** the recipe assumes one input and one output.
  **Update:** For agents, grade the final state and the transcript, and run many trials per task. Use pass^k for consistency (τ²-Bench). Check the grader itself for errors (Agentic Benchmark Checklist, 2025; Anthropic, January 2026).
