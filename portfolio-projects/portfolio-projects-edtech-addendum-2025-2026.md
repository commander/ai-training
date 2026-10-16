# Addendum (2025–2026): Portfolio Projects — EdTech Domain

**Created/updated:** 2026-10-10

This file extends `portfolio-projects-edtech.md`. It lists new datasets, benchmarks, papers, tools, and rule changes from 2025 and 2026 that are relevant to the eight EdTech projects. We checked each item on 2026-10-10. This file includes only items from peer-reviewed venues, strong labs, known open-source projects, or official sources. Items from 2026 have the label **[2026]**.

---

## New datasets

**MAP – Charting Student Math Misunderstandings (Kaggle competition dataset)**
Vanderbilt University and The Learning Agency, with data from Eedi. Kaggle, July–October 2025.
https://www.kaggle.com/competitions/map-charting-student-math-misunderstandings
Paper: Crossley, Rittle-Johnson, Adler, Burleigh, King, Benner. "Automated Approaches for Scoring Math Misunderstandings in Student Self-Explanations." AIME-Con 2026. https://aclanthology.org/2026.aimecon-main.33/ **[2026]**
The data has more than 52,000 student explanations for 15 multiple-choice math questions. Experts labeled each explanation as correct, a misconception, or neither, and named the misconception. The original file uses the Eedi data with answer options only. This dataset adds the student's written reason. Use it in Project 2 to train and test the misconception classifier on free text. The metric is MAP@3, so you can compare with the competition results. Read the competition data license before you publish a model.

---

## New benchmarks

**MathTutorBench: A Benchmark for Measuring Open-ended Pedagogical Capabilities of LLM Tutors**
Macina, Daheim, Hakimi, Kapur, Gurevych, Sachan (ETH Zurich, TU Darmstadt). EMNLP 2025 (Main).
https://aclanthology.org/2025.emnlp-main.11/ — site: https://eth-lre.github.io/mathtutorbench
This open-source benchmark measures math expertise, student understanding, and teaching skill of an LLM tutor. It includes a reward model that scores the teaching quality of a tutor response. Use it in Project 6 to compare the fixed workflow and the agent. Use it also in Project 4 as a second check next to your own judge model.

**Unifying AI Tutor Evaluation (MRBench) and the BEA 2025 Shared Task**
Maurya, Srivatsa, Petukhova, Kochmar (MBZUAI). NAACL 2025.
https://aclanthology.org/2025.naacl-long.57/
Findings of the shared task: https://aclanthology.org/2025.bea-1.77/ (BEA Workshop 2025)
The paper defines eight dimensions to rate a tutor response, for example mistake identification, guidance, and actionability. The shared task had more than 50 teams. The best macro F1 scores were between 58 and 72 on three-class labels. This shows that automatic tutor evaluation is still difficult. Use these dimensions as the rubric for the Project 4 judge model. Report judge-to-human agreement for each dimension.

**Evaluating Answer Leakage Robustness of LLM Tutors against Adversarial Student Attacks**
Zhao, Knežević, Käser (EPFL). ACL 2026 (Main). **[2026]**
https://aclanthology.org/2026.acl-long.1412/
The authors test LLM tutors against students who try to get the final answer. They use six groups of persuasion and attack methods. Prompted student agents often failed to attack well. Thus the authors fine-tuned an adversarial student agent and propose it as a standard test. Use this method for the Project 5 homework-rule attacks. Report the answer leakage rate as a separate metric.

---

## New papers by project

**Automated Scoring of Short Answer Questions with Large Language Models: Impacts of Model, Item, and Rubric Design** — Project 1
Frohn, Burleigh, Chen (Khan Academy). AIED 2025 (Lecture Notes in Computer Science).
https://doi.org/10.1007/978-3-031-98465-5_6
This study measures how the model, the question, and the rubric design change LLM scoring accuracy. Use it to plan the Project 1 experiments. Change the rubric format as one experiment, not only the LoRA rank.

**From Problem-Solving to Teaching Problem-Solving: Aligning LLMs with Pedagogy using Reinforcement Learning** — Project 6
Dinucu-Jianu, Macina, Daheim, Hakimi, Gurevych, Sachan (ETH Zurich, TU Darmstadt). EMNLP 2025 (Main).
https://aclanthology.org/2025.emnlp-main.15/
The authors train a 7B tutor model with online reinforcement learning against simulated students. The reward gives credit for guidance and gives a penalty for giving away the answer. No human labels were used. Read it before you decide how the Project 6 agent controls "answer given too early".

**Simulated Students in Tutoring Dialogues: Substance or Illusion?** — Project 6
Scarlatos, Lee, Woodhead, Lan. ACL 2026 (Main). **[2026]**
https://aclanthology.org/2026.acl-long.1960/
The authors define metrics for the quality of simulated students. On real math tutoring dialogues, prompted student simulators performed poorly. Fine-tuned simulators were better but still limited. Project 6 step 9 uses a prompted model to act as each student. Measure your simulated students with these metrics. Report this limit in the failure analysis.

