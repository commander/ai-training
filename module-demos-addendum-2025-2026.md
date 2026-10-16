# Addendum (2025–2026): Module Demos — Descriptions and Instructions

**Created/updated:** 2026-10-10

This file extends `module-demos.md`. It lists new tools, papers, and standards from 2025 and 2026 that apply to the demos. The check date is 2026-10-10. Each item has a unit number. Use the items to update the setup, the steps, or the slides of that unit. Items from 2026 have the tag **[2026]**.

---

# Module 1 — Foundations (Units 1–7)

## Unit 2 — Inference control

**Defeating Nondeterminism in LLM Inference**
Horace He and Thinking Machines Lab. Thinking Machines Lab blog (Connectionism). September 2025.
https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/
The post shows that the main cause of different outputs at temperature 0 is a lack of batch invariance in the kernels. Server load changes the batch size, and the batch size changes the numerical result. Use it to update the slide "why temperature 0 is not deterministic" in Unit 2. The post also gives batch-invariant kernels that make outputs identical.

## Unit 4 — Long context and failure modes

**Context Rot: How Increasing Input Tokens Impacts LLM Performance**
Kelly Hong, Anton Troynikov, Jeff Huber. Chroma technical report. July 2025.
https://www.trychroma.com/research/context-rot
Code: https://github.com/chroma-core/context-rot
The report tests 18 models and shows that accuracy decreases as input length increases, also on simple tasks. It also measures the effect of distractors and of needle-question similarity. Use it as a second source for the accuracy-by-depth chart and the 5-vs-20-document test.

**NoLiMa: Long-Context Evaluation Beyond Literal Matching**
Ali Modarressi, Hanieh Deilamsalehy, Franck Dernoncourt, Trung Bui, Ryan A. Rossi, Seunghyun Yoon, Hinrich Schütze. ICML 2025. February 2025 (arXiv).
https://arxiv.org/abs/2502.05167
Code and data: https://github.com/adobe-research/NoLiMa
NoLiMa removes word overlap between the needle and the question. At 32K tokens, 10 of 12 tested models drop below 50% of their short-context score. Add one needle with no literal match to the Unit 4 demo. It shows that a simple needle test gives a result that is too high.

## Unit 5 — Fine-tuning: decision and QLoRA hands-on

**Unsloth reinforcement learning (GRPO) support**
Unsloth AI. Official blog and documentation. February 2025, with later updates.
https://unsloth.ai/blog/r1-reasoning
https://docs.unsloth.ai/basics/reasoning-grpo-and-rl
Unsloth added GRPO training with low VRAM use. A small model (1.5B) trains on approximately 7 GB of VRAM. Use it for an optional stretch step in Unit 5: show that reinforcement learning with a reward function is a different tool from supervised fine-tuning.

## Unit 6 — SLMs, edge, and on-prem

**gpt-oss-120b and gpt-oss-20b**
OpenAI. Official release and model card. August 2025.
https://openai.com/index/introducing-gpt-oss/
These are open-weight Mixture-of-Experts models with an Apache 2.0 license. The 20B model runs in 16 GB of memory and supports tool use and three reasoning levels. Add it to the Unit 6 comparison table as a strong local option between a 1B–3B model and a frontier API.

---

# Module 2 — Retrieval-Augmented Generation (Units 8–11)

## Unit 8 — Chunking and embeddings

**MMTEB: Massive Multilingual Text Embedding Benchmark**
Kenneth Enevoldsen and many contributors (MTEB community). ICLR 2025. February 2025 (arXiv).
https://arxiv.org/abs/2502.13595
MMTEB expands MTEB to more than 500 tasks in more than 250 languages. It includes long-document retrieval, code retrieval, and instruction-following tasks. Use the current MTEB leaderboard (MMTEB tasks) when you select the two embedding models for Unit 8. For multilingual corpora, filter the leaderboard by language.

## Units 8 and 10 — Embeddings and reranking

**Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models**
Qwen Team, Alibaba. Technical report. June 2025.
https://arxiv.org/abs/2506.05176
The series has embedding models and reranker models at 0.6B, 4B, and 8B. The models use the Apache 2.0 license. Use the 0.6B and 8B embedding models as the "small" and "large" pair in Unit 8. Use the Qwen3 reranker as an open alternative to the BGE reranker in Unit 10.

---

# Module 3 — Agents (Units 12–15)

## Units 12 and 15 — Agent anatomy and multi-agent orchestration

**OpenAI Agents SDK and Responses API**
OpenAI. Official announcement "New tools for building agents". March 2025.
https://openai.com/index/new-tools-for-building-agents/
Docs: https://openai.github.io/openai-agents-python/
The SDK has four main parts: agents, handoffs, guardrails, and tracing. It replaces the experimental Swarm library. Add it to the Unit 15 framework comparison slide next to LangGraph, CrewAI, and AutoGen. Note that the Responses API replaces the Assistants API.

## Unit 13 — Tools and function calling

**Model Context Protocol specification, version 2025-11-25**
MCP maintainers (Anthropic and community). Official specification. November 2025.
https://modelcontextprotocol.io/specification/2025-11-25/changelog
This version adds guidance on tool names, URL-mode elicitation, tool calls inside sampling, and experimental tasks for long-running requests. It also improves authorization (OpenID Connect discovery, Client ID Metadata Documents). Use the tool-name guidance when you write tool schemas in Unit 13.

**Model Context Protocol specification, version 2026-07-28** **[2026]**
MCP maintainers. Official specification. July 2026.
https://modelcontextprotocol.io/specification/2026-07-28/changelog
This is the largest change since the first release. The protocol is now stateless: the `initialize` handshake and protocol sessions are removed. Tasks move to an official extension. Roots, Sampling, and Logging are deprecated. The specification also defines OpenTelemetry trace context in `_meta` (Unit 18). If a demo uses an MCP server, pin the protocol version. Test the demo against the 2026-07-28 SDKs before you deliver it.

## Unit 14 — Memory, state, and human-in-the-loop

**LangGraph 1.0**
LangChain. Official release announcement. October 2025.
https://www.langchain.com/blog/langchain-langgraph-1dot0
LangGraph 1.0 is the first stable major version. It has durable execution, checkpointing, and human-in-the-loop interrupts, with no breaking changes until 2.0. Pin LangGraph 1.x for the Unit 14 checkpoint and approval demo.

**Effective context engineering for AI agents**
Anthropic Applied AI team. Anthropic Engineering blog. September 2025.
https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
The post describes how to select and maintain the tokens in an agent's context. It covers compaction, structured notes (memory outside the context), and sub-agents. Use it to extend the Unit 14 memory table with "compaction" and "external notes" as two more strategies.

## Unit 15 — Multi-agent orchestration

**Why Do Multi-Agent LLM Systems Fail?**
Mert Cemri, Melissa Z. Pan, Shuyi Yang, and others (UC Berkeley). arXiv. March 2025 (revised October 2025).
https://arxiv.org/abs/2503.13657
The paper gives MAST, a taxonomy of 14 failure modes in 3 groups: system design issues, inter-agent misalignment, and task verification. It uses traces from 7 popular multi-agent frameworks. It also releases more than 1,600 annotated traces (MAST-Data) and an LLM-as-judge annotator. Use the taxonomy to label the failures in the Unit 15 demo, for example the supervisor loop and the error propagation step.

---

# Module 4 — Production (Units 16–19)

## Unit 16 — Serving and streaming

The MCP 2026-07-28 specification (Unit 13) removes SSE stream resumability from the Streamable HTTP transport. A broken stream loses the in-flight request, and the client must send it again. This applies only to MCP. Browser SSE in the Unit 16 demo still reconnects automatically. Tell the audience about this difference if a demo streams through MCP.

