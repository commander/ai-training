# Addendum (2025–2026): Healthcare Clinical Documentation Portfolio Projects

**Created/updated:** 2026-10-10

This addendum extends `portfolio-projects-healthcare.md`. It lists datasets, benchmarks, papers, models, and regulation changes from 2025 and 2026 that are relevant to the eight projects. The check date is 2026-10-10. The original file is not changed. Each item was checked against its official page or its arXiv record.

The selection rule is strict. Each item comes from a peer-reviewed venue, a strong lab, an official technical report, or an official regulator. Items with low impact are not included.

---

## New datasets

**MedXpertQA: Benchmarking Expert-Level Medical Reasoning and Understanding**
Zuo et al., Tsinghua University and Shanghai AI Laboratory. ICML 2025. January 2025.
Paper: https://arxiv.org/abs/2501.18362 · Data: https://huggingface.co/datasets/TsinghuaC3I/MedXpertQA
The dataset has 4,460 hard questions in 17 specialties, with a text subset and a multimodal subset. It is harder than MedQA, and the authors reduced data leakage. Use the text subset as a general medical knowledge check when you compare base, fine-tuned, and frontier models in Projects 1 and 2.

---

## New benchmarks

**HealthBench: Evaluating Large Language Models Towards Improved Human Health**
Arora et al., OpenAI. Technical report on arXiv. May 2025.
https://arxiv.org/abs/2505.08775
HealthBench has 5,000 multi-turn health conversations. Each conversation has a rubric that physicians wrote, with 48,562 rubric criteria in total. Use the rubric method in Project 4: write rubric criteria for each answer and score each criterion separately. It also shows how to measure judge-to-physician agreement.

**MedHELM: Holistic Evaluation of Large Language Models for Medical Tasks**
Bedi et al., Stanford University (CRFM and Stanford Medicine). arXiv. May 2025.
Paper: https://arxiv.org/abs/2505.23802 · Leaderboard: https://crfm.stanford.edu/helm/medhelm/latest/
MedHELM has a taxonomy of 121 clinical tasks that 29 clinicians checked. It has 35 benchmarks in five categories, including clinical note generation and administration. It uses an LLM jury and reports cost against quality. Use it to select models for Projects 1 and 8. Use the LLM-jury method as a second judge design in Project 4.

**MedAgentBench: A Realistic Virtual EHR Environment to Benchmark Medical LLM Agents**
Jiang et al., Stanford University. NEJM AI, August 2025 (arXiv January 2025).
Paper: https://arxiv.org/abs/2501.14654 · Code: https://github.com/stanfordmlgroup/MedAgentBench
The benchmark has 300 clinician-written tasks in a FHIR-compliant environment with 100 patient profiles. The agent must read and write records through API calls. Use this FHIR environment as the mock patient-record tool in Project 6. Use its task design as a template for your 30-case test set.

**SDBench (Sequential Diagnosis Benchmark), from "Sequential Diagnosis with Language Models"**
Nori et al., Microsoft AI. arXiv. June 2025.
https://arxiv.org/abs/2506.22405
SDBench converts 304 NEJM case reports into step-by-step encounters. The agent asks questions and orders tests, and each test has a cost. The paper also describes MAI-DxO, an orchestrator with several model roles. Use the "cost per correct result" metric in Projects 6 and 7. Use the orchestrator design as a reference for the multi-agent design in Project 7.

---

## New papers by project

**Project 7 — Can Large Language Models Match the Conclusions of Systematic Reviews?**
Polzak et al., Stanford University. ICLR 2026 (arXiv May 2025).
https://arxiv.org/abs/2505.22787
The paper gives MedEvidence, a benchmark that pairs 100 systematic reviews with their source studies. It tests 24 models. The models become worse on long inputs, they are overconfident, and they do not reduce the weight of low-quality studies. This result supports step 9 of Project 7 (the low-quality study test). Use MedEvidence as an external test set for your reviewer agent.

**Projects 4, 6, and 7 — see the benchmarks above.** HealthBench (rubrics), MedAgentBench (EHR agent tasks), and SDBench (cost-aware diagnosis) are the main 2025 research results for these projects.

No 2025–2026 item of sufficient quality was found that is specific to Project 3 (drug label RAG) or Project 5 (consent-aware access control). The original references stay valid for these projects.

---

## New tools and models

**MedGemma (4B multimodal, 27B text, 27B multimodal)**
Sellergren et al., Google Research and Google DeepMind. Technical report on arXiv. May 2025 (release), July 2025 (report).
Report: https://arxiv.org/abs/2507.05201 · Model: https://huggingface.co/google/medgemma-27b-text-it
MedGemma is a set of open medical models based on Gemma 3. The 4B model is a good base model for QLoRA in Project 1. It can also be a local teacher or student in Project 2, so no patient text goes to an external API. Read the Health AI Developer Foundations terms of use before you publish a fine-tuned model.

**MedGemma 1.5 and MedASR** — 2026
Google Research. Official release. January 2026.
MedGemma 1.5: https://huggingface.co/google/medgemma-1.5-4b-it · MedASR: https://huggingface.co/google/medasr
MedGemma 1.5 (4B) adds better extraction from medical lab reports and more imaging support. MedASR is an open medical speech-to-text model. Use MedGemma 1.5 for lab-value extraction in Project 1. Use MedASR to add a dictation input to Project 8. Measure word error rate on drug names and doses.

---

## Regulation and standards changes

**FDA: Clinical Decision Support Software — revised final guidance** — 2026
U.S. Food and Drug Administration. Final guidance. 6 January 2026. It replaces the 2022 guidance.
https://www.fda.gov/regulatory-information/search-fda-guidance-documents/clinical-decision-support-software
The guidance keeps the four criteria for non-device CDS software. It adds enforcement discretion for some tools that give one recommendation, and it discusses automation bias. Use it for the risk and governance summary in Project 8. The clinician must be able to review the basis of each answer, so show source text next to each claim.

**FDA: Artificial Intelligence-Enabled Device Software Functions: Lifecycle Management and Marketing Submission Recommendations (draft guidance)**
U.S. Food and Drug Administration. Draft guidance. January 2025.
https://www.fda.gov/regulatory-information/search-fda-guidance-documents/artificial-intelligence-enabled-device-software-functions-lifecycle-management-and-marketing
The draft gives lifecycle recommendations for AI device software: data quality, bias, transparency, performance monitoring, and change control. Use its list of documents as a checklist for the decision log and the Project 4 regression reports. It applies only if a product is a medical device. State in the README that your portfolio system is not a device.

**HHS: HIPAA Security Rule notice of proposed rulemaking**
U.S. Department of Health and Human Services, Office for Civil Rights. Proposed rule. 6 January 2025.
https://www.hhs.gov/hipaa/for-professionals/security/hipaa-security-rule-nprm/index.html
The proposed rule adds requirements such as an asset inventory, encryption of ePHI, multi-factor authentication, and annual compliance audits. On the check date, it is still a proposed rule, not a final rule. Use its controls as design targets for access control and audit logs in Projects 5 and 8.

**CMS Interoperability and Prior Authorization Final Rule (CMS-0057-F): 2026 and 2027 dates**
U.S. Centers for Medicare & Medicaid Services. Final rule from January 2024. Its first compliance dates are in 2026.
https://www.cms.gov/newsroom/fact-sheets/cms-interoperability-and-prior-authorization-final-rule-cms-0057-f
From January 2026, affected payers must decide urgent requests in 72 hours and standard requests in 7 calendar days. They must also give a specific reason for each denial. From January 2027, they must provide a FHIR Prior Authorization API. Use these time limits and the denial-reason field in the Project 6 mock payer. Design the Project 6 tools to match a FHIR-based API.

**Joint Commission and CHAI: Guidance on the Responsible Use of AI in Healthcare (RUAIH)**
The Joint Commission and the Coalition for Health AI. Guidance. 17 September 2025.
https://www.chai.org/news/joint-commission-and-coalition-for-health-ai-chai-release-initial-guidance
The guidance gives seven elements for hospital AI governance, including local validation, monitoring, and reporting of AI safety events. Use it as the structure for the one-page risk and governance summary in Project 8.

**EU AI Act: Digital Omnibus on AI (Regulation (EU) 2026/1744)** — 2026
European Parliament and Council. Official Journal of the EU. Published 24 July 2026.
https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202601744
The omnibus moves the high-risk dates. High-risk systems in Annex III now apply from 2 December 2027. AI in products under Annex I law (this includes medical devices under the MDR and IVDR) now applies from 2 August 2028. The Article 50 transparency duties apply from 2 August 2026, with a four-month transition period for marking duties on systems already on the market. For Project 8, tell users that they talk to an AI system if you deploy in the EU.

---

## Items not included

These items were found but not included, because they did not meet the quality rule or could not be fully confirmed: smaller 2025 medical agent and RAG papers without a strong-lab source or venue, and vendor blog posts about clinical assistants.
