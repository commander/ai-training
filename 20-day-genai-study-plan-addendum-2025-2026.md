# Addendum (2025–2026): Teaching Readiness Plan — Modular GenAI Training

**Created/updated:** 2026-10-10

This file extends `20-day-genai-study-plan.md`. We checked the resources on 2026-10-10. For each day, it lists 1–3 new resources from 2025 or 2026. We include only official lab or vendor reports, peer-reviewed papers, papers from strong labs, recognized benchmarks, widely used tools, and courses from established providers. Items from 2026 have the tag **[2026]**. If no item met this rule for a day, the day says so. Full notes for many items are in `genai-resources-addendum-2025-2026.md`.

---

# Days 1–4 — Foundations (L1/L2)

**D1 — Unit 1 (tokenization, KV cache)**
- **Stanford CS336: Language Modeling from Scratch** **[2026 edition available]** — Percy Liang, Tatsunori Hashimoto, Stanford. Spring 2025 and Spring 2026. https://cs336.stanford.edu — Watch the lecture on tokenization and the lecture on inference. The inference lecture covers KV cache size, which you compute by hand on D1.
- **nanochat** — Andrej Karpathy. Oct 2025. https://github.com/karpathy/nanochat — It includes a tokenizer you can train and inspect. Use it to show BPE on your own text.

**D2 — Unit 2 (inference control)**
- **Defeating Nondeterminism in LLM Inference** — Horace He and Thinking Machines Lab. Sep 2025. https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference — It explains why temperature 0 is not reproducible on a shared server. The cause is batch size that changes per request. Use it for the D2 determinism point.

**D3 — Units 3 + 4 (attention L1, long context)**
- **The Big LLM Architecture Comparison** — Sebastian Raschka, Ahead of AI. Jul 2025. https://magazine.sebastianraschka.com/p/the-big-llm-architecture-comparison — One article on attention variants in current models (GQA, MLA, sliding window). It is enough for L1.
- **Context Rot** — Chroma (Hong, Troynikov, Huber). Jul 2025. https://research.trychroma.com/context-rot — Tests 18 models and shows accuracy falls as input length grows. Put its results next to your needle-in-a-haystack chart.
- **NoLiMa: Long-Context Evaluation Beyond Literal Matching** — Modarressi et al., Adobe Research and LMU Munich. ICML 2025. https://arxiv.org/abs/2502.05167 — A needle test with no word overlap between question and needle. Add one such needle to your D3 test to show the harder case.

**D4 — Unit 7 (structured output)**
- **JSONSchemaBench** — Saibo Geng et al., EPFL and Microsoft. Jan 2025. https://arxiv.org/abs/2501.10868 — Compares constrained-decoding engines on 10K real JSON schemas. Use it to show that "constrained" does not mean every schema feature is supported.

---

# Days 5–8 — Fine-tuning block (L3)

**D5 — Theory & decision**
- **LoRA Without Regret** — John Schulman and Thinking Machines Lab. Sep 2025. https://thinkingmachines.ai/blog/lora/ — Shows what `r`, target layers, and learning rate do in practice. Key rules: apply LoRA to all layers, and use about 10 times the full fine-tuning learning rate.

**D6 — Environment**
- **Qwen3 Technical Report** — Qwen Team, Alibaba. May 2025. https://arxiv.org/abs/2505.09388 — Qwen3 has 0.6B and 1.7B instruct models, the class the plan asks for. Check them against your T4 time budget.
- **Gemma 3 Technical Report** — Gemma Team, Google DeepMind. Mar 2025. https://arxiv.org/abs/2503.19786 — Gemma 3 1B is a second small base model. Keep it as a fallback if your first choice fails on the T4.

**D7 — Full run & failure modes**
- **TRL "LoRA Without Regret" recipe** — Hugging Face TRL docs. 2025. https://huggingface.co/docs/trl/main/en/lora_without_regret — Gives working LoRA settings for `SFTTrainer`. A learning rate that is too low is one cause of a flat loss curve. Add it to the troubleshooting one-pager.

