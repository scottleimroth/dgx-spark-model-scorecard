# Single-Spark scorecard, September 2026

Every model served in isolation on **one NVIDIA DGX Spark** (GB10, 128 GB
unified memory), each at its own best configuration.

**A score is a standing from 0 to 10 within its row only.** Rows are never
summed into a column total. Read across a row to compare models; reading down
a column tells you nothing.

Live interactive version, with per-cell configuration popups:
<https://scottleimroth.com/ai-technology/spark-benchmarks>

Methodology: [`../methodology/`](../methodology/). Serving configs:
[`../configs/`](../configs/).

---

## Agent tasks

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Apodex-1.1-mini | 10 | 55/60 - 60 items - 0 LOST | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| DeepSeek-V4-Flash | 10 | 55/60 - 60 items - 0 LOST | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| RadixArk FP4 27B | 10 | 55/60 - 60 items - 0 LOST | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen 27B apostate | 9 | 53/60 - 60 items - 2 lost | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Unsloth 27B (vLLM) | 9 | 169/191 - 88.5% - 191 items | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| QUASAR-QAT 27B | 8.5 | 52/60 - 60 items - 2 lost | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.6-35B-A3B | 8.5 | 52/60 - 60 items - 2 lost | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| GLM-4.7-Flash | 8 | 50/60 - 60 items - 0 lost | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Nemotron-Omni-30B | 8 | 51/60 - 60 items - 4 lost | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Qwen 27B orcarouter | 8 | 51/60 - 60 items - 2 lost | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 8 | 51/60 - 60 items - 4 lost | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Gemma-4-26B-A4B | 7.5 | 50/60 - 60 items - 2 lost | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| GLM-5.3-Flash EXL3 (K2) | 6.5 | 50/60 - 60 items - 0 LOST (6 a-ctx/a-turn items exceed the 8k ctx: clean 400s) | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Ling-3.0-Flash INT4 | 6.5 | 49/58 scoreable - 60 items - 2 harness-artifact, 0 wrong-answer excess | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen3.8-Flash-Next | 6.5 | 48/60 - 60 items - 8 LOST | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Qwen3.8-27B BF16 base | 5.5 | 43/60 - 60 items - 2 lost | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| RadixArk BF16-head | 5.5 | 46/60 - 60 items - 9 LOST | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| Qwen3.5-122B | 4 | 36/60 - 60 items - 23 LOST | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| olmOCR-2-7B | 0.5 | cannot call tools - 60 items attempted 0/0 | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |

## Tools and structured output

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Qwen3.5-122B | 10 | 24/24 - 24 items - PERFECT | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Apodex-1.1-mini | 9.5 | 23/24 - 24 items - a-tool 11/12, a-schema 12/12 | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| DeepSeek-V4-Flash | 9.5 | 23/24 - 24 items | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| GLM-5.3-Flash EXL3 (K2) | 9.5 | 23/24 - 24 items - a-tool 12/12 PERFECT, a-schema 11/12 | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Nemotron-Omni-30B | 9.5 | 23/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Qwen 27B apostate | 9.5 | 23/24 - 24 items | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen 27B orcarouter | 9.5 | 23/24 - 24 items | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen3.6-35B-A3B | 9.5 | 23/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| RadixArk BF16-head | 9.5 | 23/24 - 24 items | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 9.5 | 23/24 - 24 items | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 9.5 | 23/24 - 24 items | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (vLLM) | 9.5 | 127/138 - 92.0% - 191-item suite | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| QUASAR-QAT 27B | 9 | 22/24 - 24 items - a-tool 10/12, a-schema 12/12 | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.8-Flash-Next | 9 | 22/24 - 24 items | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Gemma-4-26B-A4B | 8.5 | 21/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Ling-3.0-Flash INT4 | 8 | 21/24 - 24 items - a-tool 11/12, a-schema 10/12 | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen3.8-27B BF16 base | 8 | 20/24 - 24 items - a-tool 11/12, a-schema 9/12 | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| GLM-4.7-Flash | 7.5 | 19/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| olmOCR-2-7B | 0 | no tool calls even with --jinja - 24 items attempted 0/0 | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |

