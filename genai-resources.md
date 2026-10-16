# Resource Guide — Iterative Depth

Three passes. Do them in order. Pass 2 is what makes the rest efficient.

---

# Pass 1 — The map (90 minutes, one sitting)

Not to learn. To build the mental index everything else hangs on. Read in this order.

| # | Resource | Time | Why this one |
|---|---|---|---|
| 1 | **Anthropic — Building Effective Agents**<br>`anthropic.com/engineering/building-effective-agents` | 20 min | The best short map of the agent space. Its core argument — workflows before agents, simplest thing that works — is also the most valuable thing you can tell an enterprise audience. |
| 2 | **Anthropic — Effective Context Engineering for AI Agents**<br>`anthropic.com/engineering/effective-context-engineering-for-ai-agents` | 20 min | Reframes prompting + RAG + memory as one problem: what tokens are in the window and why. Adopt this framing for your course. |
| 3 | **Karpathy — "Deep Dive into LLMs like ChatGPT"** (YouTube, 3h31m) | 30 min | Watch the chapter markers at 2x, stopping only where you're unsure. Covers pretraining → tokenization → SFT → RLHF → hallucination → tool use. Return to full sections in Pass 3. |
| 4 | **Retrieval-Augmented Generation for LLMs: A Survey** (arXiv 2312.10997) | 20 min | Abstract, figures, and section headers only. Gives you the naive → advanced → modular RAG taxonomy, which is a good spine for Module 2. |

If you only have 60 minutes, do 1, 2, and 4.

---

# Pass 2 — Diagnose (45 minutes)

Answer each question out loud, in plain language, without looking anything up. Score yourself:

- **3** — could answer this from a senior architect in front of a room, including follow-ups
- **2** — know the concept, would fumble the specifics
- **1** — would have to deflect

**Anything scoring 1–2 on a unit targeted at L3 is your priority queue.** Everything else can wait.

| Unit | Diagnostic question |
|---|---|
| 1. Tokenization | Why does the same sentence cost 2–3× more tokens in Hindi than English, and what does that do to a customer's context budget? |
| 2. Inference control | At temperature 0, does the same prompt give the same output every time? Why or why not? |
| 3. Attention | Why is attention O(n²)? What exactly is growing quadratically? |
| 4. Long context | Retrieval returns 20 documents and accuracy drops compared to returning 5. Explain. |
| 5. Fine-tuning | A team fine-tuned a model and it performs worse than the base model. Name three likely causes in order of probability. |
| 6. SLMs | When is a 3B on-prem model the right answer over a frontier API? Give two conditions. |
| 7. Prompting | How do you *guarantee* valid JSON output — not "usually valid"? |
| 8. Embeddings | Why does a cross-encoder beat a bi-encoder on accuracy, and why can't you use one for search? |
| 9. Indexes | Your HNSW index gives 95% recall. Where did the other 5% go, and what do you trade to get it back? |
| 10. Hybrid search | A user searches for an exact error code and gets nothing relevant. Diagnose it. |
| 11. Evaluation | The answer is wrong. How do you determine whether it's a retrieval failure or a generation failure? |
| 12. Agents | When is a fixed workflow strictly better than an agent? |
| 13. Tools | The model calls a tool that doesn't exist. What does your code do next? |
| 14. Memory | The conversation exceeds the context window. Three options — what does each one lose? |
| 15. Multi-agent | Two agents under a supervisor. Where exactly does token cost blow up? |
| 16. Serving | Why SSE rather than WebSockets for token streaming? |
| 17. Guardrails | A user embeds instructions inside an uploaded document. Which of your defenses fires, and which won't? |
| 18. Observability | TTFT is 4 seconds and users complain, but TPS looks fine. What do you investigate? |
| 19. Manager bundle | What does this cost per user per month, and which three variables drive that number? |

Write your 19 scores down. Re-score at Day 10 and Day 20 — it's the only honest measure of whether the prep is working.

---

# Pass 3 — Depth, by unit

Two to four resources each. Ordered: **read first** → **go deeper** → **hands-on**. Skip anything you scored 3 on.

## Units 1–4 · Mechanics

**Tokenization & context economics**
- OpenAI Tokenizer / `tiktoken` — tokenize the same text in three languages, look at the counts
- Karpathy, "Let's build the GPT Tokenizer" (YouTube, ~2h) — the definitive BPE explanation; watch if you scored 1
- Hugging Face tokenizers docs — special tokens and chat templates. Don't skip chat templates; they matter again in Unit 5

**Inference control**
- Provider docs on sampling parameters (Anthropic or OpenAI) — short, precise
- "The Curious Case of Neural Text Degeneration" (arXiv 1904.09751) — where nucleus/top-p sampling comes from and why greedy decoding fails

**Attention** *(target L1 — read one, stop)*
- Jay Alammar, "The Illustrated Transformer" — still the best diagram-first explanation
- "Attention Is All You Need" (arXiv 1706.03762) — read for literacy, not depth
- 3Blue1Brown's transformer/attention videos — best visual intuition if the matrices aren't landing

**Long context**
- **"Lost in the Middle" (arXiv 2307.03172)** — the position-accuracy finding your Day 3 chart replicates. Read this properly
- RoFormer / RoPE (arXiv 2104.09864) — skim the method section
- FlashAttention (arXiv 2205.14135) — abstract and figure 1 only; it's IO-aware, same math
- Recent "context rot" work on long-horizon degradation — search current papers, this is moving fast

## Unit 5 · Fine-tuning *(your L3 block)*

