# Credits

This board stands on other people's work. Several columns exist only because
somebody else published a working recipe for this hardware first. Where a
number depends on someone else's build, method or patch, it is credited here
and in the relevant `configs/` file.

If your work is used here and credited wrongly or not at all, **open an issue
and it will be fixed.**

## Recipes and kits adopted

- **[MiaAI-Lab](https://github.com/MiaAI-Lab)** DGX Spark recipes. The
  DeepSeek-V4-Flash single-Spark column is served through their `sparkinfer`
  EXL3 recipe; their GLM-5.3-Flash EXL3 kit and their TP=2 vLLM recipe are
  used on the dual-Spark board. Adopted substantially as published.
- **z-lab** DFlash speculative-decoding build, used by the 122B column.
- **tonyd2wild** `sm121` vLLM builds, used on the dual-Spark board.

## Checkpoints and quantisations

- **QUASAR-QAT**, quantisation-aware-training NVFP4: Together AI and Cornell,
  arXiv **2608.13966**. Checkpoint published by its authors; this repository
  benchmarks it and does not redistribute it.
- **RadixArk** NVFP4 builds.
- **Unsloth** 27B NVFP4 build.
- **AI2** for olmOCR.
- **maurienne-ai** for the calibrated NVFP4 DFlash2 draft model.
- Base models from **Qwen**, **Z.ai (GLM)**, **DeepSeek**, **Google (Gemma)**,
  **NVIDIA (Nemotron)**, **InclusionAI (Ling)** and others, each under its own
  upstream license.

**No model weights are redistributed in this repository.** Every checkpoint is
downloaded from its original publisher.

## Serving stacks

- **vLLM** and **SGLang**, including the community work that got both running
  on GB10 / sm_121.
- **llama.cpp** for the GGUF columns.
- **ExLlamaV3 / EXL3** for the EXL3 columns.
- **FlashInfer** kernels, including the NVFP4 linear kernels the 4-bit builds
  depend on.
- **NVIDIA TensorRT Model Optimizer (ModelOpt)** for post-training
  quantisation recipes.

## Hardware

- NVIDIA DGX Spark (GB10, 128 GB unified memory).
