# Module Demos — Descriptions and Instructions

This document gives one demo specification for each of the 19 units in `20-day-genai-study-plan.md`.
It does not contain code. You write the code.

Each demo has these parts:

- **Goal** — the one point that the audience must remember.
- **Setup** — what you must prepare before the session.
- **Steps** — the sequence to do live.
- **Show the failure** — a controlled failure to show the limit of the method.
- **Expected result** — what the audience must see.
- **Asset** — what you keep for the course.
- **Time** — approximate duration in the room.

---

## General rules for all demos

1. Make each demo run from one command or one notebook cell sequence.
2. Pin all library versions. Record the versions in the demo README.
3. Record a screen capture of each successful run. Use it if the live run fails.
4. Keep the input data in the repository. Do not download data live.
5. Cache API responses where possible. Use the cache if the network fails.
6. Show numbers, not opinions. Each demo must produce a table, a chart, or a trace.
7. For L3 units, prepare a "break it" step. The failure teaches more than the success.

---

# Module 1 — Foundations (Units 1–7)

## Unit 1 — Tokenization and context economics (L2)

**Goal:** Tokens are the unit of cost, latency, and context limit. Language and format change the token count.

**Setup:**
- Prepare one paragraph of the same text in English, Hindi, and one more language (for example, Tamil or Japanese).
- Prepare the same data in three formats: JSON, Markdown table, and plain CSV.
- Get two or three tokenizers (for example, one OpenAI tokenizer and one open-model tokenizer from Hugging Face).
- Get the current price per million input and output tokens for two providers.

**Steps:**
1. Tokenize the English paragraph. Show the token boundaries with colors.
2. Tokenize the Hindi and third-language versions. Show the token count ratio against English.
3. Tokenize the three data formats. Show which format uses the fewest tokens.
4. Tokenize numbers, dates, and code identifiers. Show how the tokenizer splits them.
5. Calculate the cost of one request and of 10,000 requests per day for each language.
6. Calculate the KV cache size for a 7B model at 8k context. Do the calculation on a slide, step by step. Use layers × 2 (K and V) × heads × head dimension × sequence length × bytes per value.

**Show the failure:** Show a prompt that fits the context window in English but does not fit in Hindi.

**Expected result:** A table with columns: language or format, token count, ratio to English, cost per 10,000 requests.

**Asset:** Tokenizer notebook. One cost-per-token slide. One KV cache calculation slide.

**Time:** 20 minutes.

---

## Unit 2 — Inference control (L2)

**Goal:** Sampling parameters change the logit distribution. Temperature 0 does not guarantee identical output.

**Setup:**
- Use one open model that gives access to logits (a small local model is sufficient).
- Use one hosted API model for the reproducibility test.
- Prepare two prompts: one creative ("write a product tagline") and one factual ("list the planets").

**Steps:**
1. Show the top 10 next-token probabilities for one prompt as a bar chart.
2. Apply temperature 0.2, 1.0, and 1.5. Redraw the bar chart each time.
3. Apply top-k and top-p. Show which tokens are removed from the candidate set.
4. Apply a repetition penalty. Show the change on a prompt that causes repetition.
5. Generate five outputs for each setting. Put them side by side.
6. Send the same prompt at temperature 0 to the hosted API 20 times. Count the unique outputs.

**Show the failure:** Use greedy decoding on a long generation. Show the repetition loop.

**Expected result:** Side-by-side output grid. A count of unique outputs at temperature 0.

**Asset:** Live parameter demo. One slide that explains why temperature 0 is not deterministic (batching, floating-point order, hardware differences).

**Time:** 20 minutes.

---

## Unit 3 — Attention and transformer internals (L1)

**Goal:** The audience understands what attention does. The audience does not need the mathematics.

**Setup:**
- Get an attention visualization tool (for example, BertViz) or prepared images.
- Prepare one sentence with a pronoun that refers to an earlier noun.

**Steps:**
1. Show the sentence. Ask the audience which word "it" refers to.
2. Show the attention weights from "it" to the other words.
3. Explain Q, K, V with one analogy: a search query, index keys, and stored values.
4. Show why the cost is O(n²): each token attends to every other token. Draw the n × n grid for 4, 8, and 16 tokens.

