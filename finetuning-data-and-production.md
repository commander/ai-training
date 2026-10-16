# Fine-Tuning: Datasets and Production — Resource Guide

This document has five parts:

- **Part A** — Dataset types. Which format to use for which purpose.
- **Part B** — Ready-made datasets for learning and for labs.
- **Part C** — How to make a dataset from your own documents.
- **Part D** — Data quality checklist.
- **Part E** — The production fine-tuning lifecycle.

Read Part A first. It controls the decisions in all other parts.

---

# Part A — Dataset types by purpose

First, decide what the model must do differently after training. The purpose sets the format.

| Purpose | Format | Example record | Typical size |
|---|---|---|---|
| **Instruction following / task behavior** (SFT) | Chat `messages` (ChatML) or Alpaca (`instruction`, `input`, `output`) | User asks, assistant answers | 1k–10k |
| **Multi-turn chat** | Chat `messages`, many turns | Full conversation | 1k–10k conversations |
| **Structured output** (JSON, SQL, extraction) | Chat `messages`, assistant reply is the structured output | Text in → JSON out | 500–5k |
| **Classification / routing** | Chat `messages`, assistant reply is one label | Ticket text → `"priority: high"` | 500+ per class |
| **Function / tool calling** | Chat `messages` with tool definitions and tool calls | Query → tool call → result → answer | 1k–10k |
| **Preference tuning** (DPO, ORPO) | `prompt`, `chosen`, `rejected` | One prompt, a good and a bad answer | 1k–10k pairs |
| **Continued pretraining** (domain language) | Raw `text` | Long plain text | Millions of tokens |
| **Embedding / reranker fine-tuning** (for RAG) | `anchor`, `positive`, optional `negative` | Query and matching passage | 1k–10k pairs |

**Key rule — fine-tuning teaches behavior, not facts.** Fine-tuning is good for format, style, task, and domain vocabulary. It is weak for adding new facts. For facts, use RAG. Many teams make Q&A pairs from their documents to "teach" the model the documents. This usually gives a model that sounds correct but gives incorrect details. Teach this rule to your audience.

**Best fit for enterprise documents:** structured output and classification. Example: "Read this incident report and return a JSON triage record." The model learns a task. The facts stay in the input.

**Start here:**
- **Unsloth Datasets Guide** — `unsloth.ai/docs/get-started/fine-tuning-llms-guide/datasets-guide`. Clear explanation of raw text, Alpaca, ShareGPT, and ChatML formats.
- **HF smol course, Unit 1** — `huggingface.co/learn/smol-course`. Chat templates in detail.
- **TRL dataset formats** — `huggingface.co/docs/trl` → "Dataset formats and types". The formats that `SFTTrainer` and `DPOTrainer` accept.
- **Sentence-Transformers training docs** — `sbert.net`. Formats for embedding and reranker training.

---

# Part B — Ready-made datasets for learning

## Where to look

- **`mlabonne/llm-datasets`** — `github.com/mlabonne/llm-datasets`. A curated list of post-training datasets, sorted by purpose (general, math, code, multilingual, function calling, preference). It also lists tools. Use this as your catalog.
- **Hugging Face Hub** — `huggingface.co/datasets`. Filter by task and license. Always read the dataset card.

## Recommended datasets by purpose

Always check the license on the dataset card before you use a dataset in a client or commercial setting. Some datasets are made with outputs from commercial models and have usage limits.

| Purpose | Dataset (Hugging Face ID) | Why use it |
|---|---|---|
| First lab, classic SFT | `yahma/alpaca-cleaned` | Small, simple, used in most tutorials |
| Human-written instructions | `databricks/databricks-dolly-15k` | Written by people, not by a model. Permissive license. |
| Modern general SFT | `HuggingFaceTB/smoltalk` (and SmolTalk2) | Used to train SmolLM models. High quality. |
| Modern general SFT | `mlabonne/FineTome-100k` | Filtered for quality. Used in Unsloth notebooks. |
| Multi-turn chat | `OpenAssistant/oasst2` | Human conversations with quality ratings |
| Function calling | `Salesforce/xlam-function-calling-60k` | Verified tool calls. Gated: accept terms first. |
| Function calling | `NousResearch/hermes-function-calling-v1` | Chat format with tool calls |
| Text to SQL | `b-mc2/sql-create-context` | Good structured-output lab |
| Intent classification | `PolyAI/banking77` | 77 intents. Good enterprise-style classification demo. |
| Preference (DPO) | `HuggingFaceH4/ultrafeedback_binarized` | Standard DPO dataset |
| Preference (DPO) | `argilla/distilabel-intel-orca-dpo-pairs` | Filtered with AI feedback. Small enough for a lab. |
| Embedding training | `sentence-transformers/all-nli` | Standard pairs and triplets |

**For the classroom lab:** take 200–500 rows. Train for a short time. Show the difference before and after. The goal is to see the effect, not to get the best score.

---

