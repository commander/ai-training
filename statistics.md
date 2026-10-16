Here are resources in four groups: statistics foundations, math for embeddings, retrieval and RAG evaluation, and LLM and agent evaluation. For each group, start with the first item.

**1. Statistics foundations**

- *Think Stats* (Allen Downey), free online: [greenteapress.com/wp/think-stats-3e](https://greenteapress.com/wp/think-stats-3e/). This book teaches statistics with Python code. It is a good start for engineers.
- *Seeing Theory* (Brown University): [seeing-theory.brown.edu](https://seeing-theory.brown.edu/). Interactive visual lessons on probability, distributions, and inference.
- *An Introduction to Statistical Learning* (James et al.), free: [statlearning.com](https://www.statlearning.com/). This book has a Python edition. It covers regression, classification, resampling (bootstrap, cross-validation), and model evaluation.
- *All of Statistics* (Larry Wasserman). A short but complete book on statistical inference. It is more mathematical.
- *Statistical Rethinking* (Richard McElreath). This book teaches Bayesian statistics. The author's free video lectures are on YouTube.

**2. Math for embeddings**

- *Mathematics for Machine Learning* (Deisenroth et al.), free: [mml-book.github.io](https://mml-book.github.io/). This book covers linear algebra (vectors, dot products, projections), probability, and optimization. This is the math that embeddings use.
- *Introduction to Information Retrieval* (Manning, Raghavan & Schütze), free: [nlp.stanford.edu/IR-book](https://nlp.stanford.edu/IR-book/). Chapter 8 explains retrieval metrics (precision, recall, MAP, nDCG). Chapter 6 explains the vector space model.

**3. Statistics for model evaluation (most important for your work)**

- *Adding Error Bars to Evals: A Statistical Approach to Language Model Evaluations* (Evan Miller, Anthropic, 2024): [arxiv.org/abs/2411.00640](https://arxiv.org/abs/2411.00640). It treats evals as experiments and gives formulas to analyze eval data, to measure differences between two models, and to plan eval experiments. It also gives recommendations to decrease statistical noise. Read this first.
- *With Little Power Comes Great Responsibility* (Card et al., 2020): [arxiv.org/abs/2010.06595](https://arxiv.org/abs/2010.06595). This paper explains statistical power, which you need to calculate the necessary number of test questions.
- *Trustworthy Online Controlled Experiments* (Kohavi, Tang & Xu). This is the standard book on A/B tests. It is useful when you compare two versions of a RAG system or an agent in production.

**4. RAG and retrieval evaluation**

- *RAGAS: Automated Evaluation of Retrieval Augmented Generation* (Es et al., 2023): [arxiv.org/abs/2309.15217](https://arxiv.org/abs/2309.15217). This paper defines metrics such as faithfulness and context relevance.
- *ARES: An Automated Evaluation Framework for RAG Systems* (Saad-Falcon et al., 2023): [arxiv.org/abs/2311.09476](https://arxiv.org/abs/2311.09476). This paper uses prediction-powered inference to give confidence intervals with a small set of human labels.
- *MTEB: Massive Text Embedding Benchmark* (Muennighoff et al., 2022): [arxiv.org/abs/2210.07316](https://arxiv.org/abs/2210.07316). This paper explains how to benchmark embedding models on many tasks.

**5. LLM and agent evaluation**

- *Survey on Evaluation of LLM-based Agents* (Yehudai et al., 2025): [arxiv.org/abs/2503.16416](https://arxiv.org/abs/2503.16416). It covers agent capabilities (planning, tool use, self-reflection, memory), benchmarks for specific applications, benchmarks for generalist agents, and evaluation frameworks. It also identifies gaps in the evaluation of cost, safety, and robustness.
- *Evaluation and Benchmarking of LLM Agents: A Survey* (2025): [arxiv.org/abs/2507.21504](https://arxiv.org/abs/2507.21504).
- *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena* (Zheng et al., 2023): [arxiv.org/abs/2306.05685](https://arxiv.org/abs/2306.05685). This paper explains when an LLM judge agrees with humans, and its biases.
- *Holistic Evaluation of Language Models (HELM)* (Liang et al., 2022): [arxiv.org/abs/2211.09110](https://arxiv.org/abs/2211.09110). This paper shows how to evaluate on many metrics, not only accuracy.
- *τ-bench* (Yao et al., 2024): [arxiv.org/abs/2406.12045](https://arxiv.org/abs/2406.12045). This paper introduces the pass^k metric. The metric measures if an agent is reliable over many runs, not only one run.

**6. Practical articles**

- Hamel Husain's blog on evals: [hamel.dev](https://hamel.dev/). Practical guides on how to build evals for LLM products, including error analysis and LLM judges.
- Eugene Yan's articles: [eugeneyan.com](https://eugeneyan.com/). Clear articles on evaluation of LLM systems, RAG, and LLM judges.

**Recommended order:**

1. *Think Stats* or *Seeing Theory*, for the basics: distributions, confidence intervals, hypothesis tests, bootstrap.
2. *Mathematics for Machine Learning*, chapters on linear algebra and probability, for embeddings.
3. Miller's "Adding Error Bars to Evals", to apply statistics to AI evals.
4. The RAG and agent papers that are related to your system.

**Note:** Agent evaluation changes quickly. New benchmarks come out frequently, so check the surveys for recent references.

==================================Pass 2================================
No single survey paper covers all the statistics used in AI/ML. The field is too large for one paper. The closest options are a few books that act as surveys, plus survey papers for each area. Here is a list in that structure.

**1. Books that cover the full spectrum (closest to what you want)**

- *Computer Age Statistical Inference* (Efron & Hastie), free: [hastie.su.domains/CASI](https://hastie.su.domains/CASI/). This is the best match. It goes from classical statistics (frequentist, Bayesian) to bootstrap, cross-validation, regularization, and neural networks. It explains how statistics and ML are connected.
- *Probabilistic Machine Learning: An Introduction* and *Advanced Topics* (Kevin Murphy), free: [probml.github.io/pml-book](https://probml.github.io/pml-book/). Two books that cover probability, inference, Bayesian methods, uncertainty, causality, and deep learning. Use them as a reference.
- *The Elements of Statistical Learning* (Hastie, Tibshirani & Friedman), free: [hastie.su.domains/ElemStatLearn](https://hastie.su.domains/ElemStatLearn/). The classic book on statistical learning methods.
- *Understanding Machine Learning: From Theory to Algorithms* (Shalev-Shwartz & Ben-David), free online. This covers learning theory: why models generalize, and how much data is necessary.

**2. Short papers about statistics and ML in general**

- *Statistical Modeling: The Two Cultures* (Leo Breiman, Statistical Science, 2001). A famous essay on the difference between the statistics approach and the ML approach.
- *Statistics versus machine learning* (Bzdok, Altman & Krzywinski, Nature Methods, 2018). A short, easy comparison.

**3. Survey papers by area**

Model evaluation and comparison:
- *Model Evaluation, Model Selection, and Algorithm Selection in Machine Learning* (Raschka, 2018): [arxiv.org/abs/1811.12808](https://arxiv.org/abs/1811.12808). Covers bootstrap, cross-validation, confidence intervals, and statistical tests to compare models. Very practical.
- *Statistical Comparisons of Classifiers over Multiple Data Sets* (Demšar, JMLR, 2006). The standard reference for tests that compare models on many datasets.
- *The Hitchhiker's Guide to Testing Statistical Significance in NLP* (Dror et al., ACL 2018). Significance tests for NLP and LLM results.

Uncertainty:
- *A Survey of Uncertainty in Deep Neural Networks* (Gawlikowski et al., 2021): [arxiv.org/abs/2107.03342](https://arxiv.org/abs/2107.03342).
- *A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification* (Angelopoulos & Bates, 2021): [arxiv.org/abs/2107.07511](https://arxiv.org/abs/2107.07511). Conformal prediction gives guaranteed error rates. It is now also used for LLMs and RAG.

Causality:
- *Toward Causal Representation Learning* (Schölkopf et al., 2021): [arxiv.org/abs/2102.11107](https://arxiv.org/abs/2102.11107).

LLM evaluation:
- *Adding Error Bars to Evals* (Miller, 2024): [arxiv.org/abs/2411.00640](https://arxiv.org/abs/2411.00640). I gave this earlier. It applies the statistics above to LLM evals.

**Recommended order:**

1. *Computer Age Statistical Inference*, for the full overview.
2. Raschka's paper, for model evaluation.
3. The conformal prediction tutorial, for uncertainty.
4. Murphy's books, as a reference when you need more detail on a topic.

==================================Pass 3================================
I searched for 2025 and 2026 resources. I did not find a new survey that covers all statistics for AI/ML. The recent work is focused on specific problems, mainly the statistics of LLM and agent evaluation. Here are the notable items, newest first in each group.

**1. Statistics for LLM evaluation**

- *Measuring all the noises of LLM Evals* (Sida Wang, Meta FAIR, Dec 2025): [arxiv.org/abs/2512.21326](https://arxiv.org/abs/2512.21326). It defines and measures three types of noise: prediction noise from generating different answers on a given question, data noise from sampling questions, and their combined total noise following the law of total variance. An important result: paired prediction noise typically exceeds paired data noise, which means reducing prediction noise by averaging can significantly increase statistical power. The data and analysis are at all-the-noises.github.io. Read this together with Miller's paper.
- *Position: Don't Use the CLT in LLM Evals With Fewer Than a Few Hundred Datapoints* (Bowyer, Aitchison & Ivanova, ICML 2025 Spotlight): [arxiv.org/abs/2503.01747](https://arxiv.org/abs/2503.01747). In small-data settings, CLT-based methods usually underestimate uncertainty a lot (the error bars are too small). The paper recommends alternative frequentist and Bayesian methods that are easy to implement. It includes a simple Python library for the Bayesian methods at github.com/sambowyer/bayes_evals. This is very relevant if your RAG test set is small.
- *How to Correctly Report LLM-as-a-Judge Evaluations* (Lee et al., Nov 2025, revised Jan 2026): [arxiv.org/abs/2511.21140](https://arxiv.org/abs/2511.21140). Imperfect sensitivity and specificity of LLM judges cause bias in scores. The paper gives a simple framework that corrects this bias and builds confidence intervals that include the uncertainty from both the test dataset and a human-labeled calibration dataset.
- *Noisy but Valid: Robust Statistical Evaluation of LLMs with Imperfect Judges* (Feng et al., Jan 2026): [arxiv.org/abs/2601.20913](https://arxiv.org/abs/2601.20913). It shows how evaluation power depends on judge quality, dataset size, and certification levels.

**2. Statistics for agent evaluation**

- *Towards a Science of AI Agent Reliability* (Princeton, Feb 2026, ICML 2026): [arxiv.org/abs/2602.16666](https://arxiv.org/abs/2602.16666). It divides reliability into four dimensions: consistency, robustness, predictability, and safety. On 12 frontier models, it finds that agents are inconsistent across runs, sensitive to prompt rephrasings, and poorly calibrated in their self-assessments, even when accuracy improves.
- arXiv 2602.07150 (Feb 2026): [arxiv.org/abs/2602.07150](https://arxiv.org/abs/2602.07150). It recommends three practices: estimate pass@1 from multiple independent runs per task, use statistical power analysis to find the necessary number of runs, and report pass@k (optimistic bound) and pass^k (pessimistic bound) with k>1.
- *Establishing Best Practices for Building Rigorous Agentic Benchmarks* (Zhu et al., 2025): [arxiv.org/abs/2507.02825](https://arxiv.org/abs/2507.02825). It shows that SWE-bench Verified uses insufficient test cases and that TAU-bench counts empty responses as successful. Such issues can cause errors in measured performance of up to 100% in relative terms. It gives a checklist (ABC) for benchmark design.

**3. Standards document**

- *NIST AI 800-2 (Initial Public Draft): Practices for Automated Benchmark Evaluations of Language Models* (2026). This draft gives voluntary practices for automated benchmark evaluations. It is useful as an industry-level checklist. Search for "NIST AI 800-2" to get the current version, because it was a draft at the time of the comment that I found.

**4. Uncertainty in LLMs (surveys)**

- *A Survey on Uncertainty Quantification of Large Language Models* (Shorinwa et al., Princeton, revised Jul 2025, published Sep 2025): [arxiv.org/abs/2412.05563](https://arxiv.org/abs/2412.05563). It reviews uncertainty quantification methods for LLMs with their strengths and weaknesses, and applications from chatbots to robotics.
- *Uncertainty Quantification and Confidence Calibration in Large Language Models: A Survey* (Liu et al., 2025): [arxiv.org/abs/2503.15850](https://arxiv.org/abs/2503.15850). It divides LLM uncertainty into input uncertainty, reasoning uncertainty, parameter uncertainty, and prediction uncertainty.

**Recommended order:**

1. Miller (2024), for the base method.
2. Wang (Dec 2025), for noise types and paired comparisons.
3. Bowyer et al. (2025), if your test sets have fewer than a few hundred items.
4. Lee et al. (2025), if you use an LLM judge for RAG evaluation.
5. The two agent papers from Feb 2026, for agent work.

For the general statistics foundation, the books from my previous answer are still the best option. Statistics fundamentals do not change quickly, so older books are still correct.