# Addendum (2025–2026): Portfolio Projects — Travel and Hospitality Domain

**Created/updated:** 2026-10-10

This file extends `portfolio-projects-travel.md`. It lists new benchmarks, papers, tools, and rule changes from 2025 and 2026 that are relevant to the eight travel projects. We checked each item on 2026-10-10. This file includes only items from peer-reviewed venues, strong labs, known open-source projects, or official sources. Items from 2026 have the label **[2026]**.

---

## New datasets

The new travel datasets from this period come with a benchmark. They are listed in the next section.

---

## New benchmarks

**τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment**
Barres, Dong, Ray, Si, Narasimhan (Sierra Research). arXiv, June 2025.
https://arxiv.org/abs/2506.07982 — code: https://github.com/sierra-research/tau2-bench
This benchmark extends τ-bench. It has airline, retail, and telecom domains with a policy document, tools, and a simulated user. It measures success over repeated runs (pass^k), so it shows if an agent is consistent. Use the airline domain as an external test for the Project 6 rebooking agent. Copy its policy-plus-tools format for your own 30 disruption cases.

**TripCraft: A Benchmark for Spatio-Temporally Fine Grained Travel Planning**
Chaudhuri, Purkar, Raghav, Mallick, Gupta, Jana. ACL 2025 (Main).
https://aclanthology.org/2025.acl-long.834/
TripCraft uses real transit schedules, events, attraction categories, and user personas. It adds continuous scores for meal timing, attraction timing, spatial order, and persona fit. Use these scores in Project 7 in addition to pass or fail constraint checks.

**TP-RAG: Benchmarking Retrieval-Augmented Large Language Model Agents for Spatiotemporal-Aware Travel Planning**
Ni, Liu, Ma, Su, Wang. EMNLP 2025 (Main).
https://doi.org/10.18653/v1/2025.emnlp-main.626
TP-RAG tests travel plans that use retrieved reference trips. It checks spatial and time consistency of the plan. It helps Project 7 (the researcher agent gives retrieved facts to the planner) and Project 3 (retrieval quality for planning questions).

**ChinaTravel: An Open-Ended Travel Planning Benchmark with Compositional Constraint Validation for Language Agents**
Shao, Zhang, Yang, et al. (Nanjing University, LAMDA). ICLR 2026. **[2026]**
https://arxiv.org/abs/2412.13682
ChinaTravel uses real user queries and a domain language that converts each constraint into code that a program can check. This is the same method that Project 7 asks for: use code, not the model, for opening hours, travel time, and budget checks.

**TravelBench (Beyond Itinerary Planning: A Real-World Benchmark for Multi-Turn and Tool-Using Travel Tasks)**
Cheng, Hu, Zhang, et al. ACL 2026 (Main). arXiv December 2025. **[2026]**
https://arxiv.org/abs/2512.22673
TravelBench has three subsets: single-turn, multi-turn, and unsolvable. It uses ten travel tools with cached results in a sandbox, so runs are repeatable. The "unsolvable" subset tests if the agent knows when it cannot do a task. Use it for the Project 6 refusal tests and the Project 7 multi-turn preference tests.

**DeepPlanning: Benchmarking Long-Horizon Agentic Planning with Verifiable Constraints**
Qwen Team (Alibaba). arXiv, January 2026. **[2026]**
https://arxiv.org/abs/2601.18137 — data: https://huggingface.co/datasets/Qwen/DeepPlanning
DeepPlanning has multi-day travel tasks with a time budget and a money budget. The agent must find information with tools before it plans. All constraints can be checked by code. Use it to test the Project 7 checker and the budget limits.

---

## New papers by project