# Part C — Make a dataset from your own documents

## The pipeline

```
Define task → Collect → Parse → Clean → Chunk → Generate examples
→ Review → Format → Split → Version
```

### Step 1 — Define the task (do this before you touch data)

Write down:
- The input the model gets.
- The output the model must give, with an exact format.
- 10 examples, written by hand.

If you cannot write 10 examples by hand, the task is not clear. Do not continue.

### Step 2 — Collect

Best sources, in order of quality:
1. **Real input/output pairs you already have.** Tickets and resolutions. Emails and replies. Reports and summaries. These show real user language.
2. **Documents plus expert-written examples.** A subject matter expert writes 50–100 "gold" examples.
3. **Documents plus synthetic examples.** A strong model makes examples from your documents. A person reviews them.

Check before you collect: data ownership, consent, and personal data (PII).

### Step 3 — Parse

Convert PDF, DOCX, PPTX, and HTML into clean text or Markdown. Keep the headings and tables.

- **Docling** — `github.com/docling-project/docling`. Understands page layout, reading order, and tables. Runs locally, so it is good for sensitive enterprise documents.
- **Unstructured** — `github.com/Unstructured-IO/unstructured`. An alternative with many connectors.

Look at the output by eye. Bad parsing is the most common hidden problem. Tables and multi-column pages fail most often.

### Step 4 — Clean

- Remove headers, footers, page numbers, and repeated boilerplate.
- Remove exact duplicates and near duplicates (MinHash). Tools: `datatrove` (Hugging Face), and the dedup tools listed in `mlabonne/llm-datasets`.
- Remove or mask PII. Tool: **Microsoft Presidio** (`github.com/microsoft/presidio`).
- Remove documents that are old or incorrect. The model will learn them.

### Step 5 — Chunk

Split by document structure (section, heading), not by fixed length. Each chunk must make sense alone. This is the same skill as RAG chunking.

### Step 6 — Generate examples

**Manual:** Experts write examples. Slow, but gives the best quality. Use for the seed set and the test set.

**Synthetic:** A model writes examples from each chunk. Tools:

- **Meta `synthetic-data-kit`** — `github.com/meta-llama/synthetic-data-kit`. A CLI with four steps: `ingest` (PDF, DOCX, PPTX, HTML, TXT) → `create` (Q&A, chain-of-thought, summaries) → `curate` (an LLM judge removes weak examples) → `save-as` (Alpaca, ChatML, JSONL). The fastest way from documents to a training file.
- **HF Cookbook: "Documentation Chatbot with Meta Synthetic Data Kit"** — `huggingface.co/learn/cookbook/en/fine_tune_chatbot_docs_synthetic`. A complete notebook: documents → synthetic Q&A → Unsloth training. Use it as a lab template. Remember the Part A rule about facts.
- **distilabel** (Argilla) — `distilabel.argilla.io`. For larger, programmable pipelines. Also makes preference pairs and embedding training pairs.

Rules for synthetic data:
- Use your gold examples as few-shot examples in the generation prompt.
- Ask for variety: short and long inputs, different user types, edge cases.
- Add negative examples: inputs where the correct answer is "not enough information" or "out of scope". Without these, the model will guess.

### Step 7 — Review

- Filter with an LLM judge first (score each example, remove low scores).
- Then a person reviews a sample. **Argilla** (`github.com/argilla-io/argilla`) gives a review interface.
- Check the label or category balance. An unbalanced dataset gives a biased model.

### Step 8 — Format

- Convert to chat `messages` JSONL for your base model.
- Apply the model's own chat template. Do not write the template by hand.
- Train on the assistant replies only (completion-only loss). TRL and Unsloth support this.
- **Look at 5 tokenized examples** before training. Check the template, the special tokens, and the loss mask.

### Step 9 — Split

- Train / validation / test. For example 80 / 10 / 10.
- **Split by source document, not by example.** If examples from the same document are in train and test, the test score will be too high. This is data leakage.
- Make the test set by hand if possible, and never train on it.

### Step 10 — Version

Record: source documents, generation prompts, generator model, filter settings, row count, and a hash of the file. Write a short dataset card. You need this to repeat or explain any result.

## How much data?

- Classroom demo: 100–500 examples.
- Production: Hugging Face suggests 1,000 examples as a minimum and 10,000+ for robust results.
- Quality is more important than quantity. The **LIMA paper** (arXiv 2305.11206) got strong results with 1,000 carefully selected examples. Read the abstract for a good slide.

---

# Part D — Data quality checklist

Use this before every training run.

- [ ] The task and output format are written down.
- [ ] 10 hand-written examples exist and the team agrees they are correct.
- [ ] Parsed text was checked by eye, including tables.
- [ ] Duplicates and near duplicates are removed.
- [ ] PII is removed or masked.
- [ ] Negative / out-of-scope examples are included.
- [ ] Classes or categories are balanced.
- [ ] A sample was reviewed by a person.
- [ ] The chat template matches the base model.
- [ ] 5 tokenized examples were checked.
- [ ] Split is by source document. Test set is held out.
- [ ] License and ownership are confirmed for every source.
- [ ] Dataset version and card are recorded.