**Show the failure:** Not needed at L1.

**Expected result:** The audience can say, in one sentence, why long context is expensive.

**Asset:** Two slides. One scope-boundary sentence for deeper questions.

**Time:** 10 minutes.

---

## Unit 4 — Long context and failure modes (L2)

**Goal:** A large context window does not mean the model uses all of it well. Position changes accuracy.

**Setup:**
- Prepare a long filler document (for example, public-domain text or unrelated reports).
- Prepare 5 "needle" facts that do not appear anywhere else in the filler.
- Prepare one question for each needle.
- Choose two context lengths (for example, 8k and 32k tokens).

**Steps:**
1. Insert one needle at 10%, 50%, and 90% depth of the document.
2. Ask the question. Record correct or incorrect.
3. Repeat for all needles, all depths, and both lengths.
4. Plot accuracy against depth. Draw one line for each context length.
5. Do a second test: give 5 relevant documents, then 20 documents (5 relevant plus 15 distractors). Compare answer accuracy.

**Show the failure:** The 20-document answer is worse than the 5-document answer, although it contains the same relevant information.

**Expected result:** An accuracy-by-depth chart. A 5-vs-20-document comparison table.

**Asset:** The accuracy-by-depth chart. This is your most important single slide.

**Time:** 25 minutes (run the test before the session; show the results and do one live query).

---

## Unit 5 — Fine-tuning: decision and QLoRA hands-on (L3)

**Goal:** Fine-tuning changes behavior, format, and tone. It does not reliably add facts. The audience runs a full QLoRA training.

**Setup:**
- Choose a small instruct model (0.5B–3B class) that trains on a free T4 GPU in 15–25 minutes. Verify the current Unsloth guidance before you choose.
- Prepare approximately 200 instruction examples in one domain. Use one clear, visible behavior change (for example, output in a fixed report format, or a specific tone).
- Prepare a held-out test set of 20 prompts.
- Save a finished adapter for the fallback path.
- Prepare a Colab notebook with cells in a fixed sequence.

**Steps:**
1. Show the decision tree: behavior/format/tone → fine-tune; facts → retrieval; both → retrieval first.
2. Run the 20 test prompts on the base model. Save the outputs.
3. Load the base model in 4-bit. Show the GPU memory use.
4. Attach the LoRA adapter. Show the number of trainable parameters as a percentage of the total.
5. Format the dataset with the model's chat template. Show one formatted example as raw text.
6. Start training. Show the loss curve as it updates.
7. Run the 20 test prompts on the fine-tuned model.
8. Show base and fine-tuned outputs side by side.
9. Save the adapter. Load it again in a new session to prove it works.

**Show the failure:** Do each of these on purpose, one at a time:
- Set the batch size too high → out-of-memory error.
- Set the learning rate too low → the loss stays flat.
- Train for too many epochs on 20 examples → the loss goes to near zero and the model repeats training data.
- Train with one chat template and run inference with a different template → the model output does not change. This is the most important failure.

**Expected result:** Side-by-side output table. A loss curve. A troubleshooting table with the four failure signatures.

**Asset:** Colab notebook. Decision tree slide (manager version). Parameter math slide (developer version). Troubleshooting one-pager. Fallback adapter.

**Time:** 90–120 minutes as a lab.

---

## Unit 6 — SLMs, edge, and on-prem (L1)

**Goal:** Small models are the correct choice under specific conditions: data residency, latency, cost at volume, or offline use.

**Setup:**
- Install a local runtime (for example, Ollama or llama.cpp) on your laptop.
- Download one 1B–3B quantized model.
- Use the fine-tuned adapter from Unit 5, merged and exported, if possible.

**Steps:**
1. Disconnect the network.
2. Run the local model on a simple task. Show the tokens per second.
3. Run the same task on a frontier API (with the network connected). Compare quality, latency, and cost.
4. Show a table of quantization levels (for example, 8-bit, 4-bit) with file size and memory use.

