# Addendum (2025–2026): Resource Guide — Iterative Depth

**Created/updated:** 2026-10-10

This file extends `genai-resources.md`. We checked the resources on 2026-10-10. Each item below is from 2025 or 2026 and is not in the original file. We include only official lab or vendor reports, peer-reviewed papers, papers from strong labs, recognized benchmarks, widely used tools, and courses from established providers. Items from 2026 have the tag **[2026]**.

---

# Pass 1 — The map

Add one item to the 90-minute map.

- **Context Rot: How Increasing Input Tokens Impacts LLM Performance**
  Chroma (Kelly Hong, Anton Troynikov, Jeff Huber). Technical report. Jul 2025.
  https://research.trychroma.com/context-rot
  The report tests 18 models on simple tasks at different input lengths. Accuracy drops as input grows, even when the task does not get harder. It gives data for the "context engineering" framing in item 2 of Pass 1. Code: https://github.com/chroma-core/context-rot

---

# Pass 3 — Depth, by unit

## Units 1–4 · Mechanics

**Tokenization, inference, and model internals (course)**

- **Stanford CS336: Language Modeling from Scratch** **[2026 edition available]**
  Percy Liang, Tatsunori Hashimoto. Stanford University. Spring 2025 and Spring 2026.
  https://cs336.stanford.edu
  The lectures are free on YouTube. They cover tokenization, architectures, GPUs, inference, and post-training. Use the tokenization and inference lectures for Units 1–2.

- **nanochat**
  Andrej Karpathy. Open-source repository. Oct 2025.
  https://github.com/karpathy/nanochat
  One small codebase trains a tokenizer, pretrains, does SFT and RL, and serves a chat UI. It is a good hands-on companion to the Karpathy videos in the original list.

**Inference control**

- **Defeating Nondeterminism in LLM Inference**
  Horace He and Thinking Machines Lab. Connectionism blog. Sep 2025.
  https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference
  This post answers the Unit 2 diagnostic question directly. The main cause of different outputs at temperature 0 is batch size. The server batch changes per request, and kernels that are not "batch-invariant" give different numbers.

**Attention** *(target L1)*

- **The Big LLM Architecture Comparison**
  Sebastian Raschka. Ahead of AI. Jul 2025.
  https://magazine.sebastianraschka.com/p/the-big-llm-architecture-comparison
  One article compares the attention and MoE choices in current open models (for example GQA, MLA, sliding-window attention). Use it to update the "Attention Is All You Need" picture to current models.

- **Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention**
  Jingyang Yuan et al. DeepSeek-AI and Peking University. ACL 2025 (best paper). Feb 2025.
  https://arxiv.org/abs/2502.11089
  It shows one way to make attention cost less than O(n²) during training and decoding. Read the abstract and Figure 1 only. It gives a good answer to "is the quadratic cost a fixed limit?"

**Long context**

- **NoLiMa: Long-Context Evaluation Beyond Literal Matching**
  Ali Modarressi et al. Adobe Research and LMU Munich. ICML 2025. Feb 2025.
  https://arxiv.org/abs/2502.05167
  Standard needle-in-a-haystack tests let the model match words. NoLiMa removes word overlap between question and needle. At 32K tokens, most tested models fall below 50% of their short-context score. Use it next to "Lost in the Middle" for the Day 3 chart.

## Unit 5 · Fine-tuning

- **LoRA Without Regret**
  John Schulman and Thinking Machines Lab. Connectionism blog. Sep 2025.
  https://thinkingmachines.ai/blog/lora/
  The study shows when LoRA matches full fine-tuning. Apply LoRA to all layers, including MLP layers, not only attention. Use a learning rate about 10 times the full fine-tuning rate. Hugging Face TRL has a recipe page: https://huggingface.co/docs/trl/main/en/lora_without_regret

- **Qwen3 Technical Report**
  Qwen Team, Alibaba. arXiv. May 2025.
  https://arxiv.org/abs/2505.09388
  The Unsloth fine-tuning guide now uses Qwen3 in its examples. Qwen3 has small dense models (0.6B, 1.7B, 4B) that fit a free T4. Read the model table before you change the lab base model from Qwen 2.5.

## Units 8–11 · RAG

**Embeddings & chunking**

- **MMTEB: Massive Multilingual Text Embedding Benchmark**
  Kenneth Enevoldsen et al. ICLR 2025. Feb 2025.
  https://arxiv.org/abs/2502.13595
  This is the large multilingual extension of MTEB. The current MTEB leaderboard uses it. It has more than 500 tasks in many languages. It supports the point that the "best" model depends on task and language.

