There is no single paper that defines the "parent-document retriever." LangChain and LlamaIndex made the pattern popular: you search on small chunks, then return the larger parent section to the LLM. These papers explain why the pattern works and how the hierarchical versions work. Read them in this order.

**Why chunk size matters**

1. **Dense X Retrieval: What Retrieval Granularity Should We Use?** (Chen et al., 2023). [arXiv 2312.06648](https://arxiv.org/abs/2312.06648). This paper compares passage, sentence, and proposition units. It shows that small units give more precise matches. This is the "search small" part of the pattern.
2. **Lost in the Middle** (Liu et al., 2023). [arXiv 2307.03172](https://arxiv.org/abs/2307.03172). This paper shows that LLMs use information at the middle of a long context less well. It explains why you must control how much parent text you return.
3. **LongRAG** (Jiang et al., 2024). [arXiv 2406.15319](https://arxiv.org/abs/2406.15319). This paper takes the opposite approach. It uses very large retrieval units and a long-context reader. It is a good way to see the trade-off.

**Hierarchical retrieval**

4. **RAPTOR** (Sarthi et al., ICLR 2024). [arXiv 2401.18059](https://arxiv.org/abs/2401.18059). This is the most important paper for hierarchical retrieval. It clusters chunks, summarizes the clusters, and repeats this to make a tree. At query time, it retrieves from all levels of the tree.
5. **Walking Down the Memory Maze (MemWalker)** (Chen et al., 2023). [arXiv 2310.05029](https://arxiv.org/abs/2310.05029). This paper also builds a summary tree. But the LLM navigates the tree from the root to the leaves, and does not use a flat vector search.
6. **From Local to Global: A GraphRAG Approach** (Edge et al., Microsoft, 2024). [arXiv 2404.16130](https://arxiv.org/abs/2404.16130). This paper makes a hierarchy of entity-graph communities with summaries at each level. It is useful for "global" questions about a full corpus.

**Keeping the parent context in the chunk**

7. **Late Chunking** (Günther et al., Jina AI, 2024). [arXiv 2409.04701](https://arxiv.org/abs/2409.04701). This paper embeds the full document first and then pools the embeddings for each chunk. Each chunk embedding then contains context from its parent.
8. **Contextual Retrieval** (Anthropic blog, 2024). [Link](https://www.anthropic.com/news/contextual-retrieval). This is not a paper. It adds an LLM-generated description of the parent document to each chunk before indexing. It is a practical alternative to the parent-document pattern.

**Background**

9. **Retrieval-Augmented Generation for LLMs: A Survey** (Gao et al., 2023). [arXiv 2312.10997](https://arxiv.org/abs/2312.10997). This survey describes "small-to-big" retrieval and other chunking methods, and gives a clear overview.

If you only have time for three papers, read Dense X, RAPTOR, and Late Chunking. My list is from my own knowledge. It can miss papers from 2025 and 2026. I can do a search for newer work if you want.