- **Project 1 (booking extractor):** No strong new paper for this task. Use the Qwen3 small models (see "New tools and models") as new base models for the QLoRA experiments.
- **Project 2 (review aspect classifier):** No strong new travel-specific paper. Use the small Qwen3 or Gemma 3 models as student models.
- **Project 3 (policy and fare rule search):** TP-RAG (above) for retrieval in travel tasks.
- **Project 4 (evaluation harness):** ChinaTravel and DeepPlanning (above) show how to write constraint checks as code. Use the same method for fee, date, and visa checks.
- **Project 5 (corporate travel assistant):** OWASP Top 10 for Agentic Applications (see "Regulation and standards changes").
- **Project 6 (rebooking agent):** τ²-Bench and TravelBench (above).
- **Project 7 (itinerary planner):** TripCraft, TP-RAG, ChinaTravel, TravelBench, and DeepPlanning (above).
- **Project 8 (capstone):** Use τ²-Bench pass^k as an online reliability metric. Run the same case many times and report the success rate for all runs.

---

## New tools and models

**Qwen3 Embedding and Qwen3 Reranker**
Qwen Team (Alibaba). arXiv, June 2025.
https://arxiv.org/abs/2506.05176 — models: https://huggingface.co/Qwen/Qwen3-Embedding-0.6B and https://huggingface.co/Qwen/Qwen3-Reranker-0.6B
These are open multilingual embedding and reranker models (Apache 2.0) in 0.6B, 4B, and 8B sizes. Use them in Project 3 as the dense model and the cross-encoder reranker. Compare them with your current models on airline codes and fare class questions.

**Qwen3 Technical Report**
Qwen Team (Alibaba). arXiv, May 2025.
https://arxiv.org/abs/2505.09388
Qwen3 has dense models from 0.6B to 32B with a "thinking" mode and a "non-thinking" mode. The 0.6B and 1.7B models are good student models for the Project 2 on-prem classifier. The 1.7B and 4B models are good base models for the Project 1 extractor.

**gpt-oss-safeguard**
OpenAI. October 2025.
https://openai.com/index/introducing-gpt-oss-safeguard/ — model: https://huggingface.co/openai/gpt-oss-safeguard-20b
These are open-weight (Apache 2.0) safety classifiers. You give the model your own written policy at inference time. Use it in Project 5 to scan supplier hotel descriptions for embedded instructions. Write the policy as text and measure the false positive rate.

---

## Regulation and standards changes

**OWASP Top 10 for Agentic Applications 2026**
OWASP GenAI Security Project, Agentic Security Initiative. December 2025.
https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
This list covers risks in agents that plan, use tools, keep memory, and act for a user. The original file maps attacks only to the OWASP Top 10 for LLM Applications. In Projects 5, 6, and 8, also map the attack test set to this agentic list. Rebooking and payment tools are the high-risk actions.

**EU Entry/Exit System (EES) start and ETIAS status**
European Commission (DG Migration and Home Affairs). EES started on 12 October 2025. It became fully operational on 10 April 2026. **[2026]**
https://home-affairs.ec.europa.eu/policies/schengen/smart-borders/entry-exit-system_en and https://travel-europe.europa.eu/en/etias
EES records the entry and exit of non-EU travelers to Schengen countries. On 2026-10-10, the official ETIAS page says that ETIAS is not in operation. Add both pages to the Project 3 visa corpus with their retrieval dates. Add test questions to Project 4 that check the answer gives the correct status for the travel date.

**India Digital Personal Data Protection (DPDP) Rules, 2025**
Ministry of Electronics and Information Technology (MeitY), Government of India. Notified on 14 November 2025.
https://www.meity.gov.in/documents/act-and-policies/digital-personal-data-protection-rules-2025-gDOxUjMtQWa?pageTitle=Digital-Personal-Data-Protection-Rules-2025
The rules start in phases. The Data Protection Board started on 14 November 2025. Consent Manager registration starts on 14 November 2026. Notice, consent, security, and breach reporting duties start on 14 May 2027. Traveler profiles in Project 5 contain passport numbers and dates of birth. Use these rules to write the consent, retention, and breach-reporting steps in the Project 5 and Project 8 governance summary.
