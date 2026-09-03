# DGX Spark Model Scorecard

**Twenty local LLMs on a single NVIDIA DGX Spark (GB10, 128 GB unified memory), each served at its own best configuration.** Scores, the exact config behind every number, and the harness that measured them.

Interactive board: **https://scottleimroth.com/ai-technology/spark-benchmarks**
By Scott Leimroth · [@ScottLeimroth](https://x.com/ScottLeimroth)

---

> Most local-LLM comparisons pick one engine and one quantisation and push every
> model through it. That quietly punishes any model that would run better on a
> different stack. This board does the opposite: **every model is served at its
> own best**, so engines, quantisations and speculative-decoding methods differ
> from column to column by design.
>
> Every cell shows a 0-10 standing *within its row only*, the raw measurement,
> and the exact configuration behind it. There is no fixed overall score, because
> adding a speed standing to an OCR standing produces a number that means nothing.
>
> Instead, **you declare what you care about.** On the
> [interactive board](https://scottleimroth.com/ai-technology/spark-benchmarks)
> you rank the dimensions that matter for your workload, and the models re-sort
> by a weighted blend of *your* priorities. A "best model" only exists relative
> to a job, so the board makes you name the job first.

## What's here

| Path | What it is |
|---|---|
| [`results/`](results/) | Dated scorecards and sanitised run logs |
| [`methodology/`](methodology/) | How each row is scored, in full |
| [`configs/`](configs/) | The serving config for every model, one file each |
| [`harness/`](harness/) | The runnable benchmark harness |
| [`patches/`](patches/) | GB10 (sm_121) vLLM / SGLang patches |
| [`GOTCHAS.md`](GOTCHAS.md) | Operational findings that cost real time |

## Dimensions measured

Agentic task completion, tool calls, multi-turn, structured-output validity,
prose speed, concurrency, reproducibility at temperature 0, context ceiling,
memory footprint, OCR / vision read, long-prompt latency, likelihood (bits per
character against the unquantised base), and speculative-decoding gain.

## Two scoring instruments worth a look

- **[`s-bench`](methodology/s-bench.md), for models that are close together.**
  When two models are a few points apart, per-item scoring is too noisy to
  separate them. s-bench uses paired binary comparisons over ground-truthed
  inputs instead.
- **[`a-bench`](methodology/a-bench.md), the agent suite.** Tool calls, schema
  adherence, multi-turn and long-context items, scored on real task completion
  rather than merely parseable output.

## Reproduce a column

> Fabric and paths below are examples. Set them for your own machine.

```bash
# 1. Serve one model at its measured-best config
#    (each configs/*.md carries the exact serve line)
./configs/serve.sh qwen3.8-27b-radixark-nvfp4

# 2. Run the battery against it
python harness/battery.py \
  --base-url "http://localhost:${PORT}" \
  --model qwen3.8-27b \
  --out results/raw/
```

Weights are **not** distributed here. Each file in [`configs/`](configs/) names
the upstream checkpoint and its license.

## Methodology in one paragraph

Every model is served in isolation on one DGX Spark, each at the engine,
quantisation and speculative method that gives it its best result, then run
through the same battery. Scores are within-row standings from 0 to 10, never
summed across rows. Configs are pinned and recorded per cell. Determinism is
enabled only for benchmark scoring and never for serving; see
[`GOTCHAS.md`](GOTCHAS.md) for why that flag is dangerous in production.

## Related

- **[geekom-benchmarks](https://github.com/scottleimroth/geekom-benchmarks)** my
  companion harness for the same questions on AMD Strix Point (Ryzen AI 9 HX 370
  / Radeon 890M via Lemonade and llama.cpp). Different hardware, different
  engines, same discipline.
- **[Blackwellboy/model-serving-minefield](https://github.com/Blackwellboy/model-serving-minefield)**
  a registry of the ways local model serving goes wrong. Worth reading alongside
  [`GOTCHAS.md`](GOTCHAS.md); some of my findings are filed there.
- **[Localmaxxing](https://www.localmaxxing.com/en)** community speed-test
  leaderboard, useful for comparing this hardware against alternatives on an
  identical model and quantisation.

## This repository is derived content

The measurements here are **extracted and rewritten** from a private working
repository, which remains the authoritative source. This repo is regenerated
from it, so corrections belong upstream: open an issue here and the fix is made
at the source and re-exported. Please do not send pull requests that edit the
results tables directly.

## Contributing

Numbers from other DGX Spark boxes are welcome, especially if they disagree with
these. Open an issue with your config, the raw output, and the engine build.

## License

Code and results tables: [MIT](LICENSE). Model weights are not distributed here;
each model keeps its own upstream license. Credits in [`CREDITS.md`](CREDITS.md).