**Show the failure:** Give the small model a multi-step reasoning task. Show where it fails and the frontier model does not.

**Expected result:** A comparison table: model, location, quality score, latency, cost per 1,000 requests.

**Asset:** Discussion slides. One comparison table.

**Time:** 15 minutes.

---

## Unit 7 — Prompting as a software primitive (L2)

**Goal:** A prompt is an interface with a contract. Structured output with validation makes it reliable.

**Setup:**
- Prepare 20 unstructured inputs (for example, support emails or incident descriptions).
- Define one target schema with required fields, types, and an enumeration field.
- Prepare three extraction methods: prompt-only, prompt plus retry with validation errors, and constrained decoding or provider-native structured output.

**Steps:**
1. Run the 20 inputs with prompt-only extraction. Count the outputs that fail schema validation.
2. Run the same inputs with retry-on-validation-error. Count failures and total retries.
3. Run the same inputs with constrained decoding or native structured output. Count failures.
4. Show a table with failure rate, average latency, and average token cost for each method.
5. Show a system prompt and a user message that tries to override it.

**Show the failure:** Prompt-only extraction returns text around the JSON, a missing field, or an invalid enumeration value.

**Expected result:** A three-row comparison table.

**Asset:** Structured-output lab handout (short).

**Time:** 25 minutes.

---

# Module 2 — Retrieval-Augmented Generation (Units 8–11)

Use one repository for Modules 2–4. Each unit adds a tagged stage. Use one corpus for all RAG units.

**Corpus requirements:**
- 50–300 documents from one domain (for example, product documentation, internal runbooks, or public security advisories).
- The corpus must contain exact identifiers: error codes, version numbers, CVE IDs, or product SKUs. You need these for Unit 10.
- The corpus must have metadata: document type, date, product, and access level. You need these for Unit 9.
- Prepare a question set of 20 questions with reference answers and the IDs of the source chunks.

---

## Unit 8 — Chunking and embeddings (L3)

**Goal:** Chunking decisions change retrieval quality more than the embedding model choice does.

**Setup:**
- Load the corpus.
- Prepare three chunking strategies: fixed-size, structure-aware (by heading or section), and parent-child (small chunks for search, large chunks for the answer).
- Choose two embedding models from the MTEB leaderboard (one small, one large).

**Steps:**
1. Show one document split with each strategy. Highlight the chunk boundaries.
2. Show one chunk that fixed-size splitting cuts in the middle of a table or list.
3. Embed all chunks with each strategy and each model.
4. Run the 20 questions. Measure hit rate at 5 (is the correct chunk in the top 5?).
5. Show a matrix: chunking strategy × embedding model → hit rate.
6. Explain bi-encoders against cross-encoders with one diagram.
7. Show asymmetric search: a short question against a long passage.

**Show the failure:** A question whose answer spans two fixed-size chunks. Neither chunk alone answers it.

**Expected result:** A hit-rate matrix. Tag the repository `01-baseline`.

**Asset:** Chunking lab handout. Hit-rate matrix slide.

**Time:** 60 minutes as a lab.

---

## Unit 9 — Vector indexes and filtering (L2)

**Goal:** Index choice is a trade between recall, latency, and memory. Filter order changes results.

**Setup:**
- Run a vector database (for example, Qdrant or Postgres with pgvector) in a container.
- Load the chunks with metadata.
- Prepare a synthetic larger set (for example, 100,000 random vectors) for the latency test.

**Steps:**
1. Run exact (brute-force) search on the large set. Record latency. Use this as the recall reference.
2. Build an HNSW index. Run the same queries. Record recall against exact search and latency.
3. Change the HNSW search parameter (for example, `ef`). Plot recall against latency.
4. Show index memory use.
5. Run a query with a restrictive metadata filter (for example, one product, last 30 days) using post-filtering. Count the results.
6. Run the same query with pre-filtering. Count the results.

**Show the failure:** Post-filtering on a restrictive filter returns fewer results than requested, or zero results.

**Expected result:** A recall-latency curve. A pre-filter against post-filter result count.

**Asset:** Trade-off slide for architects.

**Time:** 25 minutes.

