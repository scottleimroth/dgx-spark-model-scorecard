# Serving configs

One file per board column. Each names the upstream checkpoint, the exact serve
line, and **why that configuration is the one this model was scored at**.

That last part is the point of this directory. The board's core claim is that
every model was served at its own best, and a claim like that is only worth
anything if the configuration is on the table next to the number.

## Reading a config file

Every file carries:

- **Upstream** the base checkpoint, its quantisation contract, drafter and
  engine build. Weights are never redistributed here.
- **Serve** a runnable command. Paths, ports and fabric are **examples**; set
  them for your own machine.
- **Why this is its best config** the reasoning behind each non-obvious flag,
  including flags deliberately left *out*.
- **Preflight** what to check before launching, because several failure modes
  on this stack show up late and confusingly.

## Conventions

- Nothing here contains real hostnames, addresses, key material or private
  paths. If you spot something that looks like it does, open an issue.
- Memory fractions in these files generally leave headroom for other work on
  the box rather than maximising LLM throughput. Raise them if your Spark is
  dedicated to serving.
- Determinism flags are absent from every serving config on purpose. See
  [`../GOTCHAS.md`](../GOTCHAS.md) item 1.

## Files

| Config | Column |
|---|---|
| [`qwen3.8-27b-radixark-nvfp4.md`](qwen3.8-27b-radixark-nvfp4.md) | Qwen3.8-27B, NVFP4, DFlash2 drafter |

Remaining columns are being added; see the interactive board for the full set
in the meantime.
