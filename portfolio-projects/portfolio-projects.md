# Portfolio Projects — Real-World AI Applications

This document describes eight projects that show your AI engineering skills.
The projects start with fine-tuning small models. Then they move to RAG pipelines, agentic applications, and production systems.

It does not contain code or a tutorial. Each project gives the problem, the scope, the build phases, and the success criteria. You write the code.

---

## How to use this document

- Do the projects in order. Each project uses the output of an earlier project.
- Use one domain for all projects. This makes the portfolio a single story, not eight unrelated demos.
- Recommended domain: **application security and vulnerability management**. It has public data (CVE records, CWE descriptions, security advisories, OWASP documents), exact identifiers that test retrieval, and clear high-risk actions that need human approval. You can replace it with another domain. The instructions stay the same.
- Each project must produce the portfolio deliverables listed at the end of this document.

## Progression map

| # | Project | Main skill | Study plan units | Effort |
|---|---|---|---|---|
| 1 | Security finding normalizer | QLoRA fine-tuning, structured output | 5, 7 | 1–2 weeks |
| 2 | Distilled severity classifier on the edge | Distillation, quantization, local serving | 5, 6 | 1–2 weeks |
| 3 | Advisory search with hybrid RAG | Chunking, hybrid search, reranking | 8, 9, 10 | 2 weeks |
| 4 | RAG evaluation and regression harness | Evaluation, CI quality gates | 11 | 1 week |
| 5 | Multi-tenant knowledge assistant | Access control, metadata filtering, guardrails | 9, 17 | 2 weeks |
| 6 | Vulnerability triage agent | Tools, error recovery, human approval | 12, 13, 14 | 2–3 weeks |
| 7 | Research and report multi-agent system | Orchestration, evaluator-optimizer pattern | 15 | 2 weeks |
| 8 | Capstone: production AI security analyst | Serving, streaming, observability, cost | 16, 17, 18, 19 | 3 weeks |

---

# Stage 1 — Fine-tuning small models

## Project 1 — Security finding normalizer

**Problem:** Security scanners produce output in many different formats. Analysts spend time converting the output into one standard report format. A general model does this with an inconsistent structure.

**What you build:** A fine-tuned small language model (1B–3B) that converts raw scanner output into a fixed JSON finding schema with a consistent writing style.

**Skills shown:**
- Dataset design and cleaning.
- QLoRA fine-tuning with a correct chat template.
- Evaluation of a fine-tuned model against its base model and a frontier model.
- The decision of when fine-tuning is the correct tool.

**Instructions:**
1. Define the target schema. Include: title, CWE ID, severity, affected component, evidence, and remediation. Make severity an enumeration.
2. Collect raw input examples. Use public scanner output formats (for example, SAST, DAST, and dependency scanner outputs) or write realistic synthetic examples.
3. Create 300–800 input-output pairs. You can use a frontier model to create first drafts. Review and correct every example by hand. Record how many examples you corrected.
4. Split the data into train, validation, and test sets. Make sure no near-duplicates cross the split.
5. Measure the base model on the test set before training. This is your baseline.
6. Fine-tune with QLoRA. Record the base model, rank, alpha, learning rate, epochs, and GPU type.
7. Measure the fine-tuned model on the same test set.
8. Measure a frontier model with a good prompt on the same test set.
9. Do at least two experiments that change one parameter (for example, rank 8 against rank 32, or 200 examples against 800 examples).

**Success criteria:**
- Schema-valid output rate on the test set is higher than the base model.
- Field-level accuracy (for example, correct CWE ID and correct severity) is reported for each field.
- A comparison table shows base, fine-tuned, and frontier results with cost per 1,000 requests.

**Stretch goals:**
- Compare LoRA with full fine-tuning on the smallest model.
- Add constrained decoding at inference. Measure whether fine-tuning is still necessary with it.

**Pitfalls to document:** Chat template mismatch, overfitting on a small dataset, and data leakage between train and test sets.

---

## Project 2 — Distilled severity classifier on the edge

**Problem:** An organization cannot send vulnerability data to an external API. It needs a fast, local model for severity classification that runs on a CPU.

**What you build:** A small model trained on labels from a large "teacher" model, then quantized and served locally.