**D8 — Package the lab + Unit 6 (SLMs)**
- **Small Language Models are the Future of Agentic AI** — Peter Belcak et al., NVIDIA Research. Jun 2025. https://arxiv.org/abs/2506.02153 — Argues that most agent sub-tasks can run on small models. Use it for the "when is a 3B on-prem model right" discussion.
- **EmbeddingGemma** — Google DeepMind. Sep 2025. https://arxiv.org/abs/2509.20354 — A 300M embedding model for on-device use. It shows that the on-prem story also applies to the RAG stack.

---

# Days 9–12 — RAG (L3)

**D9 — Unit 8 (embeddings)**
- **MMTEB** — Kenneth Enevoldsen et al. ICLR 2025. https://arxiv.org/abs/2502.13595 — The multilingual benchmark behind the current MTEB leaderboard. Use it when you pick the embedding model.
- **Qwen3 Embedding** — Qwen Team, Alibaba. Jun 2025. https://arxiv.org/abs/2506.05176 — Open embedding models and matching rerankers (0.6B–8B, Apache 2.0). One family covers D9 and D11.

**D10 — Unit 9 (indexes)**
- No new 2025–2026 item met the quality rule. Keep the original resources (HNSW paper, Qdrant docs, pgvector).

**D11 — Unit 10 (hybrid search & reranking)**
- **On the Theoretical Limitations of Embedding-Based Retrieval** **[2026 venue]** — Weller, Boratko, Naim, Lee. Google DeepMind and Johns Hopkins University. ICLR 2026. https://arxiv.org/abs/2508.21038 — Proves that a single vector cannot return every top-k set. This is a formal reason for hybrid search.
- **Training and Finetuning Sparse Embedding Models with Sentence Transformers** — Tom Aarsen, Arthur Bresnu, Hugging Face. Jul 2025. https://huggingface.co/blog/train-sparse-encoder — The `SparseEncoder` class gives a learned sparse retriever. Compare it with BM25 in the before/after table.

**D12 — Unit 11 (RAG evaluation)**
- **Demystifying Evals for AI Agents** **[2026]** — Anthropic Engineering. Jan 2026. https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents — Defines graders, trials, and harnesses, and separates capability evals from regression evals. Use the terms in the D12 eval handout.

---

# Days 13–16 — Agents

**D13 — Unit 12 (agent anatomy & ReAct)**
- **Agentic AI (course)** — Andrew Ng, DeepLearning.AI. 2025 (page date 30 Sep 2025; updated Aug 2026). https://www.deeplearning.ai/courses/agentic-ai — Teaches reflection, tool use, planning, and multi-agent patterns. Use its structure to check your agent loop diagram.
- **LangGraph v1 release notes** — LangChain. Oct 2025. https://docs.langchain.com/oss/python/releases/langgraph-v1 — `create_react_agent` is deprecated. LangChain `create_agent` replaces it. Write the D13 code against v1.

**D14 — Unit 13 (tools & function calling)**
- **Writing Effective Tools for AI Agents — Using AI Agents** — Anthropic Engineering. Sep 2025. https://www.anthropic.com/engineering/writing-tools-for-agents — Tool names, namespaces, descriptions, and short error messages that help the agent recover. Use it for the retry-with-error-feedback lab.
- **MCP specification, revision 2026-07-28** **[2026]** — Model Context Protocol project. Jul 2026. https://modelcontextprotocol.io/specification/2026-07-28 — The current tool protocol. This revision is stateless and has no `initialize` handshake. If you expose the D14 tools over MCP, write against this version.
- **τ²-Bench** — Victor Barres et al., Sierra. Jun 2025. https://arxiv.org/abs/2506.07982 — A recognized benchmark for tool-calling agents. Use one example task to show how tool use is measured.