## Unit 17 — Guardrails and prompt injection

**OWASP Top 10 for Agentic Applications for 2026**
OWASP GenAI Security Project. Official standard. December 2025.
https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
This list has ten risk categories (ASI01–ASI10) for agents that plan, keep memory, and call tools. It maps each risk to real incidents from 2025. Map the Unit 17 attacks to this list as well as to the OWASP Top 10 for LLM Applications. OWASP published the 2026 edition of that list in August 2026 (https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/) **[2026]**. Use the 2026 IDs on the OWASP mapping slide. Use it for the tool and privilege-limit layer.

**LlamaFirewall: An open source guardrail system for building secure AI agents**
Sahana Chennabasappa and others (Meta). arXiv. May 2025.
https://arxiv.org/abs/2505.03574
Code: https://github.com/meta-llama/PurpleLlama
LlamaFirewall has three layers: Prompt Guard 2 (a small jailbreak classifier, 86M and 22M), AlignmentCheck (an auditor of the agent's reasoning), and CodeShield (static analysis of generated code). Use Prompt Guard 2 as the Unit 17 input classifier. Use AlignmentCheck to show a layer that can find indirect injection after retrieval.

**Defeating Prompt Injections by Design (CaMeL)**
Edoardo Debenedetti, Ilia Shumailov, Tianqi Fan, Jamie Hayes, Nicholas Carlini, Daniel Fabian, Christoph Kern, Chongyang Shi, Andreas Terzis, Florian Tramèr (Google, Google DeepMind, ETH Zurich). arXiv. March 2025.
https://arxiv.org/abs/2503.18813
Code: https://github.com/google-research/camel-prompt-injection
CaMeL gets the control flow from the trusted user query only. Untrusted data cannot change which tools run. It solves 77% of AgentDojo tasks with provable security. Use it on the Unit 17 slide to explain the difference between "detect the attack" and "design the system so that the attack cannot act".

**Design Patterns for Securing LLM Agents against Prompt Injections**
Luca Beurer-Kellner, Florian Tramèr, and others (ETH Zurich, Google, Microsoft, IBM, Invariant Labs, and others). arXiv. June 2025.
https://arxiv.org/abs/2506.08837
The paper gives six design patterns, for example plan-then-execute and dual LLM. The common rule: after an agent reads untrusted input, that input must not be able to start a consequential action. Use it to explain the privilege-limit step (Step 4) in Unit 17.

**The Attacker Moves Second: Stronger Adaptive Attacks Bypass Defenses Against LLM Jailbreaks and Prompt Injections**
Milad Nasr, Nicholas Carlini, Chawin Sitawarin, Sander V. Schulhoff, and others (OpenAI, Anthropic, Google DeepMind, and others). arXiv. October 2025.
https://arxiv.org/abs/2510.09023
The authors use adaptive attacks against 12 recent defenses. Most defenses fail with attack success rates above 90%, although they reported near-zero rates. Use it to support the Unit 17 goal: no single defense is sufficient, and a fixed attack list gives a result that is too optimistic.

## Unit 18 — Observability and cost

**OpenTelemetry Semantic Conventions for Generative AI (model, agent, and tool spans)**
OpenTelemetry GenAI Special Interest Group. Official specification (status: development). 2025, with updates in 2026.
https://opentelemetry.io/docs/specs/semconv/gen-ai/
Agent spans: https://opentelemetry.io/docs/specs/semconv/gen-ai/gen-ai-agent-spans/
The conventions define standard `gen_ai.*` attributes and span types for model calls, agent runs (`invoke_agent`), and tool calls. Version 1.37.0 (2025) replaces `gen_ai.system` with `gen_ai.provider.name`. Use these attribute names in the Unit 18 instrumentation. Then the traces work in Phoenix, LangSmith, and other backends without changes. The conventions are not yet stable, so pin the version.
