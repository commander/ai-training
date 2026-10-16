# Addendum (2025–2026): Embeddings, rerankers, sparse encoders, and multi-vector retrieval

**Created/updated:** 2026-10-10

This addendum extends `embeddings.md`. That file covers Sentence Transformers, bi-encoders, cross-encoders, sparse encoders, multi-vector (late interaction) models, CLIP, ColPali, and survey papers. This addendum adds work from 2025 and 2026 on the same topics. The check date is 2026-10-10. Items from 2026 have the label **[2026]**.

## New tools and libraries

- **Sentence Transformers v5.0.0.** Hugging Face. GitHub release, Jul 2025. https://github.com/huggingface/sentence-transformers/releases/tag/v5.0.0
  This release added the `SparseEncoder` model type that the original file mentions. It also added `encode_query` and `encode_document`, which apply the correct query or document prompt for each model. Use these two methods with the new instruction-based models below.

## New embedding models (technical reports)

- **Gemini Embedding: Generalizable Embeddings from Gemini.** Lee, Chen, Dua et al. (Google). arXiv, Mar 2025. https://arxiv.org/abs/2503.07891
  Google initializes an embedding model from the Gemini LLM. At release, it had the top score on the MMTEB multilingual, English, and code benchmarks. It is an example of the "LLM used directly as an embedding model" group in the Nie et al. (2024) survey.

- **Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models.** Zhang, Li, Long et al. (Alibaba Qwen team). arXiv, Jun 2025. https://arxiv.org/abs/2506.05176
  This series has open-weight embedding models and reranker models in 0.6B, 4B, and 8B sizes. The Qwen3 LLM also makes synthetic training data. You can use the embedding model for the first step and the reranker for the second step of the "Retrieve & Re-Rank" system in the original file.

- **EmbeddingGemma: Powerful and Lightweight Text Representations.** Schechter Vera, Dua, Zhang et al. (Google). arXiv, Sep 2025. https://arxiv.org/abs/2509.20354
  This is an open 300M-parameter model for on-device use. On MTEB it is better than prior models with fewer than 500M parameters. Its quality stays high with quantized weights and truncated (Matryoshka) embeddings. It is a good default when you need a small bi-encoder.

- **jina-embeddings-v4: Universal Embeddings for Multimodal Multilingual Retrieval.** Günther, Sturua, Akram et al. (Jina AI). arXiv, Jun 2025. https://arxiv.org/abs/2506.18902
  One 3.8B model gives both single-vector and multi-vector (late interaction) outputs for text and images. It is strong on visually rich documents such as tables and charts. It joins the "Embedding" and "Multi-vector" columns of the comparison table in the original file, and it is an alternative to ColPali.

- **[2026] Gemini Embedding 2: A Native Multimodal Embedding Model from Gemini.** Shanbhogue, Li, Zhang, Hernández Ábrego et al. (Google). arXiv, May 2026. https://arxiv.org/abs/2605.27295
  This model puts text, image, video, and audio into one embedding space. It reports state-of-the-art results on text, cross-modal, and multimodal retrieval (for example, MTEB Multilingual 69.9). It extends the CLIP idea in the original file to more modalities.

## New research papers

- **On the Theoretical Limitations of Embedding-Based Retrieval.** Weller, Boratko, Naim, Lee et al. (Google DeepMind and Johns Hopkins University). ICLR 2026. https://arxiv.org/abs/2508.21038
  The paper proves that the embedding dimension limits the number of top-k document sets that a single-vector model can return. The LIMIT dataset tests this with very simple queries. Single-vector models get less than 20% recall@2. A multi-vector model (GTE-ModernColBERT) does better, BM25 gets about 98%, and an LLM reranker gets 100%.

- **ReasonIR: Training Retrievers for Reasoning Tasks.** Shao, Qiao, Kishore et al. (Meta FAIR and others). arXiv, Apr 2025. https://arxiv.org/abs/2504.20595 . Code: https://github.com/facebookresearch/ReasonIR
  Most embedding models are trained for short factual queries. ReasonIR-8B is trained on synthetic hard queries and hard negatives for reasoning tasks. It reports 29.9 nDCG@10 on the BRIGHT benchmark without a reranker and 36.9 with a reranker.

## New benchmarks and datasets

- **MMTEB: Massive Multilingual Text Embedding Benchmark.** Enevoldsen, Chung, Kerboua et al. (MTEB community). ICLR 2025. https://arxiv.org/abs/2502.13595
  MMTEB expands MTEB to more than 500 tasks in more than 250 languages. It adds instruction following, long-document retrieval, and code retrieval. The MTEB leaderboard in the original file now uses benchmarks from MMTEB, for example MTEB(Multilingual). The paper found that a 560M model (multilingual-e5-large-instruct) was the best public model at that time, so a larger model is not always better.

- **ViDoRe Benchmark V2: Raising the Bar for Visual Retrieval.** Macé, Loison, Faysse et al. (Illuin Technology and partners). arXiv, May 2025. https://arxiv.org/abs/2505.17166
  Top models scored more than 90% nDCG@5 on the first ViDoRe benchmark from the ColPali paper. V2 adds harder, multilingual, and cross-document queries. Use it to compare ColPali-type models.

- **[2026] ViDoRe V3: A Comprehensive Evaluation of Retrieval Augmented Generation in Complex Real-World Scenarios.** Loison, Macé, Edy et al. ACL 2026 (arXiv Jan 2026). https://arxiv.org/abs/2601.08620 . ACL Anthology: https://aclanthology.org/2026.acl-long.755/
  V3 has about 26,000 document pages and 3,099 human-checked queries in 6 languages, from 10 professional domains. It evaluates retrieval, visual grounding, and answer generation. It reports that visual retrievers are better than text retrievers, and that late interaction models and text rerankers improve results.

## Changes to earlier advice

- **Claim in the original:** In the comparison table, a single-vector "Embedding" has "Good" accuracy, and the table implies it is enough for most search.
  **Update:** Weller et al. (ICLR 2026) prove that single-vector embeddings cannot represent all top-k combinations for a fixed dimension. For queries that combine many conditions, add a sparse (BM25 or SPLADE) step, a multi-vector model, or a reranker. This supports the hybrid and two-step designs in the original file.

- **Claim in the original:** "Papers from 2022 and earlier do not include LLM-based embedding models. For the newest models, also check the MTEB leaderboard."
  **Update:** In 2025–2026, the top models are LLM-based (Gemini Embedding, Qwen3 Embedding). Small models such as EmbeddingGemma (300M) are also strong. On the leaderboard, use the MMTEB benchmarks (for example, MTEB(Multilingual)) and compare scores for your task type, not only the mean score.

- **Claim in the original:** ColPali is the method to search PDFs and scanned documents without OCR.
  **Update:** This is still correct. Now there are also general models that give multi-vector outputs for images and text (jina-embeddings-v4, 2025). Use ViDoRe V2 and V3 to select a model, because the ViDoRe V1 benchmark is nearly saturated.