## Multi-turn and sequencing

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Gemma-4-26B-A4B | 10 | 22/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen3.8-Flash-Next | 10 | 22/24 - 24 items | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Apodex-1.1-mini | 8.5 | 20/24 - 24 items - a-turn 9/12, a-seq 11/12 | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| DeepSeek-V4-Flash | 8.5 | 20/24 - 24 items | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| GLM-4.7-Flash | 8.5 | 20/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| QUASAR-QAT 27B | 8.5 | 20/24 - 24 items - a-turn 10/12, a-seq 10/12 | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen 27B apostate | 8.5 | 20/24 - 24 items | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| RadixArk BF16-head | 8.5 | 20/24 - 24 items | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 8.5 | 20/24 - 24 items | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 8.5 | 20/24 - 24 items | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| GLM-5.3-Flash EXL3 (K2) | 8 | 20/23 scoreable - 24 items - a-turn 9/11, a-seq 11/12 | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Nemotron-Omni-30B | 8 | 19/24 - 24 items - 1 lost | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Qwen3.6-35B-A3B | 8 | 19/24 - 24 items | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Qwen 27B orcarouter | 7.5 | 18/24 - 24 items | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Ling-3.0-Flash INT4 | 7 | 18/24 - 24 items - a-turn 9/12, a-seq 9/12 | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Unsloth 27B (vLLM) | 6.5 | 26/37 - 70.3% - 191-item suite | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Qwen3.8-27B BF16 base | 4.5 | 13/24 - 24 items - a-turn 8/12, a-seq 5/12 | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| Qwen3.5-122B | 4 | 12/24 - 24 items - 11 LOST | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| olmOCR-2-7B | 0.5 | not scoreable without tools - 24 items attempted 0/0 | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |

## Likelihood (bits per character; lower raw is better)

> Raw figures are bits per character, where **lower is better**. The score is flipped so that, as in every other row, higher is better. Measured with speculative decoding off; see [methodology](../methodology/README.md).

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Qwen 27B apostate | 10 | 0.5129 bpc | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen3.8-27B BF16 base | 10 | 0.5115 / 0.5115 bpc - spec off for this cell | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 - SPEC OFF for this cell |
| QUASAR-QAT 27B | 9.5 | 0.5156 / 0.5153 bpc (0.06% twin delta) - spec off for this cell | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa - SPEC OFF for this cell |
| Qwen 27B orcarouter | 9.5 | 0.5184 bpc | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 9.5 | 0.5172 bpc | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 9.5 | 0.5196 bpc | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 9.5 | 0.5173 bpc | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (vLLM) | 9.5 | 0.5206 bpc | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| GLM-5.3-Flash EXL3 (K2) | 8.5 | 0.5493 / 0.5491 bpc (0.03% twin delta) | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Apodex-1.1-mini | 7.5 | 0.5553 / 0.5553 bpc (0.00% twin delta) - spec off for this cell | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a - SPEC OFF for this cell |
| DeepSeek-V4-Flash | 7 | 0.5623 bpc (twin runs 0.013% apart) | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| Ling-3.0-Flash INT4 | 7 | 0.5621 / 0.5627 bpc (0.10% twin delta) | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen3.5-122B | 7 | 0.5670 bpc | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Qwen3.6-35B-A3B | 7 | 0.5729 bpc (corrected) | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn - SPEC OFF for this cell |
| Nemotron-Omni-30B | 5.5 | 0.6358 bpc | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| GLM-4.7-Flash | 4.5 | 0.6796 bpc | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Gemma-4-26B-A4B | 1 | 1.7716 bpc | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen3.8-Flash-Next | 0.5 | CANNOT BE MEASURED on this serving engine - scored 0.5, below all measured, tied | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| olmOCR-2-7B | 0.5 | CANNOT BE MEASURED on this serving engine - scored 0.5, below all measured, tied | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |

## Prose speed

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Qwen3.6-35B-A3B | 10 | 96.1 tok/s | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - standard serving flags, warm |
| Nemotron-Omni-30B | 7 | 59.4 tok/s BARE | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Qwen3.5-122B | 5.5 | 45.6 tok/s (DFlash blk12) | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| RadixArk FP4 27B | 5.5 | 45.0 tok/s (DFlash2 blk8) | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Apodex-1.1-mini | 5 | 42.5 tok/s (MTP-3) | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| Unsloth 27B (SGLang) | 5 | 41.4 tok/s (DFlash2 blk8) | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| olmOCR-2-7B | 5 | 41.6 tok/s | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| GLM-4.7-Flash | 4.5 | 36.8 tok/s | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Ling-3.0-Flash INT4 | 4.5 | 37.3 / 37.5 tok/s (author's band reproduces) | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen 27B orcarouter | 4.5 | 37.7 tok/s (DFlash2 blk8) | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 4.5 | 38.5 tok/s (DFlash2 blk8) | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| DeepSeek-V4-Flash | 4 | 31.9 tok/s (DSpark-5) | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| Gemma-4-26B-A4B | 4 | 33.5 tok/s (ngram k5) | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen3.8-Flash-Next | 3.5 | 27.3 tok/s | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Unsloth 27B (vLLM) | 3.5 | 27.3 tok/s (MTP-5) | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| QUASAR-QAT 27B | 2.5 | 19.3 tok/s (MTP-2) | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| GLM-5.3-Flash EXL3 (K2) | 2 | 15.07 tok/s c=1 (MTP-2) | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Qwen 27B apostate | 0.5 | 5.6 tok/s (ngram k5; 4.4 bare) | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers + ngram k5 |
| Qwen3.8-27B BF16 base | 0.5 | 4.8 tok/s (ngram k5) | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |

## Concurrency

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Qwen3.6-35B-A3B | 10 | 64.3 tok/s per agent at c=2 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Nemotron-Omni-30B | 8 | 51.6 tok/s per agent at c=2 | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| olmOCR-2-7B | 6.5 | 40.9 tok/s per agent at c=2 | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| Apodex-1.1-mini | 6 | 35.7 tok/s per agent at c=2 (71.4 agg); c=4 25.7/stream (102.6 agg) | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| GLM-4.7-Flash | 5.5 | 33.7 tok/s per agent at c=2 | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Ling-3.0-Flash INT4 | 5.5 | 31.32 tok/s per agent at c=2 (62.63 agg); c=8 19.6/stream (156.8 agg) | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| RadixArk FP4 27B | 5 | 28.0 tok/s per agent at c=2 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| DeepSeek-V4-Flash | 4.5 | 26.5 tok/s per agent at c=2 | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| Unsloth 27B (SGLang) | 4.5 | 25.7 tok/s per agent at c=2 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen 27B orcarouter | 4 | 24.2 tok/s per agent at c=2 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 4 | 22.8 tok/s per agent at c=2 | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| QUASAR-QAT 27B | 3.5 | 19.5 tok/s per agent at c=2 (39.0 agg); c=4 19.0/stream (75.9 agg) | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.5-122B | 3.5 | 21.0 tok/s per agent at c=2 | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Qwen3.8-Flash-Next | 3.5 | 19.5 tok/s per agent at c=2 | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Unsloth 27B (vLLM) | 3.5 | 21.0 tok/s per agent AT c=8 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Gemma-4-26B-A4B | 3 | 17.6 tok/s per agent at c=2 | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| GLM-5.3-Flash EXL3 (K2) | 0.5 | 14.48 tok/s per agent at c=2 - the serve caps max-num-seqs at 1 | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Qwen 27B apostate | 0.5 | 4.3 tok/s per agent at c=2 | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen3.8-27B BF16 base | 0.5 | 4.58 tok/s per agent at c=2; c=1 4.87, c=4 4.23/stream (16.9 agg) | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |

## Speculative decoding gain

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| RadixArk FP4 27B | 10 | 12.2 -> 45.0 (3.7x, DFlash2 blk8) | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen3.5-122B | 9.5 | 16.2 -> 45.6 (2.8x, z-lab DFlash blk12) | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Unsloth 27B (SGLang) | 9.5 | 10.9 -> 41.4 (3.8x, DFlash2 blk8) | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen 27B orcarouter | 9 | 9.8 -> 37.7 (3.8x, DFlash2 blk8) | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 9 | 9.8 -> 38.5 (3.9x, DFlash2 blk8) | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| Qwen3.6-35B-A3B | 8.5 | 66.4 -> 96.1 (+45%, MTP-3) | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| GLM-5.3-Flash EXL3 (K2) | 8 | 8.97 -> 15.07 (+68%, MTP-2) | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Unsloth 27B (vLLM) | 8 | 10.9 -> 27.3 tok/s (2.5x, MTP-5) | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Apodex-1.1-mini | 7 | 29.6 -> 47.2 (1.59x, MTP-3) - 76% draft acceptance | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| DeepSeek-V4-Flash | 7 | 31.9 tok/s with DSpark-5 in-recipe | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| Ling-3.0-Flash INT4 | 7 | 21.1 -> 37.3 (1.77x, MTP) - corroborates the author's own 1.8x claim | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| QUASAR-QAT 27B | 7 | 12.3 -> 21.8 (1.77x, MTP-2) - 82% draft acceptance | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Gemma-4-26B-A4B | 6 | 28.9 -> 33.5 (+16%, ngram k5) | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen 27B apostate | 3 | 4.4 -> 5.6 (+27%, ngram k5) | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers + ngram k5 |
| Qwen3.8-27B BF16 base | 3 | 4.4 -> 5.6 (+27%, ngram k5) - 13.6% draft acceptance | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| Qwen3.8-Flash-Next | 2 | ngram-mod at defaults: no effect (27.1 vs 27.3) | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| GLM-4.7-Flash | 1.5 | no measured gain - spec arms produced nothing | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Nemotron-Omni-30B | 1.5 | every ngram depth SLOWER (59.4 bare; 55.2/46.7/44.6 at k3/5/8) | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| olmOCR-2-7B | 1 | no drafter exists for a 7B OCR model | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |

## Reproducibility at temperature 0

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Unsloth 27B (vLLM) | 10 | 0.000000 bpc spread, 4/4 loads | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Qwen 27B apostate | 9.5 | within-session identical | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen 27B orcarouter | 9.5 | within-session identical | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen3.5-122B | 9.5 | within-session identical | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Qwen3.8-27B BF16 base | 9.5 | within-session identical (5/5 on all three probes) | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| Qwen3.8-Flash-Next | 9.5 | within-session identical | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| RadixArk BF16-head | 9.5 | within-session identical | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 9.5 | within-session identical | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 9.5 | within-session identical | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Apodex-1.1-mini | 6 | byte-nondeterministic at temp 0 - outcome flips 0/16; battery probes 5/5 identical | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| DeepSeek-V4-Flash | 6 | byte-nondeterministic at temp 0 | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| GLM-4.7-Flash | 6 | byte-nondeterministic at temp 0 | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| GLM-5.3-Flash EXL3 (K2) | 6 | byte-nondeterministic at temp 0 (long generations 5/5 distinct) | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Gemma-4-26B-A4B | 6 | byte-nondeterministic at temp 0 | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Ling-3.0-Flash INT4 | 6 | byte-nondeterministic at temp 0 | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Nemotron-Omni-30B | 6 | byte-nondeterministic at temp 0 | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| QUASAR-QAT 27B | 6 | byte-nondeterministic at temp 0 - outcome flips 0/16; battery probes 5/5 identical | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.6-35B-A3B | 6 | byte-nondeterministic at temp 0 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| olmOCR-2-7B | 6 | byte-nondeterministic at temp 0 | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |

## Context ceiling

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Nemotron-Omni-30B | 10 | KV pool 1,902,416 tokens | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Gemma-4-26B-A4B | 9 | KV pool 1,351,800 tokens | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen3.6-35B-A3B | 9 | KV pool 1,303,911 tokens | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Unsloth 27B (vLLM) | 7.5 | 670,797 tokens @ mf 0.60 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Qwen 27B orcarouter | 7 | ctx 262,144 configured | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 7 | ctx 262,144 configured | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 7 | ctx 262,144 configured | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 7 | ctx 262,144 configured | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Apodex-1.1-mini | 6.5 | KV pool 1,054,625 tokens at ctx 262,144 | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| Ling-3.0-Flash INT4 | 6.5 | KV pool 1,090,054 tokens | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen 27B apostate | 6.5 | KV pool 561,199 tokens | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| GLM-4.7-Flash | 6 | KV pool 470,640 tokens | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Qwen3.8-27B BF16 base | 6 | KV pool 479,637 tokens at ctx 131,072 | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| DeepSeek-V4-Flash | 5.5 | KV pool 420,562 tokens | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| Qwen3.5-122B | 5 | ctx 131,072 configured | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| QUASAR-QAT 27B | 4.5 | KV pool 271,825 tokens at ctx 131,072 | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.8-Flash-Next | 3 | 131,072 total / 32,768 per slot | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| olmOCR-2-7B | 1 | 8,192 fixed | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| GLM-5.3-Flash EXL3 (K2) | 0.5 | KV pool 59,684 tokens - 8,192 per request | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |

## Memory footprint

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| olmOCR-2-7B | 10 | 6.3 GiB resident | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| Nemotron-Omni-30B | 9 | 31.6 GiB resident | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Gemma-4-26B-A4B | 7.5 | 44.7 GiB resident | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| QUASAR-QAT 27B | 7.5 | 43.3 GiB resident @ util 0.37 | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.6-35B-A3B | 7.5 | 45.4 GiB resident @ util 0.37 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| GLM-4.7-Flash | 7 | 47.4 GiB resident | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Unsloth 27B (vLLM) | 6 | 57.0 GiB resident | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Qwen3.8-Flash-Next | 5.5 | 67.6 GiB resident (94 GB weights mmap'd) | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Qwen 27B orcarouter | 4 | 84.9 GiB resident @ mf 0.72 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 4 | 85.0 GiB resident @ mf 0.72 | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 4 | 85.0 GiB resident @ mf 0.72 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 4 | 84.7 GiB resident @ mf 0.72 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Apodex-1.1-mini | 3.5 | 96.5 GiB resident @ util 0.80 (BF16) | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| Qwen 27B apostate | 3.5 | 90.0 GiB resident (F16) | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen3.8-27B BF16 base | 3.5 | 89.5 GiB resident @ util 0.75 (BF16) | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| Ling-3.0-Flash INT4 | 3 | MemAvailable 27.14 GiB free, loaded and idle - roughly 94.5 GiB resident | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen3.5-122B | 3 | 100.1 GiB resident @ mf 0.80 | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| DeepSeek-V4-Flash | 2 | 112.5 GiB resident | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| GLM-5.3-Flash EXL3 (K2) | 1.5 | MemAvailable 11.58 GiB free, loaded and idle - roughly 110 GiB resident | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |

## OCR and handwriting read

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Qwen3.8-27B BF16 base | not entered | not entered - no vision tower in this checkpoint |  |
| Qwen3.8-Flash-Next | 10 | 6/6 struck (0 as live) - 97.4 / 87.2 | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| olmOCR-2-7B | 10 | 6/6 struck - 98.0 / 86.5 word acc | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| Gemma-4-26B-A4B | 9.5 | 5/6 struck (0 as live) - 96.7 / 91.9 | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen3.6-35B-A3B | 8 | 4/6 struck (2 as live) - 97.4 / 87.2 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Qwen3.5-122B | 7.5 | 4/6 struck (2 as live) - 93.4 / 87.2 | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Apodex-1.1-mini | 6.5 | 3/6 struck (3 as live) - 96.7 / 87.2 | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| Qwen 27B orcarouter | 6.5 | 3/6 struck (3 as live) - 95.4 / 85.8 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| RadixArk BF16-head | 6.5 | 3/6 struck (3 as live) - 94.0 / 87.2 | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| Unsloth 27B (SGLang) | 6.5 | 3/6 struck (3 as live) - 94.7 / 87.2 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Nemotron-Omni-30B | 5 | 1/6 struck (3 as live) - 64.9 / 83.8 AT BOARDED CONFIG | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| QUASAR-QAT 27B | 5 | 3/6 struck (3 as live) - 66.2 / 85.8 | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| RadixArk FP4 27B | 5 | 3/6 struck (3 as live) - 66.9 / 85.8 | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| GLM-5.3-Flash EXL3 (K2) | 4.5 | 2/6 struck (3 as live, 1 indeterminate) - 67.5 / 86.5 word acc | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Unsloth 27B (vLLM) | 3 | 0/6 struck - 96.0 / 83.1 word acc | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| DeepSeek-V4-Flash | 1 | images rejected (HTTP 400, all pages) | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| GLM-4.7-Flash | 1 | images rejected (HTTP 400, all pages) | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Ling-3.0-Flash INT4 | 1 | no vision in this build | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen 27B apostate | 1 | images rejected (HTTP 400, all pages) | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |

## Vision read (screenshots)

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| DeepSeek-V4-Flash | not entered | not entered - no vision in this build |  |
| GLM-4.7-Flash | not entered | not entered - no vision in this build |  |
| GLM-5.3-Flash EXL3 (K2) | not entered | not entered - no vision in this build |  |
| Ling-3.0-Flash INT4 | not entered | not entered - no vision in this build |  |
| QUASAR-QAT 27B | not entered | not entered - not measured in this round |  |
| Qwen 27B apostate | not entered | not entered - no vision in this build |  |
| Qwen3.8-27B BF16 base | not entered | not entered - no vision tower in this checkpoint |  |
| Qwen3.8-Flash-Next | not entered | not entered - weights not available for this round |  |
| RadixArk BF16-head | not entered | not entered - not measured in this round |  |
| Unsloth 27B (SGLang) | not entered | not entered - not measured in this round |  |
| Qwen3.5-122B | 9.1 | 9.1/10 - 17/19 scoreable, 2 capture-failures | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| RadixArk FP4 27B | 8.5 | 8.5/10 - 17/19 scoreable, 2 capture-failures | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen 27B orcarouter | 8.4 | 8.4/10 - 16/19 scoreable, 3 capture-failures | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (vLLM) | 8.2 | 8.2/10 - 17/19 scoreable, 2 capture-failures | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| olmOCR-2-7B | 8.2 | 8.2/10 - 19/19 scoreable, the only column with zero capture failures | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| Apodex-1.1-mini | 6.6 | 6.6/10 - 16/19 scoreable, 3 capture-failures | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| Qwen3.6-35B-A3B | 6.6 | 6.6/10 - 16/19 scoreable, 3 capture-failures | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Nemotron-Omni-30B | 6.5 | 6.5/10 - 17/19 scoreable, 2 capture-failures | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| Gemma-4-26B-A4B | 5.5 | 5.5/10 - 19/19 scoreable | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |

## Long-prompt latency

> **Prompt sizes here are CHARACTERS, not tokens.** 25k characters is roughly 6k tokens and 95k characters roughly 24k tokens, each with a needle question at the end, timed to a correct answer. A true 95k-*token* prompt is a separate and much larger test, not this row.

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Apodex-1.1-mini | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| DeepSeek-V4-Flash | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| GLM-4.7-Flash | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| Gemma-4-26B-A4B | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Ling-3.0-Flash INT4 | 9.5 | 25k chars ANSWERED (1.9 s) - 95k chars ANSWERED (6.9 s) | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Nemotron-Omni-30B | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| QUASAR-QAT 27B | 9.5 | 25k chars ANSWERED (2.6 s) - 95k chars ANSWERED (8.8 s) | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen 27B apostate | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen 27B orcarouter | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen3.5-122B | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Qwen3.6-35B-A3B | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Qwen3.8-27B BF16 base | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| Qwen3.8-Flash-Next | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| RadixArk BF16-head | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (SGLang) | 9.5 | 25k chars ANSWERED - 95k chars ANSWERED | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (vLLM) | 8 | ok - was fatal until a flag was removed | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| olmOCR-2-7B | 7 | 25k chars ANSWERED - 95k chars clean HTTP 400 | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| GLM-5.3-Flash EXL3 (K2) | 6.5 | 25k chars ANSWERED (10.8 s) - 95k chars clean HTTP-400 naming the 8k chars limit | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |

## Continuity / drop-in fit

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Qwen3.6-35B-A3B | 10 | adopted here as the general-purpose model | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| olmOCR-2-7B | 10 | added beside, no conflict | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| Qwen 27B orcarouter | 8 | drop-in, same arch | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Nemotron-Omni-30B | 7 | vLLM drop-in; audio and vision in one model could replace a separate speech service | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| QUASAR-QAT 27B | 7 | vLLM drop-in, same base family as the general-purpose model here | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen3.5-122B | 7 | runs on a single Spark | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |
| Unsloth 27B (SGLang) | 7 | SGLang - engine change, known-good recipe | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Apodex-1.1-mini | 6 | vLLM drop-in, same family as the 35B column; BF16 only | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| RadixArk BF16-head | 6 | SGLang - engine change | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 6 | SGLang - engine change | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (vLLM) | 6 | replaced 2026-08-28 | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| Ling-3.0-Flash INT4 | 5.5 | single Spark - new model, new serving column, author's watchdog supervisor adopted whole | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| GLM-4.7-Flash | 5 | served name + 5 column change | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| GLM-5.3-Flash EXL3 (K2) | 5 | single Spark - new serving column (EXL3), no vision | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Gemma-4-26B-A4B | 4 | served name + engine change | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Qwen3.8-Flash-Next | 4 | new serving column (llama.cpp + GGUF) | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| Qwen 27B apostate | 3 | F16 - not a deployable size | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen3.8-27B BF16 base | 3 | BF16 - not a deployable size; the yardstick column | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| DeepSeek-V4-Flash | 2 | different engine + whole box | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |

## Fine-tune path

| Model | Score | Measurement | Configuration |
|---|---|---|---|
| Unsloth 27B (SGLang) | 8 | proven family path - the reference lineage | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Apodex-1.1-mini | 7 | Qwen3.5-MoE family - Unsloth path applies | vLLM 0.26 - BF16 weights, the only 16-bit MoE column (no four-bit build of this model exists) - MTP-3 - util 0.80 - qwen3_coder + qwen3 parsers - ctx 262144 - pin 4e4f109a |
| QUASAR-QAT 27B | 7 | same family tooling applies (QAT checkpoint; a further training pass is unproven here) | vLLM 0.26 - NVFP4 W4A4 on all 496 linears (arch:sm_120, FlashInferCutlassNvFp4LinearKernel asserted at init) - MTP-2 - util 0.37 - qwen3 parsers - ctx 131072 - pin d8e6fbfa |
| Qwen 27B apostate | 7 | same family as the 27B | vLLM 0.28 - F16 - util 0.75 - qwen3 parsers |
| Qwen 27B orcarouter | 7 | same family as the 27B | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Qwen3.6-35B-A3B | 7 | Qwen3 family - Unsloth path applies | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-3 - util 0.37 - triton_attn |
| Qwen3.8-27B BF16 base | 7 | same family as the 27B - an edit pass was run on this checkpoint | vLLM 0.28 - BF16 weights (the only 16-bit dense-27B column here) - ngram k5 - util 0.75 - qwen3 parsers - ctx 131072 - snapshot 1d4bf0f2 |
| RadixArk BF16-head | 7 | same family tooling applies | SGLang - NVFP4+BF16 head (arch:sm_120) - DFlash2 blk8 - mf 0.72 |
| RadixArk FP4 27B | 7 | same family tooling applies | SGLang - NVFP4 (arch:sm_120) - DFlash2 blk8 - mf 0.72 - fp8 KV |
| Unsloth 27B (vLLM) | 7 | Unsloth QLoRA/RL - proven family path | vLLM 0.26 - NVFP4 (arch:sm_120) - MTP-5 |
| olmOCR-2-7B | 4 | GGUF/llama.cpp path - not the marking lineage | llama.cpp - Q4_K_M GGUF - mmproj - ctx 8192 |
| Gemma-4-26B-A4B | 3 | different family - no local path proven here | vLLM 0.26 - NVFP4 (arch:sm_120) - ngram k5 - util 0.37 - gemma4 parsers |
| Nemotron-Omni-30B | 3 | NeMo tooling upstream; hybrid MoE-Mamba unproven here | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec (bare is its best) - util 0.37 |
| GLM-4.7-Flash | 2 | no local path proven here | vLLM 0.26 - NVFP4 (arch:sm_120) - no spec - util 0.37 - glm47 parsers |
| GLM-5.3-Flash EXL3 (K2) | 2 | no local path proven for this build | vLLM - EXL3 (K2 build) - MTP-2 - ctx 8192 - max-num-seqs 1 - util 0.87 - single Spark |
| Ling-3.0-Flash INT4 | 2 | no local path proven for this build | vLLM - INT4 - MTP - author's watchdog supervisor - single Spark |
| Qwen3.8-Flash-Next | 2 | no local path; GGUF serve, safetensors training upstream | llama.cpp b10689 digest e52c6104 - UD-IQ4_XS GGUF - ctx 131072/4 slots - mmproj BF16 |
| DeepSeek-V4-Flash | 1 | no EXL3 fine-tune path | sparkinfer (MiaAI one-Spark) - EXL3 - DSpark-5 |
| Qwen3.5-122B | 1 | no mature local path for the A10B MoE | SGLang - NVFP4 (arch:sm_120) - z-lab DFlash blk12 - mf 0.80 - max-running 4 |

---

## Not included here

- **Dual-Spark (TP=2) columns.** Two-box tensor-parallel results are a
  separate board; see the site.
- **Some columns are absent from some rows.** A gap means not measured at
  this configuration, not a zero.