---

## Unit 10 — Hybrid search and reranking (L3)

**Goal:** Dense vectors miss exact identifiers. BM25 plus fusion plus reranking gives the largest visible quality improvement.

**Setup:**
- Add a BM25 index over the same chunks.
- Choose a cross-encoder reranker (for example, a BGE reranker or a hosted rerank API).
- Select 10 questions. At least 4 must contain an exact identifier (error code, CVE ID, version).

**Steps:**
1. Run the 10 questions with dense search only. Record whether the correct chunk is in the top 5.
2. Run the questions with BM25 only. Record the same.
3. Combine dense and BM25 results with reciprocal rank fusion. Record the same.
4. Retrieve the top 50 with hybrid search. Rerank with the cross-encoder. Keep the top 5. Record the same.
5. Show a before/after table for all 10 questions and all four methods.
6. Show the latency added by the reranker.

**Show the failure:** Dense search for an exact error code returns chunks about similar but wrong errors.

**Expected result:** A before/after chart. Tag the repository `02-retrieval`.

**Asset:** Hybrid search lab handout. Before/after rerank chart.

**Time:** 60 minutes as a lab.

---

## Unit 11 — RAG evaluation (L3)

**Goal:** Measure retrieval and generation separately. Each metric points to a different fix.

**Setup:**
- Use the 20-question set with reference answers.
- Install an evaluation framework (for example, Ragas or DeepEval).
- Choose a judge model.

**Steps:**
1. Run the full pipeline on the 20 questions. Save question, retrieved contexts, answer, and reference.
2. Calculate context precision and context recall (retrieval quality).
3. Calculate faithfulness (is the answer supported by the context?).
4. Calculate answer relevancy (does the answer address the question?).
5. Show a table: one row per question, one column per metric.
6. Select three low-score questions. For each one, decide: retrieval failure or generation failure.
7. Apply one fix for each type. Run the evaluation again. Show the change.
8. Compare 5 judge scores against your own manual judgment. Show where they disagree.

**Show the failure:** Change the prompt so that the model uses its own knowledge. Show faithfulness decrease while answer relevancy stays high.

**Expected result:** A per-question metric table. A diagnosis table: metric low → probable cause → fix. Tag the repository `03-eval`.

**Asset:** Evaluation lab handout. Evaluation results. One slide on the limits of LLM-as-judge.

**Time:** 60 minutes as a lab.

---

# Module 3 — Agents (Units 12–15)

## Unit 12 — Agent anatomy and ReAct (L3)

**Goal:** An agent is a loop: reason, act, observe. Structured output is the interface between the model and the code.

**Setup:**
- Use the RAG pipeline from Module 2 as one tool.
- Add two more simple tools (for example, a calculator and a date lookup).
- Choose an agent framework (for example, LangGraph) or write the loop yourself.

**Steps:**
1. Draw the loop on the whiteboard before you run anything.
2. Write the loop without a framework first. Show every message sent to and received from the model.
3. Run a question that needs one tool call. Show the trace.
4. Run a question that needs two tool calls in sequence (for example, retrieve a value, then calculate). Show the trace.
5. Run a question that needs no tool. Show that the agent answers directly.
6. Show the same agent built with the framework. Compare the amount of code and the visibility of the loop.
7. Show a fixed workflow for the same task. Compare cost and reliability with the agent.

**Show the failure:** Remove the iteration limit. Give an impossible task. Show the loop that does not stop.

**Expected result:** Printed traces for each question. A workflow-against-agent comparison. Tag the repository `04-agent`.

**Asset:** Agent loop diagram. Lab handout.

**Time:** 60 minutes as a lab.

---

## Unit 13 — Tools and function calling (L3)

**Goal:** Tool errors are normal. The code must detect them and give the error back to the model.

**Setup:**
- Define tool schemas with clear descriptions, parameter types, and enumerations.
- Make one tool slow (it waits longer than the timeout).
- Make one tool fail on some inputs.
- Prepare a "bad tool description" version of one tool.