**TutorGym: A Testbed for Evaluating AI Agents as Tutors and Students** — Project 6
Weitekamp, Siddiqui, MacLellan (Georgia Institute of Technology). AIED 2025 (Lecture Notes in Computer Science).
https://doi.org/10.1007/978-3-031-98420-4_26
TutorGym connects AI agents to existing intelligent tutoring system interfaces. An agent can act as the tutor or as the student. Use it to test the Project 6 agent in a standard tutoring environment.

- **Project 2:** MAP dataset (see "New datasets").
- **Project 3:** Qwen3 Embedding and Reranker (see "New tools and models").
- **Project 4:** MRBench dimensions and MathTutorBench (see "New benchmarks"). EU AI Act change on bias detection data (see "Regulation and standards changes").
- **Project 5:** Answer leakage benchmark (see "New benchmarks"). UK DfE product safety standards (see below).
- **Project 7 and Project 8:** No strong new domain paper. Use the Project 6 papers for the tutor quality checks in production.

---

## New tools and models

**Qwen3 small models**
Qwen Team (Alibaba). Qwen3 Technical Report, arXiv, May 2025.
https://arxiv.org/abs/2505.09388
Qwen3 has dense models from 0.6B to 32B. The 0.6B and 1.7B models fit the 2–3 GB memory limit in Project 2 after quantization. The 1.7B and 4B models are good base models for the Project 1 grader.

**Qwen3 Embedding and Qwen3 Reranker**
Qwen Team (Alibaba). arXiv, June 2025.
https://arxiv.org/abs/2506.05176 — models: https://huggingface.co/Qwen/Qwen3-Embedding-0.6B and https://huggingface.co/Qwen/Qwen3-Reranker-0.6B
These are open (Apache 2.0) multilingual embedding and reranker models. Use them in Project 3 and compare them with your current models on section-number and formula questions.

**gpt-oss-safeguard**
OpenAI. October 2025.
https://openai.com/index/introducing-gpt-oss-safeguard/ — model: https://huggingface.co/openai/gpt-oss-safeguard-20b
These are open-weight (Apache 2.0) safety classifiers that read your written policy at inference time. Use one in Project 5 for the age-appropriate content check. Write a separate policy for each grade band. Measure the false positive rate on normal student questions.

---

## Regulation and standards changes

**EU AI Act — Digital Omnibus on AI, Regulation (EU) 2026/1744**
European Union. Published in the Official Journal on 24 July 2026. In force from 27 July 2026. **[2026]**
https://digital-strategy.ec.europa.eu/en/news/ai-omnibus-enters-force
Education uses such as "evaluate learning outcomes" are high-risk AI systems in Annex III of the AI Act. The rules for Annex III systems now apply from 2 December 2027, not 2 August 2026. The Omnibus also permits processing of special categories of personal data to detect and correct bias. The Project 1 grader and the Project 4 bias test are in this scope. Update the Project 8 governance summary with the new date.

**Children's Online Privacy Protection Rule (COPPA) amendments**
US Federal Trade Commission (FTC). Published 22 April 2025. In force 23 June 2025. Most compliance duties apply from 22 April 2026. **[2026]**
https://www.federalregister.gov/documents/2025/04/22/2025-05904/childrens-online-privacy-protection-rule
The changes include separate parental consent to share children's data with third parties, a written data retention policy, stronger security rules, and biometric identifiers as personal information. Add these rules to the Project 5 privacy list. Add a retention and deletion test to the Project 5 stretch goal.

**India Digital Personal Data Protection (DPDP) Rules, 2025**
MeitY, Government of India. Notified on 14 November 2025.
https://www.meity.gov.in/documents/act-and-policies/digital-personal-data-protection-rules-2025-gDOxUjMtQWa?pageTitle=Digital-Personal-Data-Protection-Rules-2025
The rules give the method for verifiable parental consent for children's data. These duties start on 14 May 2027. The original file names the DPDP Act only. Use the Rules for the Project 5 consent flow and the Project 8 governance summary.

**Generative AI: product safety standards for education (England)**
UK Department for Education (DfE). First published 22 January 2025. Updated 19 January 2026. **[2026]**
https://www.gov.uk/government/publications/generative-ai-product-safety-expectations
This guidance tells EdTech suppliers what a generative AI product for schools must do. Examples are content filtering, data protection, and governance. The 2026 update adds standards on cognitive development, emotional and social development, mental health, and manipulation. Use it as a checklist for the Project 5 guardrails and the Project 8 risk summary.

**OWASP Top 10 for Agentic Applications 2026**
OWASP GenAI Security Project. December 2025.
https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
Map the Project 6 tools (grade change and parent message) and the Project 5 attack set to this list, in addition to the OWASP Top 10 for LLM Applications.
