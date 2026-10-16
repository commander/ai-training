# Addendum (2025–2026): Parent-document and hierarchical retrieval

**Created/updated:** 2026-10-10

This file extends `hierarchical-retrieval.md`. We checked for new work on 2026-10-10. The original file says that its list "can miss papers from 2025 and 2026". This addendum fills that gap. It contains only peer-reviewed papers, official vendor reports, and widely used open-source tools. It does not repeat items that the original file lists.

## Why chunk size matters

**Is Semantic Chunking Worth the Computational Cost?**
Renyi Qu, Ruixuan Tu, Forrest Sheng Bao (Vectara, University of Wisconsin–Madison). Findings of NAACL 2025. October 2024 (arXiv), April 2025 (NAACL).
https://arxiv.org/abs/2410.13070
The authors compare semantic chunking with fixed-size chunking on retrieval and answer tasks. On documents with a typical structure, fixed-size chunks perform as well or better, at a lower cost. This supports the original file's "search small" advice: small fixed chunks plus a parent return step are a strong baseline.

## Hierarchical retrieval

**SiReRAG: Indexing Similar and Related Information for Multihop Reasoning**
Nan Zhang et al. (Penn State University, Salesforce AI Research). ICLR 2025.
https://arxiv.org/abs/2412.06206
SiReRAG builds two trees. The first is a RAPTOR-style similarity tree made with recursive summaries. The second is a relatedness tree made from propositions that share entities. Both trees go into one flat retrieval pool. It is a direct extension of RAPTOR (item 4 in the original file) and of the proposition units in Dense X (item 1). It improves F1 on MuSiQue, 2WikiMultiHopQA, and HotpotQA.

**From RAG to Memory: Non-Parametric Continual Learning for Large Language Models (HippoRAG 2)**
Bernal Jiménez Gutiérrez, Yiheng Shu, Weijian Qi, Sizhe Zhou, Yu Su (Ohio State University). ICML 2025.
https://arxiv.org/abs/2502.14802
Code: https://github.com/OSU-NLP-Group/HippoRAG
HippoRAG 2 links passages and entities in one graph and uses Personalized PageRank for retrieval. It compares against RAPTOR, GraphRAG, and LightRAG. The abstract states that for structure-augmented RAG, "performance on more basic factual memory tasks drops considerably below standard RAG". HippoRAG 2 removes this drop. Read it as a counterpoint to the tree methods in the original file.

**Hierarchical Retrieval: The Geometry and a Pretrain-Finetune Recipe**
Chong You, Rajesh Jayaram, Ananda Theertha Suresh, Robin Nittka, Felix Yu, Sanjiv Kumar (Google Research). NeurIPS 2025.
https://arxiv.org/abs/2509.16411
This paper studies dual encoders when the correct answers for a query are all of its ancestors in a hierarchy. It proves that a dual encoder can do this when the embedding dimension grows with the depth of the hierarchy. It also finds a "lost-in-the-long-distance" effect: recall drops for distant ancestors. A pretrain-finetune recipe raises long-distance recall from 19% to 76%. It gives a theory base for the "return the parent" step.

**LeanRAG: Knowledge-Graph-Based Generation with Semantic Aggregation and Hierarchical Retrieval**
Yaoze Zhang, Rong Wu, Pinlong Cai, Xiaoman Wang, Guohang Yan, Song Mao, Ding Wang, Botian Shi. AAAI 2026. August 2025 (arXiv).
https://arxiv.org/abs/2508.10391
Code: https://github.com/RaZzzyz/LeanRAG
LeanRAG builds a multi-level graph of entity clusters. It also adds explicit links between the high-level summaries. At query time, it starts from fine entities and goes up the tree to their lowest common ancestor. This builds a small and connected context. The paper reports 46% less retrieved redundancy than flat retrieval. It extends the GraphRAG community hierarchy (item 6 in the original file).

## New benchmarks

**When to use Graphs in RAG: A Comprehensive Analysis for Graph Retrieval-Augmented Generation (GraphRAG-Bench)**
Zhishang Xiang, Chuanjie Wu, Qinggang Zhang et al. (Xiamen University, Hong Kong Polytechnic University). ICLR 2026. June 2025 (arXiv).
https://arxiv.org/abs/2506.05690
Benchmark: https://github.com/GraphRAG-Bench/GraphRAG-Benchmark
The authors note that GraphRAG often does worse than plain RAG on real tasks. Their benchmark has tasks from simple fact retrieval to summarization and creative generation. It shows which task types benefit from a hierarchy and which do not. Use it before you choose GraphRAG or RAPTOR for a project.

## Keeping the parent context in the chunk

**voyage-context-3: contextualized chunk embeddings**
Voyage AI (MongoDB). Official release note. 23 July 2025.
https://www.mongodb.com/company/blog/product-release-announcements/voyage-context-3-focused-chunk-level-details-global-document-context
The model reads the full document in one forward pass. It then outputs one embedding for each chunk that also encodes document-level context. Voyage reports better chunk-level retrieval than Jina late chunking and than Anthropic Contextual Retrieval on its evaluation set. This is a hosted product form of the late-chunking idea (item 7 in the original file). It needs no LLM call per chunk, unlike Contextual Retrieval (item 8).

## New tools and libraries

**PageIndex**
VectifyAI. Open-source repository. 2025.
https://github.com/VectifyAI/PageIndex
PageIndex builds a table-of-contents tree with a summary at each node for a long document. An LLM then reasons down the tree to find the relevant section. It uses no vector database and no fixed chunks. It is a practical tool for the MemWalker approach (item 5 in the original file). It works best on long, well-structured documents such as financial filings.

## Changes to earlier advice

1. **Claim:** "RAPTOR is the most important paper for hierarchical retrieval." **Update:** RAPTOR is still the base reference. But GraphRAG-Bench (ICLR 2026) and HippoRAG 2 (ICML 2025) show that tree and graph methods do not help on all tasks. They can do worse than flat retrieval on simple factual questions. Test a flat small-to-big baseline first.
2. **Claim:** Late chunking and Contextual Retrieval are the two ways to keep parent context in the chunk. **Update:** In 2025, a third option became available: contextualized chunk embedding models such as voyage-context-3. These give the same effect as late chunking with a model that is trained for this task.
3. **Claim (implied):** The parent-document pattern needs a vector search over small chunks. **Update:** LLM-navigated tree indexes (PageIndex) became a working alternative for single long documents in 2025. Vector search is still the better choice for large collections of many documents.
4. **Recommended reading:** If you have time for only three new papers, read HippoRAG 2, GraphRAG-Bench, and SiReRAG.
