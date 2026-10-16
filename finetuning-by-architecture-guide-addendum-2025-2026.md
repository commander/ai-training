# Addendum (2025–2026): Fine-tuning by architecture (encoder-only, encoder-decoder, decoder-only)

**Created/updated:** 2026-10-10

This file extends `finetuning-by-architecture-guide.md`. It lists 2025–2026 work that updates or extends that guide. The check was done on 2026-10-10. Only papers from strong labs or peer-reviewed venues, official technical reports, and widely used open-source tools are included. Items from 2026 are marked **[2026]**.

---

## New papers: architecture comparison

**Seq vs Seq: An Open Suite of Paired Encoders and Decoders (Ettin)**
Orion Weller, Kathryn Ricci, Marc Marone, et al. (Johns Hopkins University). ICLR 2026. arXiv, July 2025.
<https://arxiv.org/abs/2507.11412> · Blog: <https://huggingface.co/blog/ettin>
The authors train encoder-only and decoder-only models with the same data and recipe, from 17M to 1B parameters. Encoders win on classification and retrieval, and decoders win on generation. Continued training to change a decoder into an encoder (or the opposite) gives worse results than a model trained with the correct objective. A 400M encoder beats a 1B decoder on MNLI. This is a controlled update to the Wang et al. (2022) comparison in the original "Architecture background" list.

## New encoder-only models

**mmBERT: A Modern Multilingual Encoder with Annealed Language Learning**
Marc Marone, Orion Weller, William Fleshman, et al. (Johns Hopkins University). arXiv, September 2025.
<https://arxiv.org/abs/2509.06888> · Blog: <https://huggingface.co/blog/mmbert>
A multilingual ModernBERT-style encoder, pretrained on 3T tokens in more than 1800 languages. Use it in place of `xlm-roberta` or mBERT for multilingual classification, NER, and retrieval. The fine-tuning code in the original Section 1 does not change. Check the target module names, because they differ from BERT.

**EuroBERT: Scaling Multilingual Encoders for European Languages**
Nicolas Boizard, Hippolyte Gisserot-Boukhlef, Duarte M. Alves, et al. arXiv, March 2025.
<https://arxiv.org/abs/2503.05500>
A family of multilingual encoders for European and other widely spoken languages. It uses methods from modern decoders and supports sequences of up to 8,192 tokens. The authors also release intermediate checkpoints and the training framework. It is an alternative to mmBERT when the target languages are European.

## New encoder-decoder models

**Encoder-Decoder Gemma: Improving the Quality-Efficiency Trade-Off via Adaptation (T5Gemma)**
Biao Zhang, Fedor Moiseev, Joshua Ainslie, et al. (Google DeepMind). arXiv, April 2025.
<https://arxiv.org/abs/2504.06225> · Blog: <https://developers.googleblog.com/en/t5gemma/>
The authors change pretrained decoder-only Gemma 2 models (2B and 9B) into encoder-decoder models. At a similar inference cost, the encoder-decoder models give much better results after fine-tuning. For example, Gemma 2B-2B is about 7% better than Gemma 2B after instruction tuning. This gives a modern, larger option for the original Section 2 (encoder-decoder). It also makes 4-bit QLoRA on an encoder-decoder model more useful, because the models are larger than FLAN-T5-base.

**T5Gemma 2: Seeing, Reading, and Understanding Longer**
Biao Zhang, Paul Suganthan, Gaël Liu, et al. (Google DeepMind). Technical report, arXiv, December 2025.
<https://arxiv.org/abs/2512.14856>
The same adaptation method applied to Gemma 3. The models accept text and images in the encoder, and support long context. Checkpoints are 270M-270M, 1B-1B, and 4B-4B. The encoder and decoder share the word embeddings, and the decoder self-attention and cross-attention are merged into one module. Because of this merge, check the module names before you set `target_modules`.

## New embedding models and tools (the "Embeddings for search" section)

**EmbeddingGemma: Powerful and Lightweight Text Representations**
Henrique Schechter Vera, Sahil Dua, Biao Zhang, et al. (Google DeepMind). arXiv, September 2025.
<https://arxiv.org/abs/2509.20354> · Blog: <https://huggingface.co/blog/embeddinggemma>
A 300M embedding model built from Gemma 3, with an encoder-decoder initialization. It gave state-of-the-art MTEB results for models below 500M parameters at release. It is a strong base model for the `sentence-transformers` fine-tuning that the original guide describes.

**Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models**
Yanzhao Zhang, Mingxin Li, Dingkun Long, et al. (Alibaba Qwen team). arXiv, June 2025.
<https://arxiv.org/abs/2506.05176>
Embedding and reranker models built on decoder-only Qwen3 LLMs (0.6B, 4B, 8B). This shows that a decoder-only model can be a strong embedding model after contrastive training. It changes the simple "search → encoder-only" rule in the original "Which family to choose" table (see below).

**Sentence Transformers v5 and v6**
UKP Lab and Hugging Face. v5.0: July 2025. v6.0: **[2026]** August 2026.
Release notes: <https://github.com/UKPLab/sentence-transformers/releases> · Sparse encoder training blog: <https://huggingface.co/blog/train-sparse-encoder>
v5 adds `SparseEncoder` models (SPLADE and similar) with their own losses and evaluators. v6 adds multi-vector (ColBERT-style, late interaction) models as a fourth model type, next to `SentenceTransformer`, `CrossEncoder`, and `SparseEncoder`. v6 requires `transformers` v5 and has breaking changes. Read the migration guide (<https://sbert.net/docs/migration_guide.html>) before you upgrade old training code.

## New tool versions

**Transformers v5**
Hugging Face. **[2026]** v5.0.0 released 26 January 2026. Weekly minor releases since then (v5.19 in October 2026).
Blog: <https://huggingface.co/blog/transformers-v5> · Migration guide: <https://github.com/huggingface/transformers/blob/main/MIGRATION_GUIDE_V5.md>
The first major release in five years. TensorFlow and JAX support is removed. A new weight-loading API can fuse or split layers when it loads a checkpoint. The `load_in_4bit` and `load_in_8bit` arguments to `from_pretrained` are removed; use `quantization_config=BitsAndBytesConfig(...)`, as the original guide already does. For seq2seq data, `prepare_seq2seq_batch()` is removed; use `tokenizer(..., text_target=...)`.

**PEFT v0.15 to v0.21**
Hugging Face. March 2025 to **[2026]** September 2026.
Releases: <https://github.com/huggingface/peft/releases> · LoRA guide: <https://huggingface.co/docs/peft/main/en/developer_guides/lora>
These releases add many new adapter methods. The changes most relevant to the original guide:
- v0.17 (August 2025): `target_parameters` applies LoRA to an `nn.Parameter` directly. This is necessary for MoE models that store all experts in one parameter.
- v0.20 (**[2026]** July 2026): `find_kappa_target_modules` suggests target modules from the condition number of the base weights.
- v0.20: generic quantization support for more adapter types, and deprecation of AutoGPTQ.

---

## Changes to earlier advice

1. **Claim: "Search or semantic similarity → Encoder-only, with `sentence-transformers`."**
   This is still a good default for small, fast models (mmBERT, EmbeddingGemma). But decoder-based embedding models (Qwen3 Embedding) now lead many benchmarks at 0.6B–8B. Choose by latency and cost, not only by architecture. Also, `sentence-transformers` now trains sparse and multi-vector models, not only dense ones.

2. **Claim: "A 400M encoder often matches or beats a 7B decoder on a fixed classification task."**
   Ettin (ICLR 2026) gives controlled evidence for this at small sizes: a 400M encoder beats a 1B decoder trained with the same data. The 7B comparison is not tested in that paper. Keep the advice, but cite Ettin for the controlled result.

3. **Claim: "Only decoder-only models are commonly large enough for [4-bit quantization] to matter."**
   T5Gemma (up to 9B-9B) and T5Gemma 2 (up to 4B-4B) are modern encoder-decoder models of this size. QLoRA on an encoder-decoder model is now a practical option, not only for `flan-t5-xl` and `flan-t5-xxl`.

4. **Claim: the target-module code that lists `torch.nn.Linear` layers finds all trainable weights.**
   In Transformers v5, many MoE models store the experts as fused parameters, not as a list of `nn.Linear` layers. The listing code does not show these weights. Use PEFT `target_parameters` for the expert weights, and check `print_trainable_parameters()` after `get_peft_model`.

5. **Claim: the `BitsAndBytesConfig` code and `load_in_4bit=True`.**
   The code in the original guide is correct for Transformers v5, because it passes `quantization_config`. Older tutorials that pass `load_in_4bit=True` directly to `from_pretrained` fail on v5.
