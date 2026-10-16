# Addendum (2025–2026): Document parsing for LLM datasets

**Created/updated:** 2026-10-10

This file extends `document-parsing.md`. We checked for new work on 2026-10-10. The list contains only peer-reviewed papers, official technical reports from known labs, recognized benchmarks, and widely used open-source tools. It does not repeat items that the original file lists.

## New benchmarks and benchmark updates

**OmniDocBench v1.5 and CVPR 2025 publication**
Ouyang et al. (Shanghai AI Laboratory, OpenDataLab). CVPR 2025. v1.5 update: September 2025.
https://github.com/opendatalab/OmniDocBench
The original file cites the 2024 arXiv version. The paper was accepted at CVPR 2025. Version 1.5 adds 374 pages, raises newspaper and note images from 72 DPI to 200 DPI, and lets formulas match text in scoring. Compare tools on v1.5 scores, not v1.0 scores. In 2026, the top of the leaderboard has compact VLMs of about 1B parameters.

**olmOCR-Bench**
Allen Institute for AI (Ai2). Released with olmOCR; updated with olmOCR 2. 2025.
https://github.com/allenai/olmocr/tree/main/olmocr/bench
The benchmark has more than 7,000 binary unit tests on about 1,400 PDFs. Tests cover old scans with math, tables, headers and footers, multi-column layouts, and very small text. Each test checks one fact, for example "this table cell is present" or "this header is removed". This makes results easy to read. It is a good second benchmark next to OmniDocBench.

## New PDF parsing models and methods

**olmOCR 2: Unit Test Rewards for Document OCR**
Jake Poznanski, Luca Soldaini, Kyle Lo (Ai2). Technical report. October 2025.
https://arxiv.org/abs/2510.19817
olmOCR 2 replaces the first olmOCR model in the original file. It is a 7B VLM trained with reinforcement learning. The rewards are the binary unit tests from olmOCR-Bench, run on synthetic documents with known HTML source. Math, tables, and multi-column pages improve. Model, data, and code are under open licenses.

**MinerU2.5: A Decoupled Vision-Language Model for Efficient High-Resolution Document Parsing**
Junbo Niu, Zheng Liu, Zhuangcheng Gu, Bin Wang, et al. (Shanghai AI Laboratory, OpenDataLab). Technical report. September 2025.
https://arxiv.org/abs/2509.22186
This is the 2025 successor to MinerU in the original file. It is a 1.2B VLM with two stages. First, it finds the layout on a small image. Then, it reads each region at full resolution. This gives high accuracy on dense text, formulas, and tables at low compute cost.

**PaddleOCR-VL: Boosting Multilingual Document Parsing via a 0.9B Ultra-Compact Vision-Language Model**
Cheng Cui, Ting Sun, Suyin Liang, et al. (Baidu, PaddlePaddle team). Technical report. October 2025.
https://arxiv.org/abs/2510.14528
PaddleOCR-VL is a 0.9B model with a dynamic-resolution vision encoder and ERNIE-4.5-0.3B as the language model. It supports 109 languages. It reads text, tables, formulas, and charts. It is a strong choice for non-English PDF collections. Later versions of this family are near the top of OmniDocBench v1.5.

**Dolphin: Document Image Parsing via Heterogeneous Anchor Prompting**
Hao Feng, Shu Wei, Xiang Fei, et al. (ByteDance). ACL 2025.
https://arxiv.org/abs/2505.14059
Dolphin first outputs layout elements in reading order. It then uses each element as an "anchor" prompt and parses all elements in parallel. It is trained on more than 30 million samples. It is the main peer-reviewed example of the "layout first, then content" design inside one model.

**DeepSeek-OCR: Contexts Optical Compression**
Haoran Wei, Yaofeng Sun, Yukun Li (DeepSeek-AI). Technical report. October 2025.
https://arxiv.org/abs/2510.18234
DeepSeek-OCR encodes a page into a small number of vision tokens. It keeps about 97% OCR precision at 10× compression (text tokens to vision tokens). The authors report 200,000+ pages per day on one A100-40G GPU for LLM and VLM training data. This makes it directly relevant to large-scale dataset creation.

**DeepSeek-OCR 2: Visual Causal Flow (2026)**
Haoran Wei, Yaofeng Sun, Yukun Li (DeepSeek-AI). Technical report. January 2026.
https://arxiv.org/abs/2601.20552
Code: https://github.com/deepseek-ai/DeepSeek-OCR-2
The new DeepEncoder V2 reorders vision tokens by page meaning, not in fixed raster order. The goal is a better reading order on complex layouts. Weights are under Apache 2.0.

## New tools and libraries

**SmolDocling and Granite-Docling-258M**
Ahmed Nassar et al. (IBM Research, Hugging Face). SmolDocling technical report: March 2025. Granite-Docling-258M release: September 2025.
https://arxiv.org/abs/2503.11576
https://huggingface.co/ibm-granite/granite-docling-258M
SmolDocling is a 256M VLM that converts a full page to "DocTags". DocTags keep content, structure, and location. Granite-Docling-258M is the Apache 2.0 successor. IBM reports that table TEDS goes from 0.82 to 0.97. Both models plug into the Docling toolkit in the original file. They give Docling an end-to-end VLM path next to its pipeline path.

**Docling: An Efficient Open-Source Toolkit for AI-driven Document Conversion**
Nikolaos Livathinos, Christoph Auer, Maksym Lysak, Ahmed Nassar, et al. (IBM Research). AAAI 2025 Workshop on Open-Source AI for Mainstream Use. January 2025.
https://arxiv.org/abs/2501.17887
This is the updated, workshop-reviewed paper for Docling. It replaces the 2024 technical report in the original file as the main reference. It describes the modular design, the MIT license, and integration with LangChain, LlamaIndex, and spaCy.

**Mistral OCR (hosted API)**
Mistral AI. Official announcement. March 2025.
https://mistral.ai/news/mistral-ocr
Mistral OCR is a hosted API that converts PDFs and images to ordered text and images, with tables and equations. The launch price was about 1,000 pages per US dollar. The announcement page now states that newer Mistral OCR models replace this first version. Check the current Mistral docs for the model name before you use it.

## Changes to earlier advice

1. **Claim:** "olmOCR (Allen AI, 2025) ... This paper is the closest match to your question." **Update:** Use olmOCR 2 (October 2025) and the current `olmocr` toolkit. olmOCR 2 is trained with RL on unit-test rewards and scores about 4 points higher on olmOCR-Bench.
2. **Claim:** MinerU (2024) is the open-source pipeline for high-quality PDF extraction. **Update:** MinerU2.5 (September 2025) replaces the multi-model pipeline with one 1.2B VLM. Use MinerU2.5 for new projects.
3. **Claim:** The 2024 survey compares pipeline methods with end-to-end VLMs. **Update:** In 2025–2026, small VLMs of 0.3B to 1.2B (MinerU2.5, PaddleOCR-VL, Granite-Docling, DeepSeek-OCR) lead OmniDocBench v1.5. Many use a "layout first, then content" design inside one model (MinerU2.5, Dolphin). Large general VLMs are no longer required for the best parsing quality.
4. **Claim:** OmniDocBench (2024, arXiv) measures parsing quality. **Update:** Cite the CVPR 2025 version. Use the v1.5 scores, because v1.0 and v1.5 scores do not compare directly.
5. **Claim (implied):** Throughput for pretraining-scale extraction needs large GPU clusters. **Update:** DeepSeek-OCR reports 200,000+ pages per day on one A100-40G. Compact models make OCR of large PDF collections practical on a small GPU budget.