**Skills shown:**
- Knowledge distillation from a large model to a small model.
- Quantization and its effect on quality.
- Local serving and the on-prem decision.

**Instructions:**
1. Select a public set of vulnerability descriptions (for example, CVE descriptions) with known severity scores.
2. Use a frontier model as the teacher. Label a subset with severity and a one-sentence reason.
3. Measure the teacher's accuracy against the known scores. Remove or correct the wrong teacher labels.
4. Fine-tune a small model (0.5B–1.5B) on the teacher labels.
5. Export the model to a local format (for example, GGUF) at two or three quantization levels.
6. Serve the model with a local runtime (for example, Ollama or llama.cpp).
7. Measure accuracy, latency, memory use, and tokens per second at each quantization level on a CPU.
8. Compare with a classical baseline (for example, a TF-IDF plus logistic regression classifier). Be honest if the classical baseline is close.

**Success criteria:**
- A table: model, quantization level, accuracy, latency, memory, file size.
- The model runs without network access.
- A written recommendation for the on-prem decision, with the conditions where it is correct.

**Stretch goals:**
- Run the model on a Raspberry Pi or a phone.
- Add a confidence threshold. Send low-confidence cases to a human or a large model (a model cascade). Measure cost savings.

---

# Stage 2 — RAG pipelines

## Project 3 — Advisory search with hybrid RAG

**Problem:** Analysts must find the correct advisory, CWE entry, or internal runbook fast. Searches often contain exact identifiers such as CVE IDs, package names, and version numbers. Pure vector search misses them.

**What you build:** A question-answering system over security documents with hybrid search, reranking, and citations.

**Skills shown:**
- Document parsing and chunking strategy design.
- Embedding model selection.
- Hybrid search (dense plus BM25) with rank fusion.
- Cross-encoder reranking.
- Answers with source citations.

**Instructions:**
1. Build a corpus of 1,000 or more documents: CVE records, CWE descriptions, OWASP guidance, and some vendor advisories.
2. Parse each source type correctly. Keep tables, code blocks, and headings.
3. Store metadata for each chunk: source, document type, publication date, affected product, and CVE IDs.
4. Implement at least two chunking strategies. Measure hit rate for each.
5. Implement dense search, BM25 search, and hybrid search with reciprocal rank fusion.
6. Add a cross-encoder reranker on the top 50 results.
7. Generate answers that cite the source chunk for each claim.
8. Refuse to answer when retrieval finds no relevant context.
9. Create a question set of 50 or more questions. Include exact-ID questions, conceptual questions, and questions with no answer in the corpus.
10. Record retrieval metrics (hit rate, MRR) for each configuration.

**Success criteria:**
- A before/after table: dense only, BM25 only, hybrid, hybrid plus rerank.
- Exact-ID questions succeed with hybrid search where dense search fails.
- Every answer has at least one valid citation.
- The system refuses questions that the corpus cannot answer.

**Stretch goals:**
- Add incremental ingestion: new advisories are indexed without a full rebuild.
- Add query rewriting. Measure whether it helps.
- Add parent-child retrieval.

---

## Project 4 — RAG evaluation and regression harness

**Problem:** Teams change prompts, models, and chunking without knowing if quality becomes worse. They need a repeatable quality gate.

**What you build:** An evaluation harness that runs on each change to the Project 3 system and blocks the change if quality decreases.

**Skills shown:**
- Retrieval and generation metrics.
- LLM-as-judge design and its limits.
- Evaluation in continuous integration.
- Diagnosis of failures to the correct component.

**Instructions:**
1. Expand the Project 3 question set to 100 questions. Add reference answers and reference source IDs.
2. Measure context precision, context recall, faithfulness, and answer relevancy.
3. Label 30 answers by hand. Compare your labels with the judge model scores. Report the agreement rate.
4. Make a failure taxonomy: retrieval miss, wrong chunk ranked first, unsupported claim, incomplete answer, correct refusal, and wrong refusal.
5. Classify every failure into the taxonomy.
6. Run the harness in CI on every pull request. Set thresholds for each metric.
7. Make three deliberate bad changes (for example, a smaller chunk size, a weaker model, and a prompt that allows outside knowledge). Show that the harness detects each one.
8. Produce an HTML or Markdown report for each run with the metric changes against the last good run.

