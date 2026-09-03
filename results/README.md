# Results

Dated scorecards. The live interactive version, with per-cell configuration
popups, is at
**https://scottleimroth.com/ai-technology/spark-benchmarks**.

## Why results are dated, not overwritten

A benchmark number is a claim about a specific engine build, quantisation and
configuration at a specific time. Engine versions move, checkpoints get
re-quantised, and flags change behaviour between builds. Overwriting a table
destroys the ability to say *when* something was true.

So each scorecard here is stamped and kept. A superseded number stays visible
with its date rather than disappearing.

## Reading a scorecard

- A cell's **0-10 figure is a standing within its row only**. Rows are never
  summed into a column total.
- Each cell shows its **raw measurement** and the **configuration** that
  produced it.
- **Not entered** is distinguished from **not capable**. A schedule gap and a
  genuine incapability are recorded differently.
- Suspected **instrument faults** are marked as faults and re-measured, not
  boarded as model results.

Full rules in [`../methodology/`](../methodology/).

## `raw/`

Sanitised run output backing the tables. Real hostnames, addresses, key
material and private paths are removed; measurement content is not altered.
Where a log has been trimmed, it says so.

## Contributing a result

Numbers from other DGX Spark boxes are welcome, particularly ones that
disagree with these. Open an issue with the config, the raw output and the
engine build, and it will be added as its own dated entry rather than merged
into an existing one.
