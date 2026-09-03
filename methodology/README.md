# Methodology

## The core rule: each model at its own best

Every model is served in isolation on one DGX Spark, at the engine,
quantisation and speculative-decoding method that gives *that model* its best
result. Engines and quantisations therefore differ from column to column by
design.

This is a deliberate departure from the usual approach of fixing one stack and
running everything through it. A fixed stack measures "how well does this model
cope with my setup", which quietly penalises any model whose best path is a
different engine or precision. This board tries to measure the model at its
ceiling on this hardware.

The cost of that choice is that columns are not directly comparable as wholes.
That is accepted and handled by the scoring rule below.

## Scoring: within-row standings only

Each cell carries three things:

1. a **0-10 standing within its row**,
2. the **raw measurement** that produced it, and
3. the **exact configuration** that produced that measurement.

**There are no column totals.** Adding a speed standing to an OCR standing
gives a number that means nothing. A standing is only meaningful against the
other cells in the same row.

Cells that were not measured are marked as not entered, and say why. A gap in
the schedule and a genuine incapability are recorded differently, because they
mean different things.

## Instrument faults are recorded as faults, not results

When a measurement is obviously out of family with every other column in its
row, it is treated as a suspected instrument fault and re-measured, rather than
boarded as a model result. One worked example: reading per-token
bits-per-character off a serve carrying speculative decoding returned values
roughly ten times every other column, because of how draft/verify tokens are
accounted. That is a measurement artifact. The fix was to re-measure with
speculation off, not to publish the number.

See [`../GOTCHAS.md`](../GOTCHAS.md) item 8.

## Configuration is pinned and recorded

Every measurement records the engine build, quantisation, context length,
memory fraction, drafter, and parser settings that produced it. A number
without its configuration is not useful on this hardware, where a single flag
can change both speed and stability. Determinism is enabled only for scoring
runs, never for serving.

## The two purpose-built instruments

- **[`s-bench.md`](s-bench.md)** for separating models that are close together.
- **[`a-bench.md`](a-bench.md)** for agentic capability.
