# Addendum (2025–2026): Floating-point data types and mixed precision for deep learning

**Created/updated:** 2026-10-10

This addendum extends `datatypes.md`. That file lists sources on floating-point basics, mixed precision (float16 and float32), bfloat16, FP8, INT8, and NF4. This addendum adds work from 2025 and 2026 on the same topics. The check date is 2026-10-10. Items from 2026 have the label **[2026]**.

## New block-scaled formats (MXFP8, MXFP4, NVFP4)

The original file stops at the 2022 FP8 formats. New GPUs (NVIDIA Blackwell) support "microscaling" (MX) formats. In these formats, a small block of values (for example, 32 values) shares one scale factor. This decreases the error from outliers, and it lets more tensors use 8-bit or 4-bit values.

- **Recipes for Pre-training LLMs with MXFP8.** Mishra, Stosic, Layton et al. (NVIDIA). arXiv, May 2025. https://arxiv.org/abs/2506.08027
  This report shows how to configure MXFP8 (E4M3) for pretraining. With a specific number conversion method, training matches bfloat16 for models up to 8B parameters and up to 15T tokens. It is the next step after the FP8 formats paper (Micikevicius et al., 2022) in the original file.

- **Pretraining Large Language Models with NVFP4.** NVIDIA (Abecassis et al.). arXiv, Sep 2025 (revised 2026). https://arxiv.org/abs/2509.25149
  NVIDIA trains a 12B model on 10T tokens with the 4-bit NVFP4 format. The loss and task accuracy are close to an FP8 baseline. The method uses random Hadamard transforms, two-dimensional quantization, stochastic rounding, and some layers in higher precision.

- **Training LLMs with MXFP4.** Tseng, Yu, Park et al. AISTATS 2025. https://arxiv.org/abs/2502.20586
  This paper gives a near-lossless recipe that uses MXFP4 matrix multiplications in the backward pass. It uses stochastic rounding to get unbiased gradients and a random Hadamard transform to limit the variance from block outliers. Models up to 6.7B parameters show small loss compared with bfloat16 mixed precision.

- **Quartet: Native FP4 Training Can Be Optimal for Large Language Models.** Castro, Panferov, Tabesh, ..., Alistarh (IST Austria). NeurIPS 2025. https://arxiv.org/abs/2505.14669
  Quartet runs all linear-layer matrix multiplications in MXFP4. The authors give GPU kernels that are almost 2x faster than FP8 for linear layers on Blackwell. Code: https://github.com/IST-DASLab/Quartet

- **[2026] Quartet II: Accurate LLM Pre-Training in NVFP4 by Improved Unbiased Gradient Estimation.** Panferov, Schultheis, Tabesh, Alistarh (IST Austria). arXiv, Jan 2026. https://arxiv.org/abs/2601.22813
  This paper replaces plain stochastic rounding with a new rounding method (MS-EDEN) that has more than 2x lower quantization error. It trains models up to 1.9B parameters fully in NVFP4 for linear layers. It reports up to 4.2x speedup compared with bfloat16 on Blackwell GPUs.

## Stochastic rounding and bfloat16

- **Stochastic Rounding for LLM Training: Theory and Practice.** Ozkara, Yu, Park et al. AISTATS 2025. https://arxiv.org/abs/2502.20566
  This paper continues the work of Zamirai et al. (2020) in the original file. It gives a convergence analysis for Adam with stochastic rounding. In pretraining runs up to 6.7B parameters, pure bfloat16 with stochastic rounding gives better perplexity than (bfloat16, float32) mixed precision. It also gives up to 1.54x more throughput and 30% less memory.

## Precision in reinforcement learning and inference

- **Defeating the Training-Inference Mismatch via FP16.** Qi, Liu, Zhou, Pang, Du, Lee, Lin (Sea AI Lab and National University of Singapore). arXiv, Oct 2025. https://arxiv.org/abs/2510.26788
  In RL fine-tuning of LLMs, the inference engine and the training engine give slightly different probabilities. This paper shows that bfloat16 rounding is a main cause of this mismatch. A change to float16 (with loss scaling) decreases the mismatch and makes RL training more stable. The change needs only a few lines of code.

- **Defeating Nondeterminism in LLM Inference.** Horace He and others, Thinking Machines Lab. Blog post, Sep 2025. https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/
  Floating-point addition is not associative, so the order of operations changes the result. The post shows that the main cause of different outputs at temperature 0 is kernels that are not "batch invariant". The result of a request changes with the batch size of the server. The post gives batch-invariant kernels as a solution.

## Low-precision weights in released models

- **gpt-oss-120b & gpt-oss-20b Model Card.** OpenAI. arXiv, Aug 2025. https://arxiv.org/abs/2508.10925
  OpenAI releases open-weight mixture-of-experts models with the expert weights in MXFP4. Because of this, the 120B model can run on one 80 GB GPU. This is an example of a 4-bit block-scaled format in a widely used released model. Compare with NF4 in QLoRA (original file).

## Framework documentation and tools

- **TorchAO: PyTorch-Native Training-to-Serving Model Optimization.** Or, Jain, Vega-Myhre et al. (PyTorch team, Meta). CODEML workshop at ICML 2025, Jul 2025. https://arxiv.org/abs/2507.16099 . Code: https://github.com/pytorch/ao
  TorchAO is the PyTorch library for FP8 training, quantization-aware training, and post-training quantization. It supports INT4, INT8, FP8, MXFP4, MXFP6, and MXFP8. It connects to TorchTitan, Axolotl, Hugging Face, vLLM, and SGLang. Use it after `torch.amp` when you need formats lower than bfloat16.

## Changes to earlier advice

- **Claim in the original:** Pure bfloat16 training loses small weight updates, so use a float32 master copy (Micikevicius 2018, Zamirai 2020).
  **Update:** This is still correct for round-to-nearest. But Ozkara et al. (2025) show that bfloat16 with stochastic rounding can match or beat float32 master weights at 6.7B scale. Stochastic rounding is now a standard part of FP4 training recipes (Tseng 2025, NVIDIA NVFP4 2025).

- **Claim in the original:** bfloat16 is the usual choice because it has the same range as float32 and needs no loss scaling.
  **Update:** For RL fine-tuning of LLMs, Qi et al. (Oct 2025) report that float16 gives more stable training than bfloat16, because float16 has more mantissa bits. This is a preprint result. Test it on your own workload before you change the default.

- **Claim in the original:** FP8 (E4M3 and E5M2) is the "newer, lower-precision" format.
  **Update:** In 2025–2026, the newest formats are block-scaled formats: MXFP8, MXFP4, and NVFP4. FP8 pretraining is now common, and 4-bit pretraining has been shown at 12B parameters and 10T tokens (NVIDIA, 2025).

- **Link change:** The PyTorch AMP page now redirects to https://docs.pytorch.org/docs/stable/amp.html .
