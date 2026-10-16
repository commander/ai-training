# Addendum (2025–2026): Portfolio Projects — Application Security Domain

**Created/updated:** 2026-10-10

This addendum extends `portfolio-projects.md` (application security and vulnerability management). It lists new datasets, benchmarks, papers, tools, and standards from 2025 and 2026. The check date is 2026-10-10. Each item was confirmed by search or by a fetch of the source page. Items marked **[2026]** were published or changed in 2026.

---

## New datasets

**PrimeVul: Vulnerability Detection with Code Language Models: How Far Are We?**
Ding, Fu, Ibrahim, Sitawarin, Chen, Alomair, Wagner, Ray, Chen. ICSE 2025 (arXiv first version March 2024). https://arxiv.org/abs/2403.18624
PrimeVul is a C/C++ vulnerability dataset with better labels, strict de-duplication, and a time-based split. Models with high F1 on older datasets such as Big-Vul score very low on PrimeVul. Use its de-duplication and time-split method for the train/test split in Project 1 and Project 2.

**CyberGym: Evaluating AI Agents' Real-World Cybersecurity Capabilities at Scale** **[2026 venue]**
Wang, Shi, He, Cai, Zhang, Song (UC Berkeley). arXiv June 2025; ICLR 2026. https://arxiv.org/abs/2506.02548
CyberGym has 1,507 real vulnerabilities from 188 open-source projects. The agent gets a text description and the code. It must write a proof-of-concept input that reproduces the bug. Use the vulnerability descriptions as realistic input text for the Project 6 triage agent.

## New benchmarks

**CVE-Bench: A Benchmark for AI Agents' Ability to Exploit Real-World Web Application Vulnerabilities**
Zhu et al. ICML 2025 (arXiv March 2025). https://arxiv.org/abs/2503.17332
CVE-Bench puts critical-severity web CVEs in sandboxed containers. The best agent framework in the paper solves up to 13% of them. Use it in Project 8 to explain why an agent must not get write or exploit tools without an approval gate.

**SEC-bench: Automated Benchmarking of LLM Agents on Real-World Software Security Tasks**
Lee, Zhang, Lu, Zhang. NeurIPS 2025 (arXiv June 2025). https://arxiv.org/abs/2506.11791
SEC-bench builds reproducible vulnerability instances automatically, at about USD 0.87 per instance. It tests proof-of-concept generation and patch generation. Agents reach at most 18% and 34% success. Its pipeline is a model for the Project 6 test set of 30 reports with expected outcomes.

**BaxBench: Can LLMs Generate Correct and Secure Backends?**
Vero, Mündler, Chibotaru, Raychev, Baader, Jovanović, He, Vechev (ETH Zurich, LogicStar.ai). ICML 2025 (arXiv February 2025). https://arxiv.org/abs/2502.11844
BaxBench has 392 backend tasks across 14 frameworks. Each task has functional tests and security exploits. No model in the paper gets above 60% correct code, and more than half of the correct programs are exploitable. Use it in Project 7 to test whether the "remediation steps are specific" check also catches insecure fix code.

**CyberSecEval 4 and AutoPatchBench**
Meta (Purple Llama). April 2025. https://github.com/meta-llama/PurpleLlama
CyberSecEval 4 adds AutoPatchBench: 136 real C/C++ fuzzing crashes from the ARVO dataset, with verified fixes. A Lite subset has 113 samples where the root cause is in one function. Use it to measure a patch-suggestion tool in Project 6, or as an extra row in the Project 1 frontier comparison.

**CyberSOCEval: Benchmarking LLMs Capabilities for Malware Analysis and Threat Intelligence Reasoning**
Meta and CrowdStrike. arXiv September 2025. https://arxiv.org/abs/2509.20166
CyberSOCEval has two parts: malware analysis and threat intelligence reasoning. The paper reports that reasoning models do not get the same gain here as in coding or mathematics. Use the threat intelligence part as an external quality check for the Project 7 report writer.

## New papers by project

### Project 5 — Multi-tenant knowledge assistant

**Defeating Prompt Injections by Design (CaMeL)**
Debenedetti, Shumailov, Fan, Hayes, Carlini, Fabian, Kern, Shi, Terzis, Tramèr (Google DeepMind, ETH Zurich). arXiv March 2025. https://arxiv.org/abs/2503.18813
CaMeL takes the control flow and data flow from the trusted user query only. Retrieved untrusted data cannot change which tools run. It solves 77% of AgentDojo tasks with provable security, against 84% with no defense. Add it as a defense layer in the Project 5 attack-by-defense-layer table. Code: https://github.com/google-research/camel-prompt-injection

### Project 6 — Vulnerability triage agent

**Design Patterns for Securing LLM Agents against Prompt Injections**
Beurer-Kellner, Buesser, Creţu, Debenedetti, Dobos, Fabian, Fischer, Froelicher, Grosse, Naeff, Ozoani, Paverd, Tramèr, Volhejn (Invariant Labs, IBM, EPFL, ETH Zurich, Google, Microsoft, and others). arXiv June 2025. https://arxiv.org/abs/2506.08837
The paper gives six agent design patterns, for example plan-then-execute and action-selector. The main rule: after the agent reads untrusted input, that input must not be able to start a consequential action. Use these patterns when you draw the Project 6 state graph. Record the pattern you choose in the decision log.

## New tools

