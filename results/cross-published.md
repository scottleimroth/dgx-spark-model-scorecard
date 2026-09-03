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
