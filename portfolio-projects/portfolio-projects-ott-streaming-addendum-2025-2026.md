# Addendum (2025–2026): Portfolio Projects — OTT Video Streaming Domain

**Created/updated:** 2026-10-10

This file extends `portfolio-projects-ott-streaming.md`. It lists new datasets, benchmarks, papers, tools, and rule changes from 2025 and 2026 that are relevant to the eight OTT projects. We checked each item on 2026-10-10. This file includes only items from peer-reviewed venues, strong labs, known open-source projects, or official sources. Items from 2026 have the label **[2026]**.

---

## New datasets

**COMI-LINGUA: Expert Annotated Large-Scale Dataset for Multitask NLP in Hindi-English Code-Mixing**
Sheth, Beniwal, Singh (IIT Gandhinagar, LINGO Lab). Findings of EMNLP 2025.
https://aclanthology.org/2025.findings-emnlp.422/ — project page: https://lingo-iitgn.github.io/resources/comi-lingua.html
This dataset has more than 125,000 Hinglish examples. Three bilingual annotators labeled each example. Tasks include token-level language ID, named entity recognition, and translation. Use the NER and language ID data in Project 3 to test query parsing for Hinglish searches such as "koi achhi thriller movie 2019 ki". Use it also to make the Hinglish part of the Project 4 test set.

---

## New benchmarks

**Overview of the TREC 2025 Tip-of-the-Tongue (ToT) Track**
Arguello, Diaz, Fröbe, Kim, Mitra. TREC 2025, arXiv January 2026. **[2026]**
https://arxiv.org/abs/2601.20671
"Tip-of-the-tongue" queries are long descriptions of an item when the user does not remember its name. Movies and TV shows are a main part of this task. The 2025 track has test queries from real users, from manual topic writing, and from LLM generation. Use these queries in Project 3 and Project 4 for "that 2012 heist film with actor X" style queries. Report dense, BM25, and hybrid results on them.

---

## New papers by project

- **Project 1 (metadata tagger):** No strong new paper for this task. Use the new small models in "New tools and models" as base models.
- **Project 2 (on-device advisory classifier):** gpt-oss-safeguard (see "New tools and models") can be the teacher model. It reads your written advisory policy at inference time.

**Collaborative Retrieval for Large Language Model-based Conversational Recommender Systems (CRAG)** — Project 3
Zhu, Wan, Steck, Liang, Feng, Kallus (University of Virginia, Cornell University, Netflix). WWW 2025.
https://arxiv.org/abs/2502.14137
CRAG adds collaborative filtering results to the LLM context in a conversational recommender. The authors also release Reddit-v2, a cleaned movie conversation dataset. This is the same idea as Project 3 step 7 (combine search score with user preference). Use Reddit-v2 to test multi-turn requests such as "something like that, but funnier".

**PLUM: Adapting Pre-trained Language Models for Industrial-scale Generative Recommendations** — Project 3
He, Heldt, Hong, Keshavan, Mao, Mehta, et al. (Google, YouTube). WWW 2026, arXiv October 2025. **[2026]**
https://arxiv.org/abs/2510.07784
PLUM gives each video a "Semantic ID" (a short code of tokens). It then trains an LLM to generate the IDs of items to recommend. Read it to understand how large video platforms move from embedding search to generative retrieval. Add a short comparison to the Project 3 decision log.