**D15 — Unit 14 (memory, state, human-in-the-loop)**
- **Effective Harnesses for Long-Running Agents** — Anthropic Engineering. Nov 2025. https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents — State kept in progress files and git across context windows. A fourth memory option next to buffer, summary, and vector memory.
- **Harness Design for Long-Running Application Development** **[2026]** — Anthropic Engineering. Mar 2026. https://www.anthropic.com/engineering/harness-design-long-running-apps — Compares compaction with full context resets. Use it for the "what does each option lose" question.

**D16 — Unit 15 (multi-agent)**
- **Why Do Multi-Agent LLM Systems Fail? (MAST)** — Cemri, Pan et al., UC Berkeley. NeurIPS 2025. https://arxiv.org/abs/2503.13657 — 14 failure modes from 1,600+ traces. Use it for the failure-mode slide.
- **How We Built Our Multi-Agent Research System** — Anthropic Engineering. Jun 2025. https://www.anthropic.com/engineering/multi-agent-research-system — Multi-agent runs used about 15 times more tokens than chat. Put this number on the cost-per-run slide.
- **Microsoft Agent Framework 1.0** — Microsoft. Status checked 2026-10-10 in the AutoGen README. https://learn.microsoft.com/en-us/agent-framework/overview/ — The successor to AutoGen. Use it instead of AutoGen on the comparison slide.

---

# Days 17–19 — Production

**D17 — Unit 16 (serving & streaming)**
- **MCP specification, revision 2026-07-28 — changelog** **[2026]** — Model Context Protocol project. Jul 2026. https://github.com/modelcontextprotocol/modelcontextprotocol — The Streamable HTTP transport no longer supports SSE stream resumption. A broken stream loses the request, and the client must send it again. Use this as a real example of the SSE trade-offs you teach on D17.

**D18 — Unit 17 (guardrails & prompt injection)**
- **OWASP Top 10 for Agentic Applications for 2026** **[2026 edition]** — OWASP GenAI Security Project. Dec 2025. https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/ — Risks ASI01–ASI10 for agents with tools and memory. Map your live injection demo to one of them.
- **Design Patterns for Securing LLM Agents against Prompt Injections** — Beurer-Kellner et al., ETH Zurich, Google, Microsoft, and others. Jun 2025. https://arxiv.org/abs/2506.08837 — Six patterns that limit agent actions after it reads untrusted input. Use one pattern to block your D18 injection by design.
- **Llama Guard 4** — Meta. Apr 2025. https://huggingface.co/meta-llama/Llama-Guard-4-12B — Current version of the classifier that the plan names. Use it in place of Llama Guard 3.

**D19 — Units 18 + 19 (observability, manager bundle)**
- **OWASP Top 10 for Agentic Applications for 2026** **[2026 edition]** — see D18. Use the ten risk names for the manager risk section.
- **How We Built Our Multi-Agent Research System** — see D16. Use the 15 times token figure in the cost-per-user model.

---

# Day 20 — Assembly kit

- No new item. Use the D3, D12, D16, and D18 items above in the feared-questions list.

---

# Outdated or changed resources

| Original item | Status on 2026-10-10 | Action |
|---|---|---|
| D16: "Skim CrewAI/AutoGen" | The AutoGen README says AutoGen is in maintenance mode. Microsoft Agent Framework 1.0 is the successor. | Skim Microsoft Agent Framework in place of AutoGen. |
| D13–D16: LangGraph build | LangGraph 1.0 shipped in Oct 2025. `create_react_agent` is deprecated. Docs moved to `docs.langchain.com`. | Use LangGraph v1 and the new docs. |
| D18: "Llama Guard" | Llama Guard 4 (Apr 2025) is the current version. | Use Llama Guard 4. |
| D6: "0.5B–1.5B instruct model" | Qwen3 0.6B and 1.7B, and Gemma 3 1B, are now in this class. | Test one of them before D6. |
| D19: Arize Phoenix and LangSmith links (from the resource guide) | Both doc sites moved. | Use https://arize.com/docs/phoenix and https://docs.langchain.com/langsmith |