**LlamaFirewall**
Meta. arXiv April 2025 (paper dated 29 April 2025). https://arxiv.org/abs/2505.03574 — code: https://github.com/meta-llama/PurpleLlama/tree/main/LlamaFirewall
LlamaFirewall is an open-source guardrail framework for agents. It includes a prompt injection classifier (PromptGuard 2), an agent alignment check on the reasoning trace, and a code scanner (CodeShield). You can add custom regular-expression or prompt-based scanners. Use it as one layer in Project 5 and Project 8.

**DARPA AI Cyber Challenge (AIxCC) cyber reasoning systems**
DARPA and ARPA-H. Final results, August 2025. https://www.darpa.mil/news/2025/aixcc-results
Seven teams built systems that find and patch bugs in open-source code. Team Atlanta, Trail of Bits, and Theori took the first three places. DARPA said all seven systems will be released as open source under OSI-approved licenses. Study one released system as a reference architecture for Project 6 and Project 7 (tool use, budget limits, patch validation).

**CodeMender (reference system, not open source)**
Google DeepMind. Blog post, October 2025. https://deepmind.google/discover/blog/introducing-codemender-an-ai-agent-for-code-security/
CodeMender is an agent that finds and fixes vulnerabilities with Gemini models, static and dynamic analysis, fuzzing, and SMT solvers. DeepMind reports 72 fixes sent to open-source projects. A human reviews every patch before submission. Use it in Project 6 as a real example of a validation step before a human approval gate.

## Regulation and standards changes

**OWASP Top 10 for LLM Applications — 2025 edition**
OWASP GenAI Security Project. Published late 2024 for the 2025 cycle. https://genai.owasp.org/llm-top-10/
Projects 5 and 8 map attacks to this list. OWASP published a newer edition, "OWASP GenAI LLM Top 10 2026", in August 2026 (https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/) **[2026]**. Use the 2026 edition for new work, and record the edition in the attack test report. In the 2025 edition, the new or changed entries are LLM07 System Prompt Leakage, LLM08 Vector and Embedding Weaknesses, LLM09 Misinformation, and LLM10 Unbounded Consumption. LLM08 applies directly to the Project 5 tenant isolation tests. LLM10 applies to the Project 8 per-user cost limit.

**OWASP Top 10 for Agentic Applications 2026**
OWASP GenAI Security Project. December 2025 (released 9 December 2025). https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
This list has ten agent-specific risks (ASI01–ASI10). It covers agents that plan, keep memory, call tools, and act with delegated access. Map the Project 6 tool errors and approval gates to this list, in addition to the LLM Top 10.

**2025 CWE Top 25 Most Dangerous Software Weaknesses**
MITRE and CISA. December 2025. https://cwe.mitre.org/top25/archive/2025/2025_cwe_top25.html
The 2025 list is based on 39,080 CVE records. Cross-site scripting, SQL injection, and CSRF are the top three. Missing Authorization moved up to fourth. Use the 2025 list to balance CWE classes in the Project 1 dataset and in the Project 3 corpus.

**Model Context Protocol authorization specification (2025-06-18 and 2025-11-25 revisions)**
Model Context Protocol project. June 2025 and November 2025. https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
MCP servers on HTTP transports act as OAuth 2.1 resource servers. Clients must use Resource Indicators (RFC 8707). Servers must not pass through tokens issued for other services. Follow this specification for the Project 6 stretch goal (tools exposed through an MCP server).

**CVE Program funding**
CISA and MITRE. April 2025 extension; status update **[2026]** March 2026. https://www.csoonline.com/article/4142600/cve-program-funding-secured-easing-fears-of-repeat-crisis.html
In April 2025 the MITRE contract almost expired. CISA used an 11-month option, and CVE Board members started the CVE Foundation. In March 2026, CVE Board minutes stated that funding continues past March 2026. Record this in the Project 8 risk summary as a dependency risk on public vulnerability data.

**NVD moves to risk-based CVE enrichment** **[2026]**
NIST. April 2026 (announced 15 April 2026). https://www.nist.gov/news-events/news/2026/04/nist-updates-nvd-operations-address-record-cve-growth
NIST now fully enriches only CVEs in the CISA KEV catalog, CVEs in federal government software, and CVEs in critical software (EO 14028). Other CVEs are marked "Lowest Priority – not scheduled for immediate enrichment". NIST also stops adding its own severity score when the CNA already gave one. Effect on Project 2: many new CVEs have no NVD CVSS score, CWE, or CPE. Use CNA scores as labels and record the label source. Effect on Project 6: the CVE lookup tool must handle records with no NVD severity.

**NIST IR 8596 (preliminary draft): Cybersecurity Framework Profile for Artificial Intelligence (Cyber AI Profile)**
NIST. December 2025. https://csrc.nist.gov/pubs/ir/8596/iprd
This draft maps AI-specific risks and controls to the NIST CSF 2.0 functions and categories. Use it to structure the one-page risk and governance summary in Project 8.

**EU AI Act — Digital Omnibus on AI, Regulation (EU) 2026/1744** **[2026]**
European Parliament and Council. Adopted 8 July 2026. Official Journal of the EU.
https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202601744
The regulation moves the high-risk obligations to 2 December 2027 (Annex III, stand-alone systems) and 2 August 2028 (Annex I, systems in regulated products). The earlier date was 2 August 2026. General-purpose AI obligations that started on 2 August 2025 do not change. Use these dates in the Project 8 governance summary.
