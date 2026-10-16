Here are reliable sources, in a suggested reading order.

## Basics of floating-point numbers

- **David Goldberg, "What Every Computer Scientist Should Know About Floating-Point Arithmetic"** (ACM Computing Surveys, 1991). This is the standard introduction to precision, rounding, and range. It does not discuss ML, but it explains why small updates are lost in low-precision formats. https://doi.org/10.1145/103162.103163

## Mixed precision training (float16 and float32)

- **Micikevicius et al., "Mixed Precision Training"** (ICLR 2018, NVIDIA and Baidu). This is the main paper on the topic. It introduces three methods: a float32 master copy of the weights, loss scaling, and float32 accumulation. https://arxiv.org/abs/1710.03740
- **NVIDIA, "Train With Mixed Precision"** (user guide). This is a practical guide to the same methods, with advice for each framework. https://docs.nvidia.com/deeplearning/performance/mixed-precision-training/index.html

## bfloat16

- **Kalamkar et al., "A Study of BFLOAT16 for Deep Learning Training"** (Intel and Facebook, 2019). It compares bfloat16 with float32 on many models. https://arxiv.org/abs/1905.12322
- **Zamirai et al., "Revisiting BFloat16 Training"** (2020). This paper is directly related to your problem. It shows that pure bfloat16 training loses small weight updates because of rounding. It also examines stochastic rounding and Kahan summation as solutions. https://arxiv.org/abs/2010.06192
- **Google Cloud blog, "BFloat16: The secret to high performance on Cloud TPUs"** (2019). It gives a short explanation of why Google designed the format, with diagrams of the bit layouts. https://cloud.google.com/blog/products/ai-machine-learning/bfloat16-the-secret-to-high-performance-on-cloud-tpus

## Newer, lower-precision formats

- **Micikevicius et al., "FP8 Formats for Deep Learning"** (2022). It defines the two 8-bit formats (E4M3 and E5M2) that are used in new GPUs. https://arxiv.org/abs/2209.05433
- **Dettmers et al., "LLM.int8()"** (2022). It discusses 8-bit integer inference for large models. https://arxiv.org/abs/2208.07339
- **Dettmers et al., "QLoRA"** (2023). It combines LoRA with 4-bit NF4 weights. It is related to your PEFT work. https://arxiv.org/abs/2305.14314

## Framework documentation

- **PyTorch, "Automatic Mixed Precision"** (`torch.amp`). It explains which operations run in low precision and which stay in float32. https://pytorch.org/docs/stable/amp.html
- **Hugging Face, "Methods and tools for efficient training on a single GPU"**. It explains the `fp16`, `bf16`, and `tf32` options in `TrainingArguments`, and their effect on memory and speed. https://huggingface.co/docs/transformers/perf_train_gpu_one

For your case, I recommend that you read the Micikevicius 2018 paper first, and then the Zamirai 2020 paper.