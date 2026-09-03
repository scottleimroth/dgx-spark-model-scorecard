# s-bench: separating models that are close together

## The problem it solves

Per-item scoring works when models are far apart. It fails when they are
close. Two models a few points apart on a 60-item suite differ by two or three
items, which is well inside the run-to-run noise of the suite itself. You get a
ranking, but it is not a finding, and re-running can reverse it.

This matters here more than usual, because "each model at its own best" tends
to compress the field: once every model is served well, several of them land
close together.

## The approach: paired binary comparisons

Rather than scoring each model's output against a rubric and comparing scores,
s-bench puts **two models' outputs on the same input side by side** and asks
for a single binary judgement of which is better. Aggregating many such
comparisons is far more sensitive than comparing two independently-derived
scores, because the noise that dominates item scoring is shared by both sides
of a pair and cancels.

Key properties:

- **Same input, always.** Both models see the identical, ground-truthed input.
  Never substitute alternative test material for one side of a pair.
- **Binary, not graded.** "Which of these two is better" is a judgement that
  can be made consistently. "Score this out of 10" is not.
- **Direction is tracked.** Presentation order is recorded and balanced, so a
  position preference cannot masquerade as a quality difference.

## What it does not do

s-bench produces a *relative* ordering over the models compared. It does not
produce an absolute quality score, and a paired win rate should never be quoted
as if it were one.

It also cannot rescue a comparison whose two sides were measured under
different conditions. If the two arms differ in more than the one variable you
mean to test, the comparison is meaningless however carefully it is scored.
Check what the baseline actually was before acting on a ratio.

## Layout

```
harness/s-bench/
  dataset.py     item loading
  items/         the ground-truthed inputs
  mine/          candidate generation
  score/         paired scoring
  review/        human adjudication of close pairs
  validate/      instrument checks
```
