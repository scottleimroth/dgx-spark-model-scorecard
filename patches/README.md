# Patches

Local patches needed to get particular builds serving on GB10 / sm_121.

## Scope

These exist because the DGX Spark's GB10 is new enough that some upstream
builds need a nudge. Each patch here states:

- the **upstream project and commit** it applies to,
- **what it changes** and why,
- whether it has been **submitted upstream**, and
- when it becomes **unnecessary** (that is, which upstream version fixes it).

A patch whose upstream fix has landed is kept, marked superseded, and dated
rather than deleted, so an older dated scorecard remains reproducible.

## Applying

Patches are against specific commits. Check the header of each `.patch` before
applying, and do not assume one still applies to a newer build. If it fails to
apply cleanly, the build has probably moved past it, which is good news.

## Contributing

If you have a GB10 patch that others need, open a pull request. Include the
upstream commit, the failure it fixes, and the observable symptom before the
fix, since the symptom is usually the hardest part to search for.
