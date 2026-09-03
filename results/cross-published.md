# Cross-published numbers, and why they differ

Some of these models also appear on the community leaderboard
**[Localmaxxing](https://www.localmaxxing.com/en)**, submitted from the same
DGX Spark. **The figures there do not match the figures here, and that is
correct.** This page says exactly why, because an unexplained gap between two
sources is worse than either number on its own.

## The rule

A throughput figure is meaningless without its serving configuration. These two
sources deliberately use different ones:

| | This board | Localmaxxing submissions |
|---|---|---|
| Speculative decoding | **On**, where it is that model's best | **Off** |
| Goal | each model at its own ceiling | one comparable method across all hardware |
| Context | the model's configured ceiling | 32k |
| Concurrency | single stream, plus separate concurrency rows | single stream |

Localmaxxing exists to compare *hardware* on a like-for-like method, so its
ranking excludes speculative decoding. This board exists to show each model at
its best, which usually means speculation on. Both are right for their purpose.

## The overlap, reconciled

**Qwen3.6-35B-A3B**, NVFP4, same box:

| Source | Figure | Configuration |
|---|---|---|
| This board (prose speed) | **96.1 tok/s** | vLLM 0.26, NVFP4, **MTP-3 speculation**, warm |
| Localmaxxing | **70.6 tok/s** | vLLM 0.26, NVFP4, **speculation off**, 32k context, single stream |

The ratio is about **1.36x**, and it is the speculative-decoding gain, not a
measurement disagreement. The board's own speculative-decoding row reports that
gain directly, so the two sources agree once the config is on the table.

If you want to compare this hardware against other hardware, use the
Localmaxxing figure. If you want to know what this model does on a Spark when
served properly, use the board figure.

## The 27B NVFP4 builds, reconciled

Three 27B NVFP4 builds are published in both places. Here the board's own
**speculative-decoding row** does the reconciling for you, because it records
the spec-off baseline alongside the gain:

| Model | Board, spec on | Board's spec-off baseline | Localmaxxing (spec off) | Agreement |
|---|---|---|---|---|
| RadixArk NVFP4 | 45.0 tok/s (DFlash2 blk8, 3.7x) | 12.2 | **12.37** | within 1.4% |
| Unsloth NVFP4 | 41.4 tok/s (DFlash2 blk8, 3.8x) | 10.9 | **10.98** | within 0.7% |
| RadixArk NVFP4, BF16 head | 38.5 tok/s (DFlash2 blk8, 3.9x) | 9.8 | **10.78** | **10% apart** |

The first two are a genuine cross-check: two independent measurement sessions,
weeks apart, agreeing to about one percent on the same checkpoint and config.
That is the kind of agreement that makes a number worth quoting.

**The third does not agree, and it is recorded here rather than smoothed over.**
9.8 against 10.78 is roughly 10%, which is far outside the ~1% the other two
managed. The two figures come from different sessions, so the candidates are a
serve-flag difference between them, a warm-versus-cold state difference, or one
of the two simply being the less careful measurement. It has not been chased
down yet. Until it is, treat the BF16-head spec-off figure as the least certain
number in either source.

Publishing the disagreement is the point. A reader who finds it themselves and
no acknowledgement of it has reason to distrust everything else.

## Measured for Localmaxxing but not on this board

These were submitted there and have no column here, so the leaderboard is the
only public record of them from this box:

| Model | Quant | Engine | tok/s (spec off) |
|---|---|---|---|
| gpt-oss-120b | MXFP4 | vLLM | 34.4 |
| KAT-Coder-V2.5 | Q4_K_M GGUF | llama.cpp | 72.0 |
| Qwen3-Next-80B-A3B (Instruct) | NVFP4 | vLLM | 39.0 |

## A note on checkpoint identity

Two of those had no verified upstream repository recorded when they were first
served, only local paths. They were confirmed against the **actual bytes on
disk** before submission: a GGUF by SHA256 and file-size match against the
publisher's LFS blob hash, ruling out two similarly-named candidate repos, and
an NVFP4 build confirmed as the Instruct rather than Thinking variant from the
checkpoint's own metadata. Neither was identified by filename.

This matters more than it sounds. Assuming a checkpoint's identity from its
directory name is a real and common way to publish a number against the wrong
model.
