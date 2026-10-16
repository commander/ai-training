# Addendum (2025–2026): Reciprocal Rank Fusion and rerankers

**Created/updated:** 2026-10-10

This file extends `reciprocal-rank-function.md`. We checked for new work on 2026-10-10. The list contains only peer-reviewed papers, official vendor documentation and reports, and models from known labs. It does not repeat items that the original file lists.

## New RRF features in search engines

**Weighted RRF in Elasticsearch**
Elastic. Elasticsearch documentation and Search Labs blog ("Balancing the scales: Making reciprocal rank fusion (RRF) smarter with weights", Mridula Sivanandan). September 2025. Generally available since Elasticsearch 9.2.
https://www.elastic.co/docs/reference/elasticsearch/rest-apis/retrievers/rrf-retriever
https://www.elastic.co/search-labs/blog/weighted-reciprocal-rank-fusion-rrf
Each retriever in an `rrf` retriever can now have a `weight`. The contribution of each retriever is `weight × 1 / (rank_constant + rank)`. Weights must be zero or more. This adds the weighted RRF variant from the research list (MMMORRF, Emergent Mind) to a main production engine. Use it when one retriever is known to be more reliable for your data.

## New rerankers

**Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models (Qwen3-Reranker)**
Qwen Team, Alibaba. Technical report and open models. June 2025.
https://arxiv.org/abs/2506.05176
Models: https://huggingface.co/Qwen/Qwen3-Reranker-8B
The Qwen3-Reranker series has 0.6B, 4B, and 8B models. They support 100+ languages and a 32K-token context, under Apache 2.0. They are a strong open-weight option next to the BGE rerankers in the original file. The 0.6B model fits the "fast, local" role of `bge-reranker-base`, with a longer context.

**jina-reranker-v3: Last but Not Late Interaction for Listwise Document Reranking**
Feng Wang, Yuqing Li, Han Xiao (Jina AI). arXiv technical report. September 2025 (model release: October 2025).
https://arxiv.org/abs/2509.25085
Release note: https://jina.ai/news/jina-reranker-v3-0-6b-listwise-reranker-for-sota-multilingual-retrieval/
This 0.6B model is a listwise reranker. It reads the query and up to 64 candidate documents in one 131K-token window, so documents can attend to each other. It reports 61.94 nDCG@10 on BEIR, above Qwen3-Reranker-4B, at about one sixth of the size. The original file describes only pointwise cross-encoders that score one (query, document) pair at a time.

**Rank1: Test-Time Compute for Reranking in Information Retrieval**
Orion Weller, Kathryn Ricci, Eugene Yang, Andrew Yates, Dawn Lawrie, Benjamin Van Durme (Johns Hopkins University). COLM 2025.
https://arxiv.org/abs/2502.18418
Rank1 is a reranker that generates a reasoning chain before it gives a relevance judgment. It is distilled from more than 600,000 DeepSeek-R1 reasoning traces on MS MARCO, which the authors released. It gives strong results on reasoning-heavy and instruction-following retrieval tasks. It costs more per document than a cross-encoder, so use it only on a short candidate list after RRF.

**Cohere Rerank 4 (`rerank-v4.0-pro`, `rerank-v4.0-fast`)**
Cohere. Official announcement. December 2025.
https://cohere.com/blog/rerank-4
Rerank 4 has a 32K-token context window, four times that of Rerank 3.5. It supports JSON (semi-structured) documents and 100+ languages. There are two models: `rerank-v4.0-pro` for quality and `rerank-v4.0-fast` for low latency. The original file names only the pro model.

## New benchmarks

**BRIGHT: A Realistic and Challenging Benchmark for Reasoning-Intensive Retrieval**
Hongjin Su, Howard Yen, Mengzhou Xia, et al. (The University of Hong Kong, Princeton University, Google Cloud AI Research, and others). ICLR 2025.
https://arxiv.org/abs/2407.12883
Benchmark: https://github.com/xlang-ai/BRIGHT
BRIGHT has 1,384 real queries from domains such as economics, math, and code. The answers need reasoning, not keyword or topic match. A top MTEB embedding model gets only 18.3 nDCG@10. BRIGHT is now the standard test for the reasoning rerankers above. It shows where BM25 + dense + RRF is not enough without a stronger reranker.

## Changes to earlier advice

1. **Claim:** "The consensus best practice (confirmed by a 2026 benchmark paper ...) is: Fuse broadly with RRF, then resort the top-K with the reranker's own scores." **Update:** The cited paper (arXiv 2604.01733) is real: *From BM25 to Corrective RAG: Benchmarking Retrieval Strategies for Text-and-Table Documents*, Akarsu, Karaman, Mierbach, April 2026. But it is one preprint on one financial text-and-table dataset. It is not peer reviewed. It does not prove a general consensus. The advice "RRF first, reranker second" is still a good default. Do not treat the exact gains (+17.2 pp MRR@3) as values that apply to your data.
2. **Claim:** Rerankers are cross-encoders that score each `(query, document)` pair separately. **Update:** Since 2025, listwise rerankers (jina-reranker-v3) and reasoning rerankers (Rank1) are available. Listwise models see all candidates at once. Reasoning models do better on hard queries (BRIGHT) but are slower. Select the reranker type by query difficulty and latency budget.
3. **Claim:** "Current model names: `rerank-v4.0-pro` (latest), `rerank-english-v3.0`, `rerank-multilingual-v3.0`." **Update:** Add `rerank-v4.0-fast` (December 2025). Use it in place of `rerank-v4.0-pro` when latency is important.
4. **Claim:** "Quick decision guide: Need speed on CPU / edge → `bge-reranker-base`". **Update:** Qwen3-Reranker-0.6B and jina-reranker-v3 (0.6B) are newer small options with 32K and 131K context. Test them before you select `bge-reranker-base` for long chunks.
5. **Claim:** RRF `k=60` is not universally optimal. **Update (supports the claim):** Elasticsearch 9.2 adds per-retriever weights. You can now tune both `rank_constant` and the weights in production, with no custom fusion code.
