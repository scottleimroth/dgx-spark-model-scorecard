# a-bench: the agent suite

Agentic capability measured on **task completion**, not on whether the output
parses. A model that emits beautifully-formed JSON containing the wrong answer
scores zero here.

## Item families

| Family | What it tests |
|---|---|
| `a-tool` | Tool calls: correct tool, correct arguments, correct handling of the result |
| `a-seq` | Sequencing: multi-step tasks where order matters |
| `a-schema` | Structured output: adherence to a required schema |
| `a-turn` | Multi-turn: carrying state and correcting course across turns |
| `a-ctx` | Long context: retrieving and using material far back in the prompt |

The full suite is 196 items. Board columns are scored on a fixed **60-item
subset**, 12 items from each family, so that columns added at different times
remain comparable to each other.

## Three things the scorer separates

Most agent evaluations collapse these together, which makes their numbers hard
to trust:

1. **Wrong answers.** The model completed the task incorrectly.
2. **Harness artifacts.** The failure was the test rig's, not the model's.
   These are counted and reported separately, never as model failures.
3. **Refusals at a real limit.** For example, a serve rejecting an `a-ctx`
   item with a clean HTTP 400 because the prompt exceeds its configured
   context ceiling is a *configuration* limit being reported honestly, not a
   reasoning failure. It is recorded as such.

## Canary items

A subset of items is repeated to detect **outcome flips**: the same item
producing a different pass/fail across identical runs. A column reporting
"0 of 16 canary flips" has demonstrated stable scoring at that configuration.
A column with flips has its other numbers read with appropriate suspicion.

This matters because byte-level non-determinism at temperature 0 is normal on
this stack. Byte-instability is tolerable; *outcome* instability is not, and
the canaries are how the difference is established rather than assumed.

## Reasoning leaks

Items are also checked for reasoning content leaking into the answer channel,
which some builds do under tool-call or schema constraints. It is reported as
its own count.

## Layout

```
harness/a-bench/
  items/     the item set
  abench/    runner
  tools/     scoring and reporting
  smoke/     quick self-check
  tests/     tests for the instrument itself
```
