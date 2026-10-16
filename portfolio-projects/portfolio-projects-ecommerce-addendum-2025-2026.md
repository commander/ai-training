# Addendum (2025–2026): E-commerce Marketplace and Customer Support Portfolio Projects

**Created/updated:** 2026-10-10

This addendum extends `portfolio-projects-ecommerce.md`. It lists benchmarks, protocols, and regulation changes from 2025 and 2026 that are relevant to the eight projects. The check date is 2026-10-10. The original file is not changed. Each item was checked against its official page or its arXiv record.

The selection rule is strict. Each item comes from a peer-reviewed venue, a strong lab, an official specification, or an official regulator. Items with low impact are not included.

---

## New datasets

No new 2025–2026 e-commerce dataset met the quality rule. Many new shopping datasets appeared on arXiv in 2026, but most have no venue or adoption yet. The ESCI, Amazon review, and Bitext datasets in the original file stay the main data sources. The WebMall benchmark below includes product offers from Common Crawl that you can also use as catalog data.

---

## New benchmarks

**τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment**
Barres, Dong, Ray, Si, and Narasimhan, Sierra. arXiv. June 2025.
Paper: https://arxiv.org/abs/2506.07982 · Code: https://github.com/sierra-research/tau2-bench
τ²-Bench extends τ-bench. It has retail, airline, and telecom domains with policy documents, tools, and a simulated user. In the telecom domain, both the agent and the user can change the shared state. It reports pass^k, the rate at which the agent succeeds on all k runs of the same task. Use the retail domain as an external test for the Project 6 refund and return agent. Report pass^k next to your single-run success rate, because a support agent must be consistent.

**WebMall — A Multi-Shop Benchmark for Evaluating Web Agents**
Peeters, Steiner, Schwarz, Caspary, and Bizer, University of Mannheim. SIGIR 2026 (arXiv August 2025).
https://arxiv.org/abs/2508.13024
WebMall has four simulated shops with real product offers from Common Crawl. Tasks include product search, price comparison, and finding substitute or related products across shops. The best agents complete fewer than 65% of the tasks in some categories. Use its product search tasks to test Project 3 on queries with exact attributes. Use its multi-shop tasks to test whether the Project 8 assistant finds the correct offer.

---

## New papers by project

**Project 6 — τ²-Bench (above).** Its retail domain is the closest public match to the Project 6 refund and return agent. Its policy-following tasks show how to build test cases with a clear expected database state.

**Projects 3 and 8 — WebMall (above).** It is a peer-reviewed benchmark for product search and comparison by agents.

No 2025–2026 item of sufficient quality was found that is specific to Project 1 (intent and entity extraction), Project 2 (intent routing), Project 5 (seller data isolation), or Project 7 (listing generation). The original references stay valid for these projects.

---

## New tools and models

**Agentic Commerce Protocol (ACP)**
OpenAI and Stripe. Open specification. 29 September 2025.
Site: https://www.agenticcommerce.dev · Specification: https://github.com/agentic-commerce-protocol/agentic-commerce-protocol
ACP is an open standard for checkout between buyers, their AI agents, and businesses. It defines checkout sessions and a delegated payment step. Use its checkout and order objects as the schema for the Project 6 mock order API. Then your tools match a real interface.

**Agent Payments Protocol (AP2)**
Google, with payment and technology partners. Open protocol that builds on A2A. 16 September 2025.
Announcement: https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol · Documentation: https://ap2-protocol.org · Code: https://github.com/google-agentic-commerce/AP2
AP2 uses signed "mandates" to record what the user approved, such as an amount limit. This gives an audit record for each payment by an agent. Use the mandate idea for the Project 6 approval gate: store a signed approval record for each refund above the money threshold.

**Universal Commerce Protocol (UCP)** — 2026
Google and partners. Open-source protocol. 11 January 2026.
Site: https://ucp.dev · Announcement: https://developers.googleblog.com/en/under-the-hood-universal-commerce-protocol-ucp/
UCP is a common protocol for platforms, agents, and businesses for product discovery, checkout, and order management. Compare UCP and ACP in the Project 8 decision log if your assistant must work with external shopping agents.

---

## Regulation and standards changes

**EU AI Act: Article 50 transparency duties, and the Digital Omnibus on AI (Regulation (EU) 2026/1744)** — 2026
European Parliament and Council. Official Journal of the EU. Omnibus published 24 July 2026.
https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202601744
The Article 50 transparency duties apply from 2 August 2026. A chat assistant must tell the user that they talk to an AI system, unless this is clear from the context. AI-generated text and images must be marked in a machine-readable format. The omnibus gives a four-month transition for this marking on systems already on the market before 2 August 2026. The omnibus also moves the Annex III high-risk date to 2 December 2027. Add the AI disclosure to the Project 8 chat widget. Mark AI-generated listing text in Project 7.

**European Accessibility Act (Directive (EU) 2019/882): application date**
European Commission. Directive adopted in 2019. It applies from 28 June 2025.
https://commission.europa.eu/strategy-and-policy/policies/justice-and-fundamental-rights/disability/union-equality-strategy-rights-persons-disabilities-2021-2030/european-accessibility-act_en
From 28 June 2025, e-commerce services in the EU must meet accessibility requirements. This includes the chat interface, the checkout, and the support flow. Test the Project 8 chat widget for keyboard use and screen reader output. Record the result in the README.

---

## Items not included

These items were found but not included, because they did not meet the quality rule: 2026 arXiv shopping-agent benchmarks without a venue or clear adoption (for example, AgenticShop, EComAgentBench, MMShopBench, and RealWorldShop), and ShoppingBench and DeepShop (2025), which had no confirmed venue at the check date.
