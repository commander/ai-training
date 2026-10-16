# Addendum (2025–2026): Portfolio Projects — Legal Contract Review Domain

**Created/updated:** 2026-10-10

This addendum extends `portfolio-projects-legal.md`. It lists new datasets, benchmarks, studies, and rule changes from 2025 and 2026 that apply to the eight legal projects. The check date is 2026-10-10. Each item was confirmed against its official source. Items that the original file already lists (CUAD, LEDGAR, EDGAR, CourtListener) are not repeated.

---

## New datasets

**ACORD: An Expert-Annotated Retrieval Dataset for Legal Contract Drafting**
Steven H. Wang, Maksim Zubkov, Kexin Fan, Sarah Harrell, Yuyang Sun, Wei Chen, Andreas Plesner, Roger Wattenhofer (The Atticus Project, ETH Zurich). ACL 2025 (main conference). arXiv January 2025.
https://arxiv.org/abs/2501.06582 · https://aclanthology.org/2025.acl-long.1206
ACORD is a clause retrieval benchmark that lawyers annotated. It has 114 queries and more than 126,000 query-clause pairs, each rated from 1 to 5 stars. It covers hard clause types such as limitation of liability, indemnification, change of control, and most favored nation. Use it in Project 3 as a graded retrieval test set (NDCG, not only hit rate). It also helps Project 6, because the playbook lookup is a clause retrieval task.

**AI Hallucination Cases database**
Damien Charlotin (HEC Paris). Public database, updated continuously (2025–2026).
https://www.damiencharlotin.com/hallucinations/
This database records court decisions that found hallucinated content, such as invented cases or false quotes, in filings. On 2026-10-10 it lists more than 2,000 decisions from many jurisdictions. It has a CSV download under CC BY 4.0. Use real failure types from it to build the "invented citation" class in the Project 4 failure taxonomy. Use the case counts in the Project 8 risk summary for managers.

---

## New benchmarks

**LEXam: Benchmarking Legal Reasoning on 340 Law Exams**
Yu Fan, Jingwei Ni, Jakob Merane, Yang Tian, Yoan Hermstrüwer, and others (University of Zurich, ETH Zurich). ICLR 2026. arXiv May 2025, last revised April 2026.
https://arxiv.org/abs/2505.12864
LEXam has 7,537 law exam questions from 116 courses, in English and German. It has open long-form questions and multiple-choice questions. The open questions include guidance on the expected reasoning steps, such as issue spotting and rule application. Current models do badly on multi-step open questions. Use its rubric method as a model for the Project 4 judge prompt and the Project 7 report checklist.

**ContractEval: Benchmarking LLMs for Clause-Level Legal Risk Identification in Commercial Contracts**
Shuang Liu, Zelong Li, Ruoyun Ma, Haiyan Zhao, Mengnan Du. Natural Legal Language Processing Workshop (NLLP) 2025. arXiv August 2025.
https://arxiv.org/abs/2508.03080 · https://aclanthology.org/2025.nllp-1.19
ContractEval compares open-weight and proprietary models on clause-level risk identification built on CUAD. It reports correctness and output quality for many open models, including quantized versions. This is the same question as the Project 2 on-prem decision. Use its results as an external reference point for your own accuracy-against-quantization table.

**LegalAgentBench: Evaluating LLM Agents in Legal Domain**
Haitao Li, Junjie Chen, Jingli Yang, Qingyao Ai, and others (Tsinghua University). ACL 2025 (main conference). arXiv December 2024.
https://aclanthology.org/2025.acl-long.116 · https://arxiv.org/abs/2412.17259
LegalAgentBench gives 300 annotated agent tasks, 17 legal corpora, and 37 tools. The tasks are Chinese law, but the method is general. It measures a progress rate from intermediate steps, not only the final result. Use the progress-rate idea in Project 6 to score partial success of the contract review agent.

**Vals Legal AI Report (VLAIR) — February 2025 and October 2025 (Legal Research)**
Vals AI, with Legaltech Hub. Independent industry evaluation. February 2025 and October 2025.
https://www.vals.ai/industry-reports/vlair-2-27-25 · https://vals.ai/industry-reports/vlair-10-14-25
The February 2025 report tested legal AI products on seven tasks with a lawyer control group. Tasks include data extraction, document Q&A, redlining, and EDGAR research. Lawyers scored higher than all tools on redlining. The October 2025 report tested 200 U.S. legal research questions against a lawyer baseline. Use the task list and the lawyer-baseline method in Project 6 (redlines) and Project 8 (the comparison in the manager summary).