**Success criteria:**
- CI fails on each of the three deliberate bad changes.
- Judge-to-human agreement is measured and reported.
- Each failure type has a documented fix.

**Stretch goals:**
- Generate synthetic test questions from the corpus. Measure their quality against your hand-written questions.
- Track evaluation cost per run and reduce it with sampling.

---

## Project 5 — Multi-tenant knowledge assistant

**Problem:** Several teams or customers share one assistant. Each user must see only the documents they are permitted to see. The system must also resist prompt injection from uploaded documents.

**What you build:** A RAG assistant with document-level access control, tenant isolation, and layered guardrails.

**Skills shown:**
- Metadata filtering and pre-filtering for access control.
- Tenant isolation design.
- Indirect prompt injection defense.
- PII detection and redaction.

**Instructions:**
1. Create three tenants with separate document sets and some shared documents.
2. Create users with roles. Assign access levels to each document.
3. Apply the access filter inside the vector database query (pre-filtering). Do not filter after retrieval.
4. Write tests that try to retrieve another tenant's documents. All tests must fail to retrieve.
5. Add document upload. Scan uploaded documents for embedded instructions before indexing.
6. Mark retrieved content as data, not instructions, in the prompt structure.
7. Add an input classifier and an output check (PII, system prompt leakage).
8. Build an attack test set of 30 or more attacks, mapped to the OWASP Top 10 for LLM Applications.
9. Measure the attack success rate and the false positive rate on normal questions.
10. Log every access decision for audit.

**Success criteria:**
- Zero cross-tenant retrievals in the test set.
- An attack-by-defense-layer table.
- A false positive rate that is reported, not hidden.
- An audit log that shows who asked what and which documents were used.

**Stretch goals:**
- Add per-tenant encryption keys.
- Add data residency: store and process each tenant's data in a specified region.

---

# Stage 3 — Agentic AI applications

## Project 6 — Vulnerability triage agent

**Problem:** When a new vulnerability is reported, an analyst checks which assets are affected, looks up the details, decides priority, and creates a ticket. This takes hours for each report.

**What you build:** An agent that does the triage steps with tools, recovers from tool errors, and asks a human for approval before any change to external systems.

**Skills shown:**
- Agent loop design (ReAct) with a state graph.
- Tool schema design.
- Error recovery and retry limits.
- Memory and checkpointing.
- Human-in-the-loop approval gates.

**Tools to give the agent:**
- Search the advisory knowledge base (Project 3 as a tool).
- Look up a CVE in a public vulnerability database API.
- Query an asset inventory (a mock database of hosts, software, and versions).
- Classify severity (Project 2 as a tool).
- Create a ticket (a mock ticket system). This tool needs approval.
- Send a notification (a mock). This tool needs approval.

**Instructions:**
1. Draw the state graph before you write code. Define the states, the edges, and the stop conditions.
2. Build a fixed workflow version first. Measure it.
3. Build the agent version. Measure it on the same inputs.
4. Write tool descriptions carefully. Test two versions of each description. Record the effect on tool choice.
5. Handle these errors: invalid arguments, timeouts, empty results, unknown tool names, and API rate limits. Return each error to the model as a clear message.
6. Set limits: maximum steps, maximum tool calls, maximum tokens, and maximum cost per run.
7. Add checkpointing. Stop the process during an approval wait. Restart it. Confirm that it continues from the same state.
8. Add approval gates for the ticket and notification tools. Show approve, reject, and edit paths.
9. Build a test set of 30 vulnerability reports with expected outcomes.
10. Measure task success rate, average steps, average cost, and the rate of unnecessary tool calls.

**Success criteria:**
- A workflow-against-agent comparison with cost and success rate.
- Each error type has a test that proves recovery.
- No external action happens without approval.
- A restart from a checkpoint works.

**Stretch goals:**
- Expose the tools through an MCP server so that other agents and clients can use them.
- Add long-term memory: the agent remembers decisions on similar earlier vulnerabilities.

---

## Project 7 — Research and report multi-agent system

**Problem:** Security teams write threat reports that combine many sources. One agent with a long context produces weak, unfocused reports.

**What you build:** A small multi-agent system (two or three agents) that researches a topic, writes a report, and reviews it against a quality checklist.

