# Addendum (2025–2026): Industrial Maintenance and Manufacturing Portfolio Projects

**Created/updated:** 2026-10-10

This addendum extends `portfolio-projects-manufacturing.md`. It lists datasets, benchmarks, papers, models, and regulation changes from 2025 and 2026 that are relevant to the eight projects. The check date is 2026-10-10. The original file is not changed. Each item was checked against its official page or its arXiv record.

The selection rule is strict. Each item comes from a peer-reviewed venue, a strong lab, an official technical report, or an official regulator. Items with low impact are not included.

---

## New datasets

**FailureSensorIQ: A Multi-Choice QA Dataset for Understanding Sensor Relationships and Failure Modes**
Constantinides, Patel, Lin, Guerrero, Patil, and Kalagnanam, IBM Research. NeurIPS 2025 Datasets and Benchmarks Track. June 2025.
Paper: https://arxiv.org/abs/2506.03278 · Code and data: https://github.com/IBM/FailureSensorIQ
The dataset asks which sensors detect a failure mode, and which failure modes a sensor can detect. It covers 10 industrial asset types, 55 failure modes, and 53 sensors. Use it to measure the domain knowledge of the teacher and student models in Project 2. Use it also to check the "likely fault" step of the Project 6 agent.

---

## New benchmarks

**AssetOpsBench: Benchmarking AI Agents for Task Automation in Industrial Asset Operations and Maintenance**
Patel et al., IBM Research. arXiv. June 2025.
Paper: https://arxiv.org/abs/2506.03828 · Code: https://github.com/IBM/AssetOpsBench · Data: https://huggingface.co/datasets/ibm-research/AssetOpsBench
The benchmark has human-written maintenance queries, a simulated IoT environment, and four domain agents (for example, IoT data, failure modes, time-series forecasting, and work orders). It compares two agent designs: tools as agents, and plan-then-execute. It also finds new failure modes automatically. Use it as the reference environment for Project 6. Use its two designs as the "workflow against agent" and "single against multi-agent" comparisons in Projects 6 and 7.

**ViDoRe V3: A Comprehensive Evaluation of Retrieval Augmented Generation in Complex Real-World Scenarios** — 2026
Loison, Macé, Edy, Xing, Faysse, et al. (ViDoRe team, Illuin Technology, with co-authors). arXiv. January 2026.
Paper: https://arxiv.org/abs/2601.08620 · Industrial subset: https://huggingface.co/datasets/vidore/vidore_v3_industrial
ViDoRe V3 tests RAG over visually rich pages: tables, charts, and images. It has about 26,000 pages, 3,099 human-verified queries, and bounding-box labels for the answer source. It has an industrial subset. The paper finds that visual retrievers and late-interaction models do better than text retrievers on these pages. Use the industrial subset in Project 3 to compare a text parser against a page-image retriever on manuals with diagrams. Use the bounding-box labels in Project 4 to check citations to the exact page area.

**ViDoRe Benchmark V2: Raising the Bar for Visual Retrieval**
Macé, Loison, and Faysse. arXiv. May 2025.
https://arxiv.org/abs/2505.17166
V2 adds long queries and cross-document queries to the first ViDoRe benchmark. It is a smaller first step before V3 for Project 3.

---

## New papers by project

**Projects 2, 6, and 7 — FailureSensorIQ and AssetOpsBench (above).** These two IBM Research works are the main 2025 results for LLMs in asset maintenance. They give tasks, data, and agent designs that map directly to fault classification (Project 2), alarm-to-work-order planning (Project 6), and failure investigation (Project 7).

**Projects 3 and 4 — ViDoRe V3 (above).** It gives a measured answer to a design question in Project 3: when a text parser loses data in tables and diagrams, a visual retriever can find the correct page.

No 2025–2026 item of sufficient quality was found that is specific to Project 1 (maintenance log normalization) or Project 5 (multi-plant access control). The original references stay valid for these projects.

---

## New tools and models

**Granite-Docling-258M**
IBM Research. Official model release, Apache 2.0 license. September 2025.
Model: https://huggingface.co/ibm-granite/granite-docling-258M · Library: https://github.com/docling-project/docling
Granite-Docling is a small vision-language model that converts document pages into structured output. It keeps tables, equations, code, and layout. It runs with the Docling library. Use it in Project 3 to parse specification tables and troubleshooting tables from manuals. It is small enough to run on a local server, which helps the offline need in Project 8.

**Chronos-2: From Univariate to Universal Forecasting**
Ansari, Shchur, Küken, et al., Amazon. arXiv. October 2025.
Paper: https://arxiv.org/abs/2510.15821 · Code: https://github.com/amazon-science/chronos-forecasting · Model: https://huggingface.co/amazon/chronos-2
Chronos-2 is a 120M-parameter time-series model. It forecasts without task-specific training (zero-shot), and it supports multivariate series and covariates. It runs on CPU and GPU. Give it to the Project 6 agent as a "forecast sensor trend" tool. Test it on the NASA C-MAPSS data from the original file. Compare it with a simple threshold rule.

---

## Regulation and standards changes

**EU Cyber Resilience Act: reporting obligations start** — 2026
European Commission. Regulation (EU) 2024/2847. Reporting obligations apply from 11 September 2026.
https://digital-strategy.ec.europa.eu/en/policies/cra-reporting
From 11 September 2026, manufacturers of products with digital elements must report actively exploited vulnerabilities and severe incidents. An early warning is due in 24 hours, and a full notification in 72 hours. The full product requirements apply from December 2027. An edge gateway with a local model (Project 2) is a product with digital elements. Add a vulnerability handling process and a software bill of materials to the Project 2 and Project 8 deployment notes.

**EU AI Act: Digital Omnibus on AI (Regulation (EU) 2026/1744)** — 2026
European Parliament and Council. Official Journal of the EU. Published 24 July 2026.
https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202601744
The omnibus moves the high-risk dates. High-risk systems in Annex III now apply from 2 December 2027. AI that is a safety component of a product under Annex I law (this includes machinery) now applies from 2 August 2028. The Article 50 transparency duties apply from 2 August 2026. In the Project 8 governance notes, state whether the assistant is a safety component. A system that only helps a technician and does not control a machine is usually not a safety component.

**EU Machinery Regulation (EU) 2023/1230: application date**
European Parliament and Council. Regulation adopted in 2023. It applies from 20 January 2027.
https://eur-lex.europa.eu/eli/reg/2023/1230/oj
The regulation replaces the Machinery Directive. It adds requirements for safety components with self-evolving behaviour, such as machine learning, and for protection against corruption of safety software. It is not new in 2025–2026, but its application date is close. Use it in Project 5 and Project 8 to explain why the assistant must not change machine safety settings, and why a person must approve each procedure.

---

## Items not included

These items were found but not included, because they did not meet the quality rule or could not be fully confirmed: smaller 2025–2026 arXiv papers on maintenance chatbots and industrial RAG without a strong-lab source or venue, and vendor "industrial copilot" product pages.