- **Unsloth fine-tuning guide** — `unsloth.ai/docs/get-started/fine-tuning-llms-guide`. Start here. Covers the LoRA vs QLoRA vs full FT decision plainly
- **Unsloth notebooks** — `unsloth.ai/docs/get-started/unsloth-notebooks`. Pre-built Colab notebooks; fork one rather than writing from scratch
- **Unsloth Colab setup** — `unsloth.ai/docs/get-started/install/google-colab`. Read before your Day 6 session
- LoRA (arXiv 2106.09685) — read sections 1–4 for the rank decomposition
- QLoRA (arXiv 2305.14314) — NF4, double quantization, paged optimizers
- Hugging Face PEFT docs — `huggingface.co/docs/peft`. Reference for `r`, `alpha`, target modules

**Model choice for the lab:** a Qwen 2.5 3B instruct model with 4-bit QLoRA is the proven free-T4 path — the quantized base lands around 1.7 GB. Avoid the newest model generation for a classroom lab; Unsloth currently warns against 4-bit QLoRA on Qwen 3.5 due to larger quantization error. Verify current guidance before Day 6, but bias toward the boring option.

**Then read specifically about chat template mismatch** — training format vs inference format. It's the most common reason a fine-tune appears to do nothing, and it's the question a developer audience will hit within an hour of starting.

## Units 8–11 · RAG

**Embeddings & chunking**
- Sentence-Transformers docs (`sbert.net`) — bi-encoders vs cross-encoders, explained by the people who built them
- MTEB leaderboard on Hugging Face — for choosing an embedding model, and for showing the audience that "best" is task-dependent
- LlamaIndex or LangChain docs on parent-document / hierarchical retrieval

**Indexes**
- **HNSW paper (arXiv 1603.09320)** — read the intro and the algorithm description; the graph intuition is teachable
- Qdrant documentation on indexing and filtering — the clearest vendor explanation of pre- vs post-filtering
- `pgvector` README — if your audience is enterprise, "it's just Postgres" is often the winning answer

**Hybrid & reranking**
- Anything current on reciprocal rank fusion — short, and the mechanism is simple enough to show on a slide
- Cohere Rerank docs, or BGE reranker model cards on Hugging Face
- Original RAG paper (arXiv 2005.11401) — historical context, useful for the "how did we get here" opening

**Evaluation**
- **Ragas docs** (`docs.ragas.io`) — faithfulness, answer relevancy, context precision, context recall. This is your Day 12 lab
- RAGAS paper (arXiv 2309.15217) — for defending the metrics when someone asks how they work
- DeepEval — pytest-style, if your developer audience wants evals in CI. Worth a mention slide
- Be ready for the honest caveat: recent work comparing Ragas, DeepEval, RAGChecker and Opik against human judgments finds the correlations are imperfect. Teaching that limitation earns more credibility than presenting the scores as truth

## Units 12–15 · Agents

- **Anthropic, Building Effective Agents** — reread properly now. The workflow patterns (chaining, routing, orchestrator-workers, evaluator-optimizer) are your teaching taxonomy
- **Anthropic, Effective Context Engineering** — reread for Unit 14. Just-in-time retrieval, compaction, message pruning
- **ReAct (arXiv 2210.03629)** — the reason→act→observe loop, in the original
- Chain-of-Thought (arXiv 2201.11903) — for Unit 7 as well
- **LangGraph docs** (`langchain-ai.github.io/langgraph`) — state graphs, conditional edges, checkpointing, human-in-the-loop. Your Days 13–16 build
- Anthropic's writing on tool design for agents — tool descriptions are prompts; this is the single most actionable thing for developers
- CrewAI and AutoGen docs — **skim only**, enough for one comparison slide

## Units 16–18 · Production

- FastAPI docs on async and streaming responses (`fastapi.tiangolo.com`)
- MDN on Server-Sent Events — for the SSE vs WebSocket answer
- **Instructor** (`python.useinstructor.com`) and **Outlines** — retry-based vs constrained-decoding approaches to structured output. Teach the difference; it's a real architectural choice
- OWASP Top 10 for LLM Applications — the framing your sysadmins and architects already recognize. Strong choice for the guardrails session
- NeMo Guardrails and Llama Guard docs / model cards
- **Arize Phoenix** (`docs.arize.com/phoenix`) — open source, self-hostable, matters for enterprise audiences
- **LangSmith** (`docs.smith.langchain.com`) — easier if you're already in LangGraph, but note the framework coupling when someone asks
- OpenTelemetry GenAI semantic conventions — the vendor-neutral answer, and the one architects will want

## Unit 19 · Manager bundle

No single source. Build from:
- Provider pricing pages — build a real cost-per-user model with your own numbers
- Your own Day 1 token math and Day 16 cost-per-run logs — far more persuasive than a vendor slide
- OWASP LLM Top 10 — for the risk section
- The workflows-before-agents argument from Building Effective Agents — reframed as a cost and reliability argument

---

# Practical notes

**Don't read papers you scored 3 on.** The papers are for gaps, not for completeness. You have 40 hours.

**Prefer official docs over blog posts** for anything you'll teach hands-on. Tutorial blog posts go stale within months in this space and you'll be debugging their version drift in front of a room.

**Verify before Day 6 and before delivery.** Model availability, library versions, and free-tier GPU allocations all change on a monthly cadence. Re-check the Unsloth notebooks and your chosen base model close to the date, not in week one.

**Keep a link file per unit** as you go. It becomes the "further reading" handout, which participants in a 5–7 day course always ask for and almost nobody prepares.