**Skills shown:**
- Orchestrator-worker and evaluator-optimizer patterns.
- Context isolation between agents.
- Cost control across agent hops.
- Honest comparison with simpler designs.

**Instructions:**
1. Define the report format and a review checklist (for example: every claim has a source, severity is justified, remediation steps are specific).
2. Build a single-agent baseline. Measure quality, cost, and latency.
3. Build a researcher agent that collects facts with citations.
4. Build a writer agent that uses only the researcher's output.
5. Build a reviewer agent that scores the report against the checklist and returns specific corrections.
6. Limit the review cycles (for example, a maximum of two).
7. Pass only the necessary context to each agent. Measure the token savings against passing the full history.
8. Put a wrong fact into a source. Record whether the reviewer finds it.
9. Evaluate 20 reports from each design with a rubric. Use a judge model and your own review on a subset.

**Success criteria:**
- A table: single agent against multi-agent, with quality score, tokens, cost, and latency.
- A clear written conclusion about when the multi-agent design is worth the cost.
- An error propagation test with a documented result.

**Stretch goals:**
- Run the researcher agents in parallel on subtopics.
- Compare two frameworks (for example, LangGraph and CrewAI) on the same task.

---

# Stage 4 — Production

## Project 8 — Capstone: production AI security analyst

**Problem:** A prototype is not a product. The system must be fast for the user, safe, observable, and affordable.

**What you build:** A deployed application that combines Projects 3, 5, and 6 behind one interface. It streams its work, applies guardrails, records traces, and reports cost.

**Skills shown:**
- Async API serving with streaming.
- A user interface that shows agent steps.
- Layered guardrails.
- Tracing, latency metrics, and cost attribution.
- Deployment and a cost model for decision-makers.

**Instructions:**
1. Build an async API. Stream tokens and intermediate steps with Server-Sent Events.
2. Build a web UI. Show the current agent step, the tool calls, the sources, and the approval requests.
3. Support request cancellation from the UI.
4. Add authentication. Pass the user identity to the access control from Project 5.
5. Instrument every step with OpenTelemetry-compatible traces. Send them to a self-hosted tracing tool.
6. Record time to first token, tokens per second, total latency, and cost for each request.
7. Attribute cost to each user and each feature.
8. Add rate limits and a per-user daily cost limit.
9. Add model fallback: if the main provider fails or is slow, use a second provider or a local model.
10. Add response caching for repeated questions. Measure the cache hit rate and the cost savings.
11. Run a load test at 10, 50, and 100 concurrent users. Record latency percentiles (p50, p95).
12. Deploy with containers. Write a one-command local start and a cloud deployment guide.
13. Write a cost-per-user-per-month model with your measured numbers.
14. Write a one-page risk and governance summary for managers.

**Success criteria:**
- A public demo video of a full triage run with approval.
- A trace waterfall for a real request.
- A load test report with p50 and p95 latency.
- A cost model built from measured data, not estimates.
- An attack test report from Project 5 that runs against the deployed system.

**Stretch goals:**
- Replace one model call with the fine-tuned model from Project 1. Measure the cost and quality change.
- Add online evaluation: sample live traffic and score it every day.
- Add a feedback button. Use the feedback to add cases to the Project 4 test set.

---

# Portfolio deliverables for each project

Each project must have these items. Without them, a reviewer cannot judge the work.

1. **README** with: the problem, the architecture diagram, the setup steps, and the results table.
2. **Results table** with measured numbers. Compare against a baseline every time.
3. **Decision log** that records each important choice, the options you considered, and why you chose one.
4. **Failure analysis** that shows what did not work and why. This section shows real engineering experience.
5. **Reproducibility**: pinned versions, a data preparation script, and a one-command run.
6. **Demo video** of 2–5 minutes.
7. **Short article** (blog or LinkedIn) that explains one finding from the project with data.

# Presentation advice

- Lead with the measured result, not the technology list.
- Always show the baseline. "Hybrid search found 9 of 10 exact-ID questions; dense search found 4" is stronger than "I built hybrid search".
- Show cost and latency next to quality. This shows production thinking.
- Show one failure you found and fixed in each project.
- Keep the domain consistent. A reviewer then sees one system that grows, not eight unrelated exercises.
