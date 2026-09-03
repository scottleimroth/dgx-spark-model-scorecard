# Gotchas

Operational findings from running twenty models hard on a single DGX Spark
(GB10). Each one cost real time. Ordered by how badly it can bite you.

## 1. The determinism flag can kill the whole server on an ordinary prompt

On the SGLang builds used here, `--enable-deterministic-inference` hard-codes
FlashInfer's prefill workspace to 2 GiB and uses fixed 4096-token KV splits.
Any **fresh prompt of roughly 7,300 tokens or more** overflows that workspace,
and the scheduler exception takes down the entire server. It exits 0, so your
supervisor cheerfully restarts it and you see a clean exit code rather than a
crash.

It also disables the radix cache in that build.

**Rule: enable determinism only for benchmark scoring, never for serving.** It
exists so scores are comparable, not so production is stable. Watch for it
creeping back in when a serving script is derived from a benchmark script,
which is exactly how it bit here.

## 2. Docker fabricates a missing bind-mount source, and the serve dies late

If a bind-mount source path does not exist, Docker silently creates it as an
empty directory. The container then starts, loads nothing useful, and fails
much later with a confusing error a long way from the real cause.

**Rule: preflight every mount source for real content before launch.** Check
that the model directory has a `config.json`, that the expected number of
safetensors shards are present, that the drafter file is non-empty, and that
the image actually exists locally. Fail loudly before the container starts.

Related: if you delete a file that is a bind-mount source, Docker recreates it
as a *directory* on the next start, and the container then fails permanently
with "not a directory". The real reason shows up in the container's
`.State.Error`, not in its logs.

## 3. Read `accept_length`, not `accept_rate`, when comparing draft budgets

Engines commonly report a speculative "acceptance rate" computed as
`(accepted_length - 1) / (budget - 1)`. The divisor grows with the budget, so
as you raise the draft budget the *rate* falls purely arithmetically while the
accepted **length** is flat.

Measured here across draft budgets 6, 8 and 10, the reported rate fell from
0.253 to 0.224 to 0.174 while accepted length was essentially unchanged from 8
to 10. Ranking budgets by "rate" would have picked the wrong one.

**Rule: accepted length is the comparable quantity across budgets. Rate is
not.**

## 4. A newer engine version is not automatically faster

vLLM 0.26 and 0.28 measured **identical** on this hardware for the same model,
both with speculation off and on. Upgrading bought nothing. Measure before you
attribute a change to a version bump.

## 5. Unified memory makes naive free-memory arithmetic lie

On GB10 the CPU and GPU share one 128 GB pool. Adding up what you think each
component wants, or reading a "free memory" figure, will mislead you.
**Measure resident footprint** for the configuration you actually intend to
serve.

## 6. Lowering the memory fraction is not always safe

Reducing the static memory fraction to make room for co-resident work can
crash Mamba-family and hybrid models outright, rather than simply giving them
a smaller cache. Test the fit; do not assume a lower fraction is strictly
more conservative.

## 7. Pre-claiming memory is a feature, not a bug

A model that claims its memory at launch will fail *at launch* if it cannot fit
beside whatever else is resident. That is much better than discovering the
collision later under load, in whichever lane happens to lose. Prefer configs
that fail loudly and early.

## 8. Speculative decoding preserves the output distribution

Draft-plus-verify speculative decoding is distribution-preserving: the target
model verifies every drafted token against its own probabilities. So enabling
it changes speed, not output quality or likelihood.

The practical corollary is a measurement one: reading a per-token quantity such
as bits-per-character off a speculative serve is fiddly, because the
draft/verify token accounting inflates a naive read. Measure likelihood with
speculation off; it is the same number either way.