---

# Part E — Production fine-tuning lifecycle

A production fine-tune is a lifecycle, not a notebook. The training script is the smallest part.

## Tool roles

| Tool | Use it for | Notes |
|---|---|---|
| **TRL** (Hugging Face) | The base layer. `SFTTrainer`, `DPOTrainer`. | Unsloth and Axolotl use it. |
| **Unsloth** | Single-GPU speed and low memory. Classroom labs. | Weak on multi-GPU. |
| **Axolotl** | Production runs. One YAML file per run. Multi-GPU with DeepSpeed/FSDP. | The YAML file is your record of the run. |

For teaching: run the lab in Unsloth. Then show the same run as an Axolotl YAML file. One slide shows the path from demo to production.

## Stage 1 — Decide if you need to fine-tune

- **Mastering LLMs (Hamel Husain, Dan Becker)** — `hamel.dev/blog/posts/course/`. Recorded talks, now nearly free. Workshop 1 covers when *not* to fine-tune.
- **Chip Huyen, *AI Engineering*** (O'Reilly) — the fine-tuning chapter. A good decision framework for managers and architects.

## Stage 2 — Data

See Parts A–D. Also:
- **Modal `llm-finetuning` repo** — `github.com/modal-labs/llm-finetuning`. See `inspect_data.ipynb` for checks on tokenized data.

## Stage 3 — Repeatable training

- **Axolotl docs** — `github.com/axolotl-ai-cloud/axolotl`. Read the config guide and the dataset formats guide.
- **Modal + Axolotl tutorial** — checkpoints, resume, experiment tracking, versioned models.
- **Experiment tracking** — Weights & Biases or Trackio. Record config, dataset version, seed, and loss curves for every run.

## Stage 4 — Evaluation

- **Set a baseline before you train.** Run your task eval on the base model with a good prompt. If the fine-tune does not do better, do not ship it.
- **Task eval:** Hamel Husain's writing on evals (`hamel.dev`). Error analysis first, then LLM judges with pass/fail labels.
- **Regression eval:** EleutherAI `lm-evaluation-harness`. Checks if the model lost general ability (catastrophic forgetting).

## Stage 5 — Preference tuning (optional)

- **HF smol course, Unit 3** — DPO after SFT.
- **Maxime Labonne's DPO article** — linked from `github.com/mlabonne/llm-course`.

Teach this as "what comes after SFT." Do not build a lab for it.

## Stage 6 — Serving

- **vLLM LoRA docs** — `docs.vllm.ai/en/stable/features/lora/`. One base model, many adapters, selected per request. Do not use runtime adapter loading in production unless the environment is isolated and trusted.
- **Merge or keep separate?** Merge for one task and the fastest inference. Keep adapters separate for many tasks or tenants on one base model.
- **Quantize for deployment** — GGUF (llama.cpp, Ollama) or AWQ/GPTQ (vLLM). `mlabonne/llm-course` has quantization notebooks.
- **Use the same chat template at inference as in training.** This is the most common production bug.

## Stage 7 — Operations

- Version adapters like code: adapter + config + dataset hash + eval results.
- Test a new adapter against the current one (canary or A/B).
- Collect production failures. Add them to the next training dataset.
- Reference: **LLM Engineer's Handbook** (Iusztin, Labonne; Packt). End-to-end pipeline, including MLOps.

---

# Suggested study order (about 12 hours)

| # | Topic | Resource | Time |
|---|---|---|---|
| 1 | When to fine-tune | Mastering LLMs, Workshop 1 | 1 h |
| 2 | Formats and chat templates | Unsloth Datasets Guide + smol course Unit 1 | 2 h |
| 3 | Browse ready-made datasets | `mlabonne/llm-datasets` + 3 dataset cards | 0.5 h |
| 4 | Parse your own documents | Docling | 1 h |
| 5 | Documents to training data | `synthetic-data-kit` + HF Cookbook notebook | 2 h |
| 6 | Review and data checks | Argilla + Modal `inspect_data` | 1 h |
| 7 | Repeatable training | Axolotl config guide + one YAML run | 2 h |
| 8 | Evaluation | `lm-evaluation-harness` + your task eval | 1.5 h |
| 9 | Serving | vLLM LoRA docs | 1 h |

## Suggested lab for your training

Use your own domain documents (for example, security incident reports).

1. Parse with Docling.
2. Define the task: "incident text → JSON triage record."
3. Write 20 gold examples by hand.
4. Generate 300–500 more with `synthetic-data-kit`, using the gold examples as a guide.
5. Review a sample. Split by source document.
6. Train with Unsloth.
7. Compare with the base model on the held-out test set.

This lab covers every step in Part C. It also shows the "behavior, not facts" rule in practice.
