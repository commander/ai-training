# Addendum (2025–2026): Portfolio Projects — Food Delivery and Quick Commerce Domain

**Created/updated:** 2026-10-10

This file extends `portfolio-projects-quick-commerce.md`. It lists new datasets, benchmarks, papers, tools, and rule changes from 2025 and 2026 that are relevant to the eight food delivery and quick commerce projects. We checked each item on 2026-10-10. This file includes only items from peer-reviewed venues, strong labs, known open-source projects, or official sources. Items from 2026 have the label **[2026]**.

---

## New datasets

**FoodInstruct (with the FoodGPT paper)**
Wang, Dong, Bai, Jiang, et al. (Alibaba Group, Ele.me). KDD 2025.
Paper: https://doi.org/10.1145/3711896.3737222 — data: https://huggingface.co/datasets/elemenlp/FoodInstruct — model: https://huggingface.co/elemenlp/FoodGPT
FoodInstruct is a Chinese dataset with 1.6 million examples for 12 food delivery NLP tasks. The paper trains FoodGPT with SFT, DPO, and PPO plus a rule-based reward. It reports gains in a live A/B test. The text is in Chinese, so you cannot use it as Indian training data. Use its task list to check that your Project 1 schema and Project 2 categories cover the common food delivery tasks. Read the dataset license before use.

**COMI-LINGUA: Expert Annotated Large-Scale Dataset for Multitask NLP in Hindi-English Code-Mixing**
Sheth, Beniwal, Singh (IIT Gandhinagar, LINGO Lab). Findings of EMNLP 2025.
https://aclanthology.org/2025.findings-emnlp.422/ — project page: https://lingo-iitgn.github.io/resources/comi-lingua.html
This dataset has more than 125,000 Hinglish examples with token-level language ID, named entities, and translation. It is general text, not grocery text. Use it in Project 2 to test the complaint classifier on real Hinglish writing styles. Use it in Project 3 to test query normalization ("doodh", "atta 5kg").

---

## New benchmarks

**τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment**
Barres, Dong, Ray, Si, Narasimhan (Sierra Research). arXiv, June 2025.
https://arxiv.org/abs/2506.07982 — code: https://github.com/sierra-research/tau2-bench
The retail domain has orders, returns, exchanges, and a written policy that the agent must follow. A simulated user talks to the agent. The benchmark reports pass^k, which shows if the agent is consistent over repeated runs. Use it as an external test for the Project 6 order issue agent. Copy its policy-plus-tools format for your 50 test cases.

**DispatchQA: A Benchmark for Small Function Calling Language Models in E-Commerce Applications**
Daiber, Maricato, Sinha, Rabinovich (Upwork). EMNLP 2025 (Industry Track).
https://aclanthology.org/2025.emnlp-industry.154/
DispatchQA tests if a small model can convert a search query into a correct API call with filters. It measures search relevance and latency together. The authors release data, code, and checkpoints. Use this method in Project 3 for the pack size, brand, and dietary filters that the model extracts from the query.

**DeepPlanning: Benchmarking Long-Horizon Agentic Planning with Verifiable Constraints**
Qwen Team (Alibaba). arXiv, January 2026. **[2026]**
https://arxiv.org/abs/2601.18137 — data: https://huggingface.co/datasets/Qwen/DeepPlanning
DeepPlanning includes multi-product shopping tasks with budget constraints that code can check. Use it as a model for the Project 7 checker: every number in the report must pass a code check, not a model check.

---

## New papers by project

- **Project 1 (catalog normalizer):** FoodGPT (see "New datasets") shows domain post-training for food delivery text.
- **Project 2 (complaint classifier):** COMI-LINGUA for Hinglish tests. Qwen3 small models as students (see "New tools and models").

**GrocLM: Grocery Category Recommendation in E-Commerce with Large Language Models** — Projects 3 and 7
Zhong, Ruan, Hasani, Tenneti, Wang, Ma (Instacart, Pennsylvania State University). ACL 2026 (Industry Track). **[2026]**
https://aclanthology.org/2026.acl-industry.25/
GrocLM fine-tunes a language model with LoRA to learn repeat purchase patterns in grocery. It uses trie-based constrained decoding so that every output is a valid category. In a live test, cart-adds per impression increased by 7.5%. Use constrained decoding over your category list in Project 3 (personal ranking stretch goal). Use the repeat-purchase idea in the Project 7 demand analysis.

- **Project 4 (evaluation harness):** τ²-Bench pass^k (see "New benchmarks") for repeat-run consistency.
- **Project 5 (multi-partner assistant):** gpt-oss-safeguard (see "New tools and models") and OWASP agentic list (see "Regulation and standards changes").
- **Project 6 (order issue agent):** τ²-Bench retail domain.
- **Project 7 (assortment system):** GrocLM and DeepPlanning.
- **Project 8 (capstone):** No strong new domain paper. Use τ²-Bench pass^k as an online reliability metric.

---

## New tools and models

**Qwen3 small models**
Qwen Team (Alibaba). Qwen3 Technical Report, arXiv, May 2025.
https://arxiv.org/abs/2505.09388
Qwen3 has dense models from 0.6B to 32B and supports many languages, including Hindi. The 0.6B and 1.7B models are good student models for the Project 2 phone classifier. The 1.7B and 4B models are good base models for the Project 1 normalizer.

**Qwen3 Embedding and Qwen3 Reranker**
Qwen Team (Alibaba). arXiv, June 2025.
https://arxiv.org/abs/2506.05176 — models: https://huggingface.co/Qwen/Qwen3-Embedding-0.6B and https://huggingface.co/Qwen/Qwen3-Reranker-0.6B
These are open (Apache 2.0) multilingual embedding and reranker models. Use them in Project 3. Measure them with and without the synonym list on Hinglish and misspelled queries.

**gpt-oss-safeguard**
OpenAI. October 2025.
https://openai.com/index/introducing-gpt-oss-safeguard/ — model: https://huggingface.co/openai/gpt-oss-safeguard-20b
These are open-weight (Apache 2.0) safety classifiers that read your written policy at inference time. Use one in Project 5 to scan menu text and reviews for embedded instructions before indexing. Measure the false positive rate on normal menu text.

**IndicConformer 600M Multilingual**
AI4Bharat (IIT Madras). Hugging Face, March 2025.
https://huggingface.co/ai4bharat/indic-conformer-600m-multilingual
This is an open (MIT) speech recognition model for the 22 official Indian languages. Use it for the Project 2 voice input stretch goal. Measure the word error rate on rider voice notes before the classifier step.

---

## Regulation and standards changes

**India Digital Personal Data Protection (DPDP) Rules, 2025**
MeitY, Government of India. Notified on 14 November 2025.
https://www.meity.gov.in/documents/act-and-policies/digital-personal-data-protection-rules-2025-gDOxUjMtQWa?pageTitle=Digital-Personal-Data-Protection-Rules-2025
The rules start in phases. Notice, consent, security, and breach reporting duties start on 14 May 2027. Customer phone numbers, addresses, and order history are personal data. Use these rules for the Project 5 redaction and data deletion stretch goal, and for the Project 8 governance summary.

**OWASP Top 10 for Agentic Applications 2026**
OWASP GenAI Security Project. December 2025.
https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
This list covers risks in agents that use tools and act for a user. The Project 6 refund, replacement, and rider contact tools are high-risk actions. Map the Project 5 and Project 6 attack tests to this list, in addition to the OWASP Top 10 for LLM Applications.
