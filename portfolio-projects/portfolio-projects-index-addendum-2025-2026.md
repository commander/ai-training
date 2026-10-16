# Addendum (2025–2026): Portfolio Projects Domain Index

**Created/updated:** 2026-10-10

This file extends `portfolio-projects-index.md`. It lists new cross-domain benchmarks from 2025–2026 and one new domain to consider. The check date is 2026-10-10. Each domain file can have its own addendum with domain-specific items.

---

## New cross-domain benchmarks

**GDPval: Evaluating AI Model Performance on Real-World Economically Valuable Tasks**
OpenAI. arXiv:2510.04374 and OpenAI report. September 2025 (arXiv October 2025).
https://arxiv.org/abs/2510.04374 and https://openai.com/index/gdpval
GDPval has tasks from 44 occupations in the 9 largest sectors of US GDP. Experts with an average of 14 years of experience wrote the tasks. OpenAI released a gold subset of 220 tasks. Use it to select domain tasks with real work products (for example, legal, healthcare, and manufacturing deliverables) for Projects 4 and 7.

**τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment**
Barres, Dong, Ray, Si, Narasimhan (Sierra, University of Toronto). arXiv:2506.07982. June 2025.
https://arxiv.org/abs/2506.07982 — code: https://github.com/sierra-research/tau2-bench
Both the agent and the simulated user can use tools on a shared state. It has retail, airline, and telecom domains. It is a direct reference design for the Project 6 agent test set in the e-commerce, travel, and support domains.

**MCP-Universe: Benchmarking Large Language Models with Real-World Model Context Protocol Servers**
Salesforce AI Research. arXiv:2508.14704. NeurIPS 2025. August 2025.
https://arxiv.org/abs/2508.14704
The benchmark uses 11 real MCP servers in 6 domains, with execution-based evaluators. In the paper, the best model completed less than half of the tasks. Use it as a reference when you expose tools through an MCP server (Project 6 stretch goal) in any domain.

**BrowseComp: A Simple Yet Challenging Benchmark for Browsing Agents**
Wei et al. (OpenAI). arXiv:2504.12516. April 2025.
https://arxiv.org/abs/2504.12516
BrowseComp has 1,266 questions that need many web search steps. The answers are short, so you can check them exactly. It is a good model for the Project 7 researcher agent test in all domains.

**HealthBench: Evaluating Large Language Models Towards Improved Human Health**
Arora, Wei, et al. (OpenAI). arXiv:2505.08775. May 2025.
https://arxiv.org/abs/2505.08775
HealthBench has 5,000 multi-turn conversations. 262 physicians wrote the rubrics (48,562 criteria). It shows how to grade open answers with per-case rubrics. The method applies to Project 4 judge design in all regulated domains, not only healthcare.

---

## New domain to consider

**Financial research and analysis.** The index has no finance domain. Public data is available: SEC EDGAR filings (already used in the legal file) and the benchmark below. The special challenges are exact numbers from tables, fiscal-period dates, and high-cost errors.

**Finance Agent Benchmark: Benchmarking LLMs on Real-world Financial Research Tasks**
Vals AI. arXiv:2508.00828. August 2025.
https://arxiv.org/abs/2508.00828
The benchmark has 537 expert-written questions in 9 task categories. The agent gets web search and EDGAR access as tools. In the paper, the best model (OpenAI o3) had 46.8% accuracy at $3.79 per query. This gives a ready baseline for Projects 3, 6, and 8 in a finance domain.

Suggested row for the domain table:

| Domain | File | Main public data | Special challenge |
|---|---|---|---|
| Financial research | (not written yet) | SEC EDGAR, Finance Agent Benchmark | Exact table numbers, fiscal periods |