---

## New papers by project

### Project 3 and Project 4 — Legal RAG and evaluation

**Hallucination-Free? Assessing the Reliability of Leading AI Legal Research Tools**
Varun Magesh, Faiz Surani, Matthew Dahl, Mirac Suzgun, Christopher D. Manning, Daniel E. Ho (Stanford RegLab, Stanford HAI). Journal of Empirical Legal Studies, vol. 22, p. 216. June 2025.
https://law.stanford.edu/publications/hallucination-free-assessing-the-reliability-of-leading-ai-legal-research-tools/
This is the peer-reviewed version of the 2024 preprint. It is the first preregistered test of commercial legal RAG tools. Tools from LexisNexis and Thomson Reuters hallucinated in 17% to 33% of queries. The paper also defines a hallucination type where the citation exists but does not support the claim. Add this "misgrounded citation" class to the Project 4 taxonomy. The exact citation check in Project 4 does not find it, so you need a support check too.

### Project 5 and Project 8 — Professional duties and citation checks

**R (Ayinde) v London Borough of Haringey; Al-Haroun v Qatar National Bank [2025] EWHC 1383 (Admin)**
High Court of England and Wales, Divisional Court. Judgment, 6 June 2025.
https://www.judiciary.uk/judgments/ayinde-v-london-borough-of-haringey-and-al-haroun-v-qatar-national-bank/
The court found false citations that AI tools produced in two cases. It stated that lawyers must check each authority against a reliable source. The likely result of a failure is referral to a professional regulator. Quote this judgment in the Project 8 governance summary. It explains why the citation check in Project 4 must have a zero threshold.

---

## New tools

No new open-source tool met the quality rule for this file. The general tools in the original file (Ollama, llama.cpp, LangGraph, OpenTelemetry) still apply.

---

## Regulation and standards changes

**EU AI Act — GPAI obligations and the Digital Omnibus on AI (Regulation (EU) 2026/1744)**
European Parliament and Council. GPAI obligations apply from 2 August 2025. Regulation (EU) 2026/1744 adopted 8 July 2026, published in the Official Journal 24 July 2026.
https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202601744
**2026 change:** The Digital Omnibus moves the high-risk obligations for Annex III systems from 2 August 2026 to 2 December 2027. Obligations for Annex I systems (AI in regulated products) move to 2 August 2028. AI used by courts to research and interpret facts and law is an Annex III area. Update the dates in the Project 8 risk and governance summary. A contract review tool that helps a lawyer is usually not Annex III, but state your classification and the reason.

**OWASP Top 10 for Agentic Applications 2026**
OWASP GenAI Security Project, Agentic Security Initiative. December 2025.
https://genai.owasp.org/2025/12/09/owasp-top-10-for-agentic-applications-the-benchmark-for-agentic-security-in-the-age-of-autonomous-ai/
This list has ten agent risks, ASI01 to ASI10. Examples are agent goal hijack, tool misuse, memory and context poisoning, and human-agent trust exploitation. Map the Project 6 approval gates and the Project 5 injection tests to these IDs. A hidden instruction in a counterparty contract is an example of goal hijack.

**OWASP GenAI LLM Top 10 2026**
OWASP GenAI Security Project. August 2026.
https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/
**2026 change:** This is the new edition of the OWASP Top 10 for LLM Applications. It maps each risk to NIST, MITRE ATLAS, and CWE. Projects 5 and 8 map attacks to the OWASP list. Use the 2026 IDs and record the edition in the attack test report.

**Proposed U.S. Federal Rule of Evidence 707 (machine-generated evidence)**
Judicial Conference of the United States, Advisory Committee on Evidence Rules. Published for comment 2025; comment period closed 16 February 2026; committee report May 2026.
https://www.uscourts.gov/sites/default/files/document/advisory_committee_on_evidence_rules_may_2026.pdf
The proposed rule applies the Rule 702 reliability test to AI output that a party offers without an expert. **2026 status:** In May 2026 the committee revised the text and did not recommend final approval. It will study the rule further, together with deepfakes. The rule is not in force. Mention it in the Project 8 summary as a possible future requirement for any AI output that goes into evidence.