**GenRec: An LLM-Backed Recommendation Ranker at Netflix** — Projects 3 and 8
Li, Sehgal, Rao, Houthooft, Zhu, Rastogi, et al. (Netflix). arXiv, August 2026. **[2026]**
https://arxiv.org/abs/2608.10257
GenRec replaces a ranker with thousands of hand-made features by an LLM that reads the user history as text. The paper describes input design, reward signals, and a serving design that keeps cost low (prefill-only inference). It reports a large A/B test. Use it in Project 8 as a reference for the latency and cost of an LLM ranking step. Netflix also described its recommendation foundation model in a tech blog post in March 2025 (https://netflixtechblog.com/foundation-model-for-personalized-recommendation-1a0bd8e02d39).

**Content Moderation in TV Search: Balancing Policy Compliance, Relevance, and User Experience** — Projects 5 and 8
Hande, Sundararajan, Hamidian, Türe (Comcast). SIGIR 2025.
https://doi.org/10.1145/3726302.3731962
The authors add an LLM monitoring layer to a TV search system. The layer flags results that do not agree with the query or with the content policy. Use this design in Project 5 as a final output check for kids profiles. Report its false positive rate.

- **Project 6 (content operations agent):** OWASP Top 10 for Agentic Applications (see "Regulation and standards changes").
- **Project 7 (localized marketing):** India IT Rules amendment 2026 (see "Regulation and standards changes") if you add AI audio or video.

---

## New tools and models

**Qwen3 Embedding and Qwen3 Reranker**
Qwen Team (Alibaba). arXiv, June 2025.
https://arxiv.org/abs/2506.05176 — models: https://huggingface.co/Qwen/Qwen3-Embedding-0.6B and https://huggingface.co/Qwen/Qwen3-Reranker-0.6B
These are open multilingual embedding and reranker models (Apache 2.0) in 0.6B, 4B, and 8B sizes. Use them in Project 3 for Hindi, Tamil, and Hinglish queries. Compare them with your current dense model and cross-encoder.

**EmbeddingGemma**
Google DeepMind. arXiv, September 2025.
https://arxiv.org/abs/2509.20354 — model: https://huggingface.co/google/embeddinggemma-300m
This is a 300M-parameter multilingual embedding model made for devices. Use it for on-device search on a set-top box (Project 2 stretch goal or a Project 8 offline mode).

**Gemma 3 (including Gemma 3 270M)**
Google DeepMind. Technical report March 2025. The 270M model was released in August 2025.
https://arxiv.org/abs/2503.19786 — model: https://huggingface.co/google/gemma-3-270m
Gemma 3 has small text models (270M and 1B) that run on a CPU with little memory. Use them as student models for the Project 2 advisory classifier on a TV device with 2–4 GB of memory.

**gpt-oss-safeguard**
OpenAI. October 2025.
https://openai.com/index/introducing-gpt-oss-safeguard/ — model: https://huggingface.co/openai/gpt-oss-safeguard-20b
These are open-weight (Apache 2.0) safety classifiers. You give the model your written policy at inference time, and it returns a label with its reasons. Use it as the Project 2 teacher with your MPA and CBFC based advisory policy. Use it also in Project 5 to scan user reviews for embedded instructions.

**Voxtral Realtime**
Mistral AI. arXiv, February 2026. **[2026]**
https://arxiv.org/abs/2602.11298 — model: https://huggingface.co/mistralai/Voxtral-Mini-4B-Realtime-2602
This is an open-weight (Apache 2.0) speech-to-text model trained for streaming. The paper reports offline-level accuracy at less than one second of delay. Use it in Project 8 for remote-control voice search. Measure it against the 300 ms speech-to-text budget.

**IndicConformer 600M Multilingual**
AI4Bharat (IIT Madras). Hugging Face, March 2025.
https://huggingface.co/ai4bharat/indic-conformer-600m-multilingual
This is an open (MIT) speech recognition model for 22 Indian languages. Use it in Project 8 for Hindi and regional voice queries. Report word error rate on title names and actor names for each language. On 2026-10-10, the model card recommends a newer model, Indic-Transcribe (https://huggingface.co/bodhan-ai/indic-transcribe-core, August 2026), with code-mixing support. It has a custom license. Read the license before you use it.

---

## Regulation and standards changes

**MIB advisory to OTT platforms on the Code of Ethics**
Ministry of Information and Broadcasting (MIB), Government of India. 19 February 2025.
https://mib.gov.in/sites/default/files/2025-02/advisory-dated-19.02.2025_0.pdf
The advisory tells OTT platforms to follow the IT Rules 2021 Code of Ethics strictly. This includes age-based content classification and access controls for "A" rated content. This supports the kids-profile filters in Project 5 and the advisory labels in Projects 1 and 2.

**Information Technology (Intermediary Guidelines and Digital Media Ethics Code) Amendment Rules, 2026**
MeitY, Government of India. Notified 10 February 2026. In force from 20 February 2026. **[2026]**
FAQ: https://www.meity.gov.in/static/uploads/2025/10/065b6deb585441b5ccdf8be42502a49c.pdf
The amendment defines "synthetically generated information" (AI-made audio, images, and video that look real). It adds labelling and due-diligence duties for intermediaries. If Project 7 or Project 8 makes AI voice-overs or AI video promotions, record how you label them.

**India Digital Personal Data Protection (DPDP) Rules, 2025**
MeitY, Government of India. Notified on 14 November 2025.
https://www.meity.gov.in/documents/act-and-policies/digital-personal-data-protection-rules-2025-gDOxUjMtQWa?pageTitle=Digital-Personal-Data-Protection-Rules-2025
The rules start in phases. Notice, consent, and verifiable parental consent for children's data start on 14 May 2027. Kids profiles and watch history are personal data. Use these rules in the Project 8 governance summary and in the Project 5 audit log design.

**OWASP Top 10 for Agentic Applications 2026**
OWASP GenAI Security Project. December 2025.
https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
This list covers risks in agents that use tools and act for a user. Map the Project 6 tools (catalog update, publish, and schedule) and the Project 5 attack set to this list, in addition to the OWASP Top 10 for LLM Applications.