- **Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models**
  Qwen Team, Alibaba. arXiv. Jun 2025.
  https://arxiv.org/abs/2506.05176
  Open embedding models and cross-encoder rerankers (0.6B, 4B, 8B) under Apache 2.0. One model family covers both the bi-encoder and the cross-encoder parts of the lab.

- **EmbeddingGemma: Powerful and Lightweight Text Representations**
  Google DeepMind. arXiv. Sep 2025.
  https://arxiv.org/abs/2509.20354
  A 300M-parameter embedding model for on-device and on-prem use. Use it when the audience asks for a small model that runs without a GPU.

**Hybrid & reranking**

- **On the Theoretical Limitations of Embedding-Based Retrieval** **[2026 venue]**
  Orion Weller, Michael Boratko, Iftekhar Naim, Jinhyuk Lee. Google DeepMind and Johns Hopkins University. ICLR 2026. arXiv Aug 2025.
  https://arxiv.org/abs/2508.21038
  The paper proves that embedding dimension limits which top-k document sets a single vector can return. Its LIMIT dataset is simple, but strong embedding models fail on it. This gives a formal reason for hybrid search and reranking in Unit 10.

- **Training and Finetuning Sparse Embedding Models with Sentence Transformers**
  Tom Aarsen, Arthur Bresnu. Hugging Face blog. Jul 2025.
  https://huggingface.co/blog/train-sparse-encoder
  Sentence Transformers now has a `SparseEncoder` class for learned sparse models such as SPLADE. Use it as the keyword-aware side of a hybrid search lab, next to BM25.

**Evaluation**

- **Demystifying Evals for AI Agents** **[2026]**
  Anthropic Engineering. Jan 2026.
  https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
  It defines tasks, trials, graders, transcripts, and harnesses. It separates capability evals from regression evals. It explains pass@k and pass^k for non-deterministic agents. Use it to extend the RAG eval lab to agents.

## Units 12–15 · Agents

- **Writing Effective Tools for AI Agents — Using AI Agents**
  Anthropic Engineering. Sep 2025.
  https://www.anthropic.com/engineering/writing-tools-for-agents
  This is the specific post for the original line "Anthropic's writing on tool design for agents". It covers tool selection, namespaces, token-efficient responses, and how to evaluate tools with an agent.

- **Model Context Protocol (MCP) specification, revision 2026-07-28** **[2026]**
  Model Context Protocol project. Jul 2026.
  https://modelcontextprotocol.io/specification/2026-07-28
  MCP is now the common protocol for tools. This revision makes the protocol stateless. It removes the `initialize` handshake and protocol sessions, and adds `server/discover`. Use this version, not older tutorials, for the Unit 13 lab. Changelog: https://github.com/modelcontextprotocol/modelcontextprotocol

- **Hugging Face MCP Course**
  Hugging Face. 2025.
  https://huggingface.co/learn/mcp-course
  A free course that builds MCP servers and clients end to end. Use it as hands-on practice for Unit 13.

- **Equipping Agents for the Real World with Agent Skills**
  Anthropic Engineering. Oct 2025.
  https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
  Skills are folders of instructions and scripts that an agent loads only when it needs them. This is a just-in-time context method for Unit 14. The format is now an open specification: https://agentskills.io

- **Effective Harnesses for Long-Running Agents**
  Anthropic Engineering. Nov 2025.
  https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
  It shows how an agent keeps state across many context windows with progress files and git history. It is a practical example for the Unit 14 memory question.

- **Harness Design for Long-Running Application Development** **[2026]**
  Anthropic Engineering. Mar 2026.
  https://www.anthropic.com/engineering/harness-design-long-running-apps
  A planner, generator, and evaluator agent build applications over multi-hour sessions. It compares compaction with full context resets. It also shows that a separate evaluator agent is easier to tune than self-critique.

- **How We Built Our Multi-Agent Research System**
  Anthropic Engineering. Jun 2025.
  https://www.anthropic.com/engineering/multi-agent-research-system
  A lead agent starts parallel subagents. Multi-agent runs used about 15 times more tokens than chat. Use this number for the Unit 15 cost question.

- **Why Do Multi-Agent LLM Systems Fail?**
  Mert Cemri, Melissa Z. Pan et al. UC Berkeley. NeurIPS 2025. arXiv Mar 2025.
  https://arxiv.org/abs/2503.13657
  The MAST taxonomy has 14 failure modes in 3 groups: specification, inter-agent misalignment, and task verification. It is based on 1,600+ annotated traces. Use it for the "teach the failure modes" slide.

- **τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment**
  Victor Barres et al. Sierra. arXiv. Jun 2025.
  https://arxiv.org/abs/2506.07982
  A recognized tool-use benchmark where both the user and the agent can act on the environment. Use it to show how tool-calling agents are measured.

