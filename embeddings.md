Here is a reading list, from practical to theoretical.

**Practical documentation**

- [Sentence Transformers documentation (sbert.net)](https://www.sbert.net). This is the main resource for the library that you use. It explains how to compute embeddings with Sentence Transformer models, similarity scores with Cross-Encoder (reranker) models, and sparse embeddings with Sparse Encoder models. Read the "Quickstart" section and the "Retrieve & Re-Rank" example. The example shows the two-step search system that I described.
- [Sentence Transformers on GitHub](https://github.com/huggingface/sentence-transformers). This has code examples for all three model types.
- [MTEB leaderboard on Hugging Face](https://huggingface.co/spaces/mteb/leaderboard). Use this to compare embedding models by task, size, and score.

**Research papers (the original ideas)**

- *Sentence-BERT* (Reimers & Gurevych, 2019): [arxiv.org/abs/1908.10084](https://arxiv.org/abs/1908.10084). This paper explains bi-encoders and why they are faster than cross-encoders.
- *Dense Passage Retrieval (DPR)* (Karpukhin et al., 2020): [arxiv.org/abs/2004.04906](https://arxiv.org/abs/2004.04906). This paper shows that dense retrieval can perform better than BM25 keyword search.
- *Passage Re-ranking with BERT* (Nogueira & Cho, 2019): [arxiv.org/abs/1901.04085](https://arxiv.org/abs/1901.04085). This is an early paper about cross-encoder rerankers.
- *SPLADE* (Formal et al., 2021): [arxiv.org/abs/2107.05720](https://arxiv.org/abs/2107.05720). This paper describes the sparse encoder method.
- *ColBERT* (Khattab & Zaharia, 2020): [arxiv.org/abs/2004.12832](https://arxiv.org/abs/2004.12832). This describes a fourth type, "late interaction". It is between bi-encoders and cross-encoders in speed and accuracy.
- *CLIP* (Radford et al., 2021): [arxiv.org/abs/2103.00020](https://arxiv.org/abs/2103.00020). This explains the image-text model that you used.

**Recommended order:** Start with the sbert.net Quickstart and the Retrieve & Re-Rank example. Then read the Sentence-BERT paper and the SPLADE paper. Read the other papers if you want more detail.


**Multi-vector encoder models (late interaction models)**

A normal embedding model gives one vector for the full text. A multi-vector model gives one vector for each token. A sentence with 20 tokens gives 20 vectors, usually of 128 numbers each.

To compare a query and a document, the model uses "MaxSim". For each query token vector, it finds the most similar document token vector. Then it adds these maximum values to get the final score. This step is "late interaction", because the query and document interact only after the model encodes them separately.

**Why it is useful:** This method is between bi-encoders and cross-encoders.

- As with a bi-encoder, you can encode all documents before the search and store the vectors.
- As with a cross-encoder, the model compares the texts token by token. Thus, it keeps more detail than one vector, and it is usually more accurate than a dense model. It also works better on long documents and on words that the model did not see during training.

**Disadvantage:** Storage is much larger, because you store one vector for each token, not one vector for each document. Newer methods (ColBERTv2, PLAID) compress the vectors to decrease this problem.

**Image version:** ColPali applies the same method to images of document pages. It encodes each page image into many patch vectors. Thus, you can search PDFs with tables and figures without OCR.

**Updated comparison**

| | Embedding | Multi-vector | Reranker | Sparse encoder |
|---|---|---|---|---|
| Output | 1 vector per text | 1 vector per token | 1 score per pair | Word weights |
| Encodes documents before search | Yes | Yes | No | Yes |
| Speed | Fast | Medium | Slow | Fast |
| Accuracy | Good | Better | Best | Good |
| Storage | Small | Large | None | Small |

**Further reading**

Papers:

- *ColBERT* (Khattab & Zaharia, 2020): [arxiv.org/abs/2004.12832](https://arxiv.org/abs/2004.12832). This is the original late interaction paper. Start here.
- *ColBERTv2* (Santhanam et al., 2021): [arxiv.org/abs/2112.01488](https://arxiv.org/abs/2112.01488). This paper decreases the storage size with compression.
- *PLAID* (Santhanam et al., 2022): [arxiv.org/abs/2205.09707](https://arxiv.org/abs/2205.09707). This paper makes the search faster.
- *ColPali* (Faysse et al., 2024): [arxiv.org/abs/2407.01449](https://arxiv.org/abs/2407.01449). This paper applies the method to document page images.

Libraries:

- [PyLate](https://github.com/lightonai/pylate). This library is built on sentence-transformers. It is the easiest way to use and train multi-vector models if you already know sentence-transformers.
- [Stanford ColBERT repository](https://github.com/stanford-futuredata/ColBERT). This is the official code from the ColBERT authors.
- [RAGatouille](https://github.com/AnswerDotAI/RAGatouille). This is a simple wrapper to use ColBERT in RAG pipelines.

**Recommended order:** Read the ColBERT paper first. Then try PyLate in your notebook. Read ColPali if you must search PDFs or scanned documents.


# Survey Papers

Here are survey papers, in groups by topic. For each group, I list the recommended paper first.

**1. General text embeddings (start here)**

- *On The Role of Pretrained Language Models in General-Purpose Text Embeddings: A Survey* (Zhao et al., 2025): [arxiv.org/abs/2507.20783](https://arxiv.org/abs/2507.20783). This is the most recent broad survey. It also covers multilingual support, multimodal integration, code understanding, and adaptation to specific tasks.
- *When Text Embedding Meets Large Language Model: A Comprehensive Survey* (Nie et al., 2024): [arxiv.org/abs/2412.09165](https://arxiv.org/abs/2412.09165). It puts the work into three groups: LLMs that improve normal embedding methods, LLMs used directly as embedding models, and LLMs used to analyze and interpret embeddings. This is relevant to models such as Qwen3-VL-Embedding.
- *Recent advances in text embedding: A Comprehensive Review of Top-Performing Methods on the MTEB Benchmark* (2024): [arxiv.org/abs/2406.01607](https://arxiv.org/abs/2406.01607). This paper compares the methods of the best models on the MTEB leaderboard.

**2. Retrieval: dense, sparse, multi-vector, and rerankers**

- *Dense Text Retrieval based on Pretrained Language Models: A Survey* (Zhao et al., 2022): [arxiv.org/abs/2211.14876](https://arxiv.org/abs/2211.14876). It organizes the work in four aspects: architecture, training, indexing, and integration, and it has more than 300 references. The authors also released a code repository and a toolkit for dense retrieval models.
- *Pretrained Transformers for Text Ranking: BERT and Beyond* (Lin, Nogueira & Yates, 2020): [arxiv.org/abs/2010.06467](https://arxiv.org/abs/2010.06467). This is a book-length survey. It explains cross-encoder rerankers and multi-stage search systems in detail.
- *Semantic Models for the First-stage Retrieval: A Comprehensive Review* (Guo et al., 2021): [arxiv.org/abs/2103.04831](https://arxiv.org/abs/2103.04831). This paper compares sparse, dense, and hybrid retrieval methods.
- *Large Language Models for Information Retrieval: A Survey* (Zhu et al., 2023): [arxiv.org/abs/2308.07107](https://arxiv.org/abs/2308.07107). This paper covers LLMs as retrievers, rerankers, and query rewriters.

**3. Multimodal embeddings (images, video, documents)**

- *Vision-Language Models for Vision Tasks: A Survey* (Zhang et al., 2023): [arxiv.org/abs/2304.00685](https://arxiv.org/abs/2304.00685). This paper explains CLIP-type models and their training.
- *A Survey of Multimodal Retrieval-Augmented Generation* (Mei et al., 2025): [arxiv.org/abs/2504.08748](https://arxiv.org/abs/2504.08748). This paper covers multimodal retrieval in RAG systems.
- *Composed Multi-modal Retrieval: A Survey of Approaches and Applications* (Zhang et al., 2025): [arxiv.org/abs/2503.01334](https://arxiv.org/abs/2503.01334). This paper covers search with queries that combine image and text.

**4. Word embeddings and foundations (background)**

- *Word Embeddings: A Survey* (Almeida & Xexéo, 2019): [arxiv.org/abs/1901.09069](https://arxiv.org/abs/1901.09069). This paper covers word2vec, GloVe, and earlier methods.
- *Representation Learning: A Review and New Perspectives* (Bengio, Courville & Vincent, 2012): [arxiv.org/abs/1206.5538](https://arxiv.org/abs/1206.5538). This is the classic theory paper about learned representations.

**5. Other data types (optional)**

- *Graph Embedding Techniques, Applications, and Performance: A Survey* (Goyal & Ferrara, 2017): [arxiv.org/abs/1705.02801](https://arxiv.org/abs/1705.02801).
- *A Survey on Knowledge Graphs: Representation, Acquisition and Applications* (Ji et al., 2020): [arxiv.org/abs/2002.00388](https://arxiv.org/abs/2002.00388).

**Recommended order:** Read the 2025 general survey (2507.20783) first. Then read the dense retrieval survey (2211.14876) for search systems. Then read the multimodal surveys for your image work.

**Note:** Embedding research changes quickly. Papers from 2022 and earlier do not include LLM-based embedding models. For the newest models, also check the MTEB leaderboard.