**Steps:**
1. Show a tool schema. Explain that the description is part of the prompt.
2. Run a normal tool call. Show argument extraction.
3. Cause malformed arguments (for example, a wrong type). Show the validation error.
4. Return the validation error to the model as the tool result. Show the model correct itself.
5. Cause a timeout. Show the error handling and the model's next action.
6. Cause the model to call a tool that does not exist. Show the code rejecting it and returning the list of valid tools.
7. Swap in the bad tool description. Show the model choose the wrong tool.
8. Add a retry limit. Show the agent stop and report failure after the limit.

**Show the failure:** Without error feedback, the agent crashes on the first malformed call. This is the "break it on purpose" lab.

**Expected result:** A trace for each error type and its recovery. Tag the repository `05-tools`.

**Asset:** "Break it on purpose" lab handout. Error-type table: error → detection → recovery.

**Time:** 60 minutes as a lab.

---

## Unit 14 — Memory, state, and human-in-the-loop (L2)

**Goal:** Each memory strategy loses something. Human approval gates make agents acceptable for high-risk actions.

**Setup:**
- Prepare a long conversation that exceeds a small context limit (set the limit artificially low).
- Implement three memory strategies: rolling buffer, summarization, and vector memory.
- Add one "dangerous" tool (for example, "send email" or "close ticket") that needs approval.
- Configure checkpointing in the agent framework.

**Steps:**
1. Run the long conversation with each memory strategy.
2. At the end, ask about a detail from the start of the conversation. Record which strategy remembers it.
3. Show a table: strategy → what it keeps → what it loses → token cost.
4. Run the agent until it wants to call the dangerous tool. Show it pause.
5. Approve the call. Show the agent continue.
6. Run it again. Reject the call. Show the agent change its plan.
7. Stop the process during a pause. Restart it. Show that the checkpoint restores the state.

**Show the failure:** Rolling buffer forgets the user's name given in the first message.

**Expected result:** A memory comparison table. A live approval demo. Tag the repository `06-graph`.

**Asset:** Human-approval demo. Governance slide for managers.

**Time:** 30 minutes.

---

## Unit 15 — Multi-agent orchestration (L2)

**Goal:** Multi-agent systems multiply cost and propagate errors. Use them only when single-agent designs fail.

**Setup:**
- Build two agents: a researcher (uses RAG) and a writer (produces a report).
- Build two orchestration patterns: sequential and supervisor.
- Add token counting per agent and per hop.

**Steps:**
1. Run one task with a single agent. Record tokens, latency, and quality.
2. Run the same task with the sequential pattern. Record the same.
3. Run the same task with the supervisor pattern. Record the same.
4. Show a table with the three results.
5. Show a trace where the supervisor passes the full history to each worker. Show the token growth per hop.
6. Put a wrong fact in the researcher's output. Show the writer repeat it.

**Show the failure:** The supervisor sends the task back and forth between agents until the loop limit.

**Expected result:** A cost-per-run table. An error propagation example.

**Asset:** Cost-per-run slide. One comparison slide on frameworks (LangGraph, CrewAI, AutoGen).

**Time:** 25 minutes.

---

# Module 4 — Production (Units 16–19)

## Unit 16 — Serving and streaming (L3)

**Goal:** Users judge speed by time to first token. Streaming intermediate steps makes agent latency acceptable.

**Setup:**
- Wrap the agent in an async API server (for example, FastAPI).
- Build a minimal web UI (one page).
- Prepare a load-test tool.

**Steps:**
1. Serve the agent as a normal request-response endpoint. Show the user wait for the full answer.
2. Change it to stream tokens with Server-Sent Events. Show the first token appear quickly.
3. Stream intermediate events too: "searching documents", "calling tool X", "writing answer".
4. Show the event format in the browser developer tools.
5. Explain why SSE fits better than WebSockets for this case (one direction, HTTP-native, automatic reconnect).
6. Run 10 and then 50 concurrent requests. Record time to first token and total latency.
7. Cancel a request from the UI. Show the server stop the generation.

**Show the failure:** Use a blocking (not async) call inside the server. Show that concurrent requests wait in a queue.

**Expected result:** A working UI with live steps. A latency table under load. Tag the repository `07-serve`.