- **Agentic AI (course)**
  Andrew Ng. DeepLearning.AI. 2025 (page date 30 Sep 2025; updated Aug 2026).
  https://www.deeplearning.ai/courses/agentic-ai
  The course teaches four patterns: reflection, tool use, planning, and multi-agent work. Its taxonomy matches the workflow patterns in Building Effective Agents.

## Units 16–18 · Production

- **JSONSchemaBench: A Rigorous Benchmark of Structured Outputs for Language Models**
  Saibo Geng et al. EPFL and Microsoft. arXiv. Jan 2025.
  https://arxiv.org/abs/2501.10868
  It compares constrained-decoding engines (for example Guidance, Outlines, XGrammar) on about 10,000 real JSON schemas. Use it to support the Instructor vs Outlines comparison with data.

- **OWASP Top 10 for Agentic Applications for 2026** **[2026 edition]**
  OWASP GenAI Security Project. Dec 2025.
  https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
  The risks have IDs ASI01–ASI10. They cover agents that plan, use tools, keep memory, and talk to other agents. It extends the OWASP Top 10 for LLM Applications. Use both lists in the guardrails session.

- **Defeating Prompt Injections by Design (CaMeL)**
  Edoardo Debenedetti et al. Google DeepMind and ETH Zurich. arXiv. Mar 2025.
  https://arxiv.org/abs/2503.18813
  A privileged LLM writes the plan from the trusted user request. A quarantined LLM reads untrusted data but has no tools. An interpreter tracks data flow and applies security policies. This is a design-level defense, not a filter.

- **Design Patterns for Securing LLM Agents against Prompt Injections**
  Luca Beurer-Kellner et al. ETH Zurich, Google, Microsoft, and others. arXiv. Jun 2025.
  https://arxiv.org/abs/2506.08837
  Six architecture patterns limit what an agent can do after it reads untrusted input. Ten case studies show the trade-off between security and utility. It answers the Unit 17 diagnostic question well.

- **Llama Guard 4**
  Meta. Model card. Apr 2025.
  https://huggingface.co/meta-llama/Llama-Guard-4-12B
  A 12B multimodal safety classifier for text and images. It replaces Llama Guard 3 for new labs.

- **Qwen3Guard Technical Report**
  Qwen Team, Alibaba. arXiv. Oct 2025.
  https://arxiv.org/abs/2510.14276
  Multilingual safety classifiers, including a streaming variant that checks tokens while the model generates them. Use it as an open alternative to Llama Guard.

---

# Outdated or changed resources

| Original item | Status on 2026-10-10 | Action |
|---|---|---|
| LangGraph docs `langchain-ai.github.io/langgraph` | The site says the docs moved to `docs.langchain.com`. LangGraph 1.0 shipped in Oct 2025. The prebuilt `create_react_agent` is deprecated. LangChain `create_agent` replaces it. | Use https://docs.langchain.com/oss/python/langgraph/overview and the v1 notes at https://docs.langchain.com/oss/python/releases/langgraph-v1 |
| AutoGen docs | The AutoGen README says AutoGen is in maintenance mode. It gets no new features. Microsoft Agent Framework 1.0 is the successor. | Use https://learn.microsoft.com/en-us/agent-framework/overview/ for the comparison slide. Migration guide: https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-autogen/ |
| Arize Phoenix `docs.arize.com/phoenix` | Redirects (301). | Use https://arize.com/docs/phoenix |
| LangSmith `docs.smith.langchain.com` | Redirects (308). | Use https://docs.langchain.com/langsmith |
| Outlines | The GitHub repository moved from `outlines-dev/outlines` to `dottxt-ai/outlines`. | Use https://github.com/dottxt-ai/outlines |
| "Anthropic's writing on tool design for agents" | No specific link in the original. | Use "Writing Effective Tools for AI Agents" (Sep 2025), above. |
| "Recent context rot work — search current papers" | Placeholder. | Use the Chroma Context Rot report and NoLiMa, above. |
| OWASP Top 10 for LLM Applications | OWASP published the 2026 edition ("OWASP GenAI LLM Top 10 2026") in August 2026. An agentic list now exists too. | Use https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/ and add the Agentic 2026 list. |
| MCP tutorials written for 2024–2025 spec versions | The 2026-07-28 revision removes the `initialize` handshake, protocol sessions, and SSE stream resumability. | Check the spec version before you teach MCP code. |
| Qwen 2.5 3B as the lab base model | Still works. The Unsloth guide now uses Qwen3 in its examples. | Test a small Qwen3 model as an option before Day 6. |
