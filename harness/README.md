# Harness

The runnable benchmark code: the battery that produces a board column, plus
the two purpose-built instruments.

## Layout

```
harness/
  battery.py     runs the full battery against one served model
  s-bench/       paired-binary scoring for models that are close together
  a-bench/       the agent suite
```

Methodology for both instruments is in
[`../methodology/`](../methodology/).

## Running the battery

```bash
python harness/battery.py \
  --base-url "http://localhost:${PORT}" \
  --model <served-model-name> \
  --out results/raw/
```

The harness talks to an OpenAI-compatible endpoint, so it does not care which
engine is behind it. That is deliberate: the board serves different models on
different engines, and the measuring instrument has to be identical across all
of them.

## Two rules the instruments enforce

1. **Harness artifacts are not model failures.** When the rig itself fails an
   item, that is counted and reported separately. Collapsing the two makes a
   score look worse than the model is and hides rig bugs.
2. **The same input goes to both sides of any comparison.** Never substitute
   alternative test material for one arm of a paired comparison, however
   equivalent it looks.

## Determinism

Determinism flags belong in scoring runs, never in serving configs. On the
builds used here, enabling deterministic inference makes the server fall over
on ordinary long prompts. See [`../GOTCHAS.md`](../GOTCHAS.md) item 1.

Byte-level non-determinism at temperature 0 is normal on this stack and is
tolerated. *Outcome* non-determinism is not, and the canary items in `a-bench`
are how the difference is established rather than assumed.