**Asset:** The centerpiece demo UI. Serving lab handout.

**Time:** 60 minutes as a lab.

---

## Unit 17 — Guardrails and prompt injection (L3)

**Goal:** No single defense is sufficient. Layer input checks, output checks, and privilege limits.

**Setup:**
- Prepare 10 attack inputs: direct instruction override, role-play, encoded instructions, and one indirect injection hidden in a document in the corpus.
- Add an input classifier (for example, Llama Guard or a hosted moderation model).
- Add an output check (for example, PII detection, or a check that the answer contains no system prompt text).
- Map each attack to the OWASP Top 10 for LLM Applications.

**Steps:**
1. Run all 10 attacks with no defenses. Record which succeed.
2. Add the input classifier. Run again. Record which succeed.
3. Add the output check. Run again. Record which succeed.
4. Add privilege limits: the agent's tools cannot do destructive actions without approval. Run again.
5. Upload the document with the hidden instruction. Show that the input classifier does not see it, because it arrives through retrieval.
6. Show which layer stops the indirect injection.
7. Show the false positive rate: run 20 normal questions and count wrong blocks.

**Show the failure:** The indirect injection passes the input filter. Keep one injection that is blocked live, and one that shows why layers are necessary.

**Expected result:** An attack-by-layer table. A false-positive count. Tag the repository `08-guard`.

**Asset:** Live injection demo. OWASP mapping slide.

**Time:** 45 minutes.

---

## Unit 18 — Observability and cost (L2)

**Goal:** Traces make agent behavior visible. Cost per request must be attributable to users and features.

**Setup:**
- Install a tracing tool (for example, Arize Phoenix, self-hosted, or LangSmith).
- Instrument the agent with OpenTelemetry-compatible spans.
- Add user ID and feature tags to each trace.

**Steps:**
1. Run five different questions through the agent.
2. Open the trace waterfall. Show each span: retrieval, rerank, model call, tool call.
3. Show the span that takes the most time.
4. Show time to first token and tokens per second for each model call.
5. Show token count and cost per span. Add them up for the full request.
6. Group cost by user and by feature.
7. Introduce a slow retrieval step. Show how the trace identifies it within one minute.

**Show the failure:** TTFT is high but TPS is normal. Use the trace to show that the delay is before the model call (for example, in retrieval or a long prompt).

**Expected result:** A trace waterfall. A cost-by-user table. Tag the repository `09-observe`.

**Asset:** Trace waterfall screenshot. Cost attribution slide.

**Time:** 25 minutes.

---

## Unit 19 — Manager bundle (L2)

**Goal:** Give decision-makers the numbers and the risks they need. Use your own measured data.

**Setup:**
- Collect the cost-per-run data from Units 15, 16, and 18.
- Collect the token ratios from Unit 1.
- Collect the evaluation results from Unit 11 and the attack results from Unit 17.

**Steps:**
1. Build a cost-per-user-per-month model in a spreadsheet. Inputs: requests per user per day, tokens per request, model price, retrieval and hosting cost.
2. Show the three variables that change the cost the most. Change each one live.
3. Show a build-against-buy table: hosted API, self-hosted open model, vendor product.
4. Show a data residency and governance checklist.
5. Show a list of questions to ask vendors.
6. Show the risk register: each risk mapped to a demo that you ran in this course.

**Show the failure:** Show the cost when an agent uses five times more tokens than a fixed workflow for the same task.

**Expected result:** A live cost model. A one-page decision summary.

**Asset:** Manager slide deck. Cost model spreadsheet.

**Time:** 45 minutes.

---

# Demo readiness checklist

Before each delivery, do these checks for each demo that you will use:

- [ ] The demo runs from a clean clone on a machine that is not yours.
- [ ] Library versions are pinned and recorded.
- [ ] API keys are in environment variables, not in files.
- [ ] A cached or recorded fallback exists.
- [ ] The "break it" step causes the expected failure every time.
- [ ] The expected result (table, chart, or trace) is saved as an image for the slides.
- [ ] The time in the room is measured, not estimated.
