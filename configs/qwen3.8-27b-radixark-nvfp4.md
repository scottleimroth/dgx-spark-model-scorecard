# Qwen3.8-27B, RadixArk NVFP4 build

The general-purpose column: a 4-bit NVFP4 build of Qwen3.8-27B served with a
DFlash2 speculative drafter, configured to leave room for other work on the
box.

## Upstream

| | |
|---|---|
| Base model | `Qwen/Qwen3.8-27B` (upstream license applies) |
| Quantisation | NVFP4 W4A4 group 16 on MLP projections and `lm_head`; FP8 on attention and linear-attention projections; FP8 KV cache; MTP and vision tensors left BF16 |
| Drafter | DFlash2 NVFP4 draft model, calibrated build |
| Engine | SGLang, pinned build for GB10 / sm_121 |

Weights are not distributed here. Download the checkpoint and drafter from
their upstream publishers.

## Serve

```bash
# Fabric, paths and ports below are EXAMPLES. Set them for your machine.
MODEL_DIR=/opt/models/qwen38-27b-nvfp4
DRAFT_DIR=/opt/models/dflash2-draft-nvfp4
IMAGE=your-registry/sglang-gb10:pinned-tag
PORT=8080          # example only; use whatever your setup expects

docker run --rm --gpus all \
  -v "$MODEL_DIR:$MODEL_DIR:ro" \
  -v "$DRAFT_DIR:$DRAFT_DIR:ro" \
  -p "$PORT:$PORT" \
  "$IMAGE" \
  python3 -m sglang.launch_server \
    --model-path "$MODEL_DIR" \
    --port "$PORT" \
    --context-length 131072 \
    --mem-fraction-static 0.43 \
    --kv-cache-dtype fp8_e4m3 \
    --attention-backend flashinfer \
    --mamba-ssm-dtype bfloat16 \
    --speculative-algorithm DFLASH \
    --speculative-draft-model-path "$DRAFT_DIR" \
    --speculative-num-draft-tokens 8 \
    --reasoning-parser qwen3 \
    --tool-call-parser qwen3_coder
```

## Why this is its best config

- **`--mem-fraction-static 0.43`.** Not a performance choice. It leaves roughly
  57 GB of the unified pool free for co-resident work (image generation and
  speech, in the setup these numbers came from). The model pre-claims at launch,
  so a memory collision fails the launch loudly instead of failing a live
  request later. Raise it if the box is dedicated to the LLM.
- **`--speculative-num-draft-tokens 8`.** This is DFlash2's block size, not a
  tunable. Measured across budgets 6, 8 and 10: accepted length is worse at 6,
  plateaus at 8, and gains nothing at 10. Compare budgets by accepted *length*,
  never by the engine's reported acceptance *rate*, which falls arithmetically
  as the budget rises. See [`../GOTCHAS.md`](../GOTCHAS.md) item 3.
- **`--context-length 131072`.** The configured ceiling for this column. An
  `a-ctx` item over this limit is refused with a clean HTTP 400, which the
  scorer records as a configuration limit rather than a wrong answer.
- **No `--enable-deterministic-inference`.** Deliberately absent. On this build
  that flag caps FlashInfer's prefill workspace at 2 GiB and uses fixed
  4096-token KV splits, so any fresh prompt of about 7,300 tokens or more takes
  down the entire server. Enable it for scoring runs only. See
  [`../GOTCHAS.md`](../GOTCHAS.md) item 1.

## Preflight

Docker silently creates a missing bind-mount source as an empty directory, and
the serve then dies late with a confusing error. Check before launching:

```bash
test -s "$MODEL_DIR/config.json"          || { echo "no model"; exit 1; }
test -s "$DRAFT_DIR/model.safetensors"    || { echo "no drafter"; exit 1; }
docker image inspect "$IMAGE" >/dev/null  || { echo "no image"; exit 1; }
```
