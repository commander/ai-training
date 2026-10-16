# Teaching Readiness Plan — Modular GenAI Training
**2 hrs/day × 20 days = 40 hours. No date or audience committed.**

---

## The model: units, not a course

You're not preparing one course. You're preparing a **menu of teaching units** you assemble once you know who's in the room. Each unit is prepped to a target depth; you can always teach below your depth, never above it.

**Depth levels:**

| | Meaning | What it requires |
|---|---|---|
| **L1** | Can talk about it credibly, answer basic questions | Understanding + a slide |
| **L2** | Can explain architecture, tradeoffs, cost; can run a demo | Above + working demo + failure knowledge |
| **L3** | Can lead a hands-on lab and debug 20 people's broken environments | Above + tested environment + fallbacks + top-10 failure modes |

**Prep effort is roughly 1 : 3 : 8 across L1 : L2 : L3.** This is why you can't be L3 on everything. The rule for deciding: **prep depth inversely to improvisability.** Anything you could wing with your existing knowledge gets L1. Anything that fails live without preparation gets L3.

---

## Readiness matrix

| # | Unit | Target | Why that level |
|---|---|---|---|
| 1 | Tokenization & context economics | **L2** | Best cost explainer for any audience. Cheap demo. |
| 2 | Inference control (temp, top-p, penalties) | **L2** | Live demo is trivial and lands well. |
| 3 | Attention & transformer internals | **L1** | Your scope boundary. Don't chase depth here. |
| 4 | Long context & failure modes | **L2** | Answers "why not just use 1M context" all week. |
| 5 | **Fine-tuning: decision + QLoRA hands-on** | **L3** | Highest prep cost, zero improvisability. Your explicit goal. |
| 6 | SLMs, edge & on-prem | **L1** | Discussion topic, no lab. |
| 7 | Prompting as a software primitive | **L2** | You can already do this. Add structured-output rigour only. |
| 8 | Chunking & embeddings | **L3** | Lab foundation. You know it — formalize it. |
| 9 | Vector indexes & filtering | **L2** | Architects want tradeoffs, not implementation. |
| 10 | Hybrid search & reranking | **L3** | Biggest visible quality jump. Strong lab. |
| 11 | RAG evaluation | **L3** | The unit managers care most about. Also the one most people skip. |
| 12 | Agent anatomy & ReAct | **L3** | Core of Module 3. |
| 13 | Tools & function calling | **L3** | Error recovery lab is the most memorable exercise. |
| 14 | Memory, state, human-in-the-loop | **L2** | Governance story for managers and sysadmins. |
| 15 | Multi-agent orchestration | **L2** | Overrated in practice. Teach the failure modes. |
| 16 | Serving & streaming | **L3** | Your centerpiece demo needs a UI. |
| 17 | Guardrails & prompt injection | **L3** | Best single live moment. Security-adjacent audiences love it. |
| 18 | Observability & cost | **L2** | Trace waterfall makes agent complexity visible to non-coders. |
| 19 | Manager bundle: build-vs-buy, cost, risk, governance | **L2** | Not a subset of the technical work. Prep separately. |

Eight units at L3, nine at L2, two at L1. That fits 40 hours. Adding a tenth L3 means dropping one.

---

## Assembly menu — once you know the audience

| Audience | Days | Assemble from |
|---|---|---|
| **All developers, want to build** | 5–7 | Units 1,2,4 (fast) → 5 full hands-on → 8,10,11 → 12,13,14 → 16,17,18. Fine-tuning gets a full day. |
| **Mixed room** | 5 | Units 1,2,4,7 → 8,9,10,11 → 12,13,14,15 → 16,17,18 at L2. Unit 5 becomes decision-framework + recorded run. Split afternoons: builders lab, others observe. |
| **Mostly managers / decision-makers** | 2–3 | Units 1,4,7 → 11 (evaluation) → 14 (human-in-loop) → 17 (risk) → 19. Demos only, no labs. You can largely improvise this. |
| **Architects & senior technical** | 3–4 | Units 4,9,10,11 → 12,15 → 16,17,18. Tradeoffs and failure modes throughout. Skip Unit 5 entirely. |

Build these four agendas as one-page documents on Day 20. When the audience is confirmed, you pick one instead of designing from scratch under time pressure.

---

## Daily block

| Time | Activity |
|---|---|
| 0:00–0:10 | Review yesterday's asset cold |
| 0:10–1:00 | Learn |
| 1:00–1:35 | Build (repo commit or environment work) |
| 1:35–2:00 | Produce the teaching asset |

The asset slot is what converts 40 hours of study into a teachable course. Protect it above everything else.

---

# Days 1–4 — Foundations (L1/L2)

**D1 — Unit 1.** Q/K/V at concept level only. BPE, token counts, why non-English costs more. KV cache: compute the size for a 7B model at 8k context by hand.
*Asset:* tokenizer notebook + cost-per-token slide.

**D2 — Unit 2.** Temperature, top-p, top-k, penalties as logit operations. Determinism and why temp 0 still isn't reproducible across providers.
*Asset:* live parameter demo, same prompt across settings.

**D3 — Units 3 + 4.** Attention to L1 and stop — write down your scope boundary sentence for when someone goes deeper. RoPE, context extension, lost-in-the-middle. Run a needle-in-a-haystack test at 10/50/90% depth.
*Asset:* accuracy-by-depth chart. Highest-value single slide you'll make.

**D4 — Unit 7.** Structured output: JSON schema, Pydantic, constrained decoding vs retry loops. System/user boundaries and why they're weak protection.
*Asset:* structured-output lab handout (short — you already know this material).

---

# Days 5–8 — Fine-tuning block (L3)

The one block that cannot be compressed. Four sessions, treated as infrastructure work.

**D5 — Theory & decision.** Low-rank decomposition, what `r` and `alpha` control, NF4 quantization and where quality is lost. The decision rule: behavior/format/tone → fine-tune; facts → retrieve; both → do RAG first.
*Asset:* decision tree (manager version) + parameter math (developer version).

**D6 — Environment.** Pick a base model small enough to train in 15–25 minutes on a free-tier T4 — a 0.5B–1.5B instruct model is the right class; verify current availability before committing. Prepare a ~200-example instruction dataset in your domain. Get one run started.
*Asset:* Colab notebook, cell-by-cell.

**D7 — Full run & failure modes.** Complete a run. Watch the loss curve. Then deliberately break it and learn each signature: OOM, loss flat, loss to zero (overfit), and **chat-template mismatch between training and inference** — the single most common reason a fine-tune "does nothing."
*Asset:* troubleshooting one-pager. This is what makes you L3 rather than L2.

**D8 — Package the lab + Unit 6.** Save a finished adapter so participants who fail can load a working result and continue. Time the whole lab end to end. Add SLM/edge discussion material.
*Asset:* complete fine-tuning lab with fallback path.

---

# Days 9–12 — RAG (L3)

Repo starts here: one application, each stage a tagged branch.

**D9 — Unit 8.** Bi- vs cross-encoders, asymmetric search, parent-child retrieval. Ingest your corpus. Tag `01-baseline`.
**D10 — Unit 9.** HNSW vs IVF, recall/latency/memory, pre- vs post-filtering. Move to pgvector or Qdrant with real metadata filters.
**D11 — Unit 10.** Why dense vectors miss IDs and error codes; BM25; reciprocal rank fusion; cross-encoder rerank top-50 → top-5. Record before/after on 10 queries. Tag `02-retrieval`.
**D12 — Unit 11.** Groundedness / answer relevance / context relevance and the different fix each implies. Ragas harness, 20-question set. Tag `03-eval`.

*Assets:* two lab handouts, the before/after rerank chart, the eval results.

---

# Days 13–16 — Agents

**D13 — Unit 12 (L3).** ReAct loop, planning, structured output as the code interface. RAG becomes an agent tool. Tag `04-agent`.
**D14 — Unit 13 (L3).** Tool schemas, argument extraction, malformed JSON, timeouts, hallucinated tool names, retry-with-error-feedback. Tag `05-tools`.
**D15 — Unit 14 (L2).** Rolling buffer vs summarization vs vector memory. Checkpointing and human approval gates. Tag `06-graph`.
**D16 — Unit 15 (L2).** Supervisor vs sequential. Loop limits, compounding token cost per hop, error propagation. Two agents maximum. Skim CrewAI/AutoGen for the comparison slide.

*Assets:* agent loop diagram, "break it on purpose" lab, human-approval demo, cost-per-run slide.

---

# Days 17–19 — Production

**D17 — Unit 16 (L3).** FastAPI async, SSE vs WebSockets, streaming intermediate reasoning steps. Minimal UI. Tag `07-serve`.
**D18 — Unit 17 (L3).** Input vs output filtering, Llama Guard / NeMo, constrained decoding. Keep one injection that gets blocked live. Tag `08-guard`.
**D19 — Units 18 + 19.** Tracing spans, TTFT vs TPS, cost attribution. Then build the manager bundle: cost per user model, build-vs-buy, data residency and governance, what to ask vendors. Most of it derives from work you've already done. Tag `09-observe`.

---

# Day 20 — Assembly kit

Not rehearsal — you can't rehearse for an unknown audience. Instead, produce:

1. **Four one-page agendas** (the assembly menu above), each mapped to specific units and timings.
2. **`SETUP.md`** for participants, plus a hosted fallback for locked-down laptops.
3. **Clean-clone test** of every repo tag on a machine that isn't yours.
4. **Scope boundary statement** — one sentence for when someone asks something past your depth. "That's deeper into model architecture than we're going today, but here's what matters for your decision..." Prepare it; don't invent it live.
5. **Feared-questions list** with answers: why not bigger context (D3), how do we know it isn't hallucinating (D12), cost per user (D1 + D16), should we fine-tune (D5), is our data safe (D18).

---

## Once the date and audience are confirmed

Budget **three more sessions** you have not yet spent:

| Session | Work |
|---|---|
| T-3 | Pick the agenda, cut units to fit the actual hours, send `SETUP.md` |
| T-2 | Rehearse Day 1 of delivery and the capstone demo with a timer |
| T-1 | Environment check, record a backup screen capture of a full successful run |

## If your daily time shrinks

Protect in this order: **fine-tuning block (D5–8) > asset slot > repo commits > learning depth.** The fine-tuning block is the only thing on this list that can't be compressed or improvised. If you lose days, lose them from Units 15 and 18, not from D5–8.
