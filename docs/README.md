# Krishna Defence Documentation Map

This folder separates **current release evidence** from **historical research** so evaluators do not have to navigate experiment-by-experiment version history.

## For Hack for Social Cause evaluators

Read these first:

1. [`../README.md`](../README.md) — project overview and setup instructions.
2. [`release/FINAL_HACKATHON_EVIDENCE.md`](release/FINAL_HACKATHON_EVIDENCE.md) — authoritative current evidence and claim boundaries.
3. [`../RELEASE_EVIDENCE.md`](../RELEASE_EVIDENCE.md) — compact evidence index.
4. [`../garuda_v3/`](../garuda_v3/) — current forecasting / runtime implementation.
5. [`../hackathon_submission/`](../hackathon_submission/) — submission and demo material.

## Current release evidence

`docs/release/` contains release-facing experiments, benchmark records, runtime audits and safety evidence.

The authoritative judge-facing file is:

> **[`release/FINAL_HACKATHON_EVIDENCE.md`](release/FINAL_HACKATHON_EVIDENCE.md)**

If an older document conflicts with this file, use `FINAL_HACKATHON_EVIDENCE.md` for the current Hack for Social Cause claim boundary.

## Historical / superseded work

`docs/archive/` contains exploratory, superseded, failed and historical research material retained for auditability.

These files are useful for engineering history, but they are **not the recommended evaluator entry point** and should not be used as current headline evidence unless the final evidence matrix explicitly references them.

## Repository navigation

| Need | Go to |
|---|---|
| Run the Hack for Social Cause demo | [`../README.md#quick-setup--main-demo`](../README.md#quick-setup--main-demo) |
| Main dashboard | [`../console.html`](../console.html) |
| Current forecasting code | [`../garuda_v3/`](../garuda_v3/) |
| Current evidence | [`release/FINAL_HACKATHON_EVIDENCE.md`](release/FINAL_HACKATHON_EVIDENCE.md) |
| Dataset provenance | [`../datasets/`](../datasets/) |
| Submission material | [`../hackathon_submission/`](../hackathon_submission/) |
| Security validation | [`../security_validation/`](../security_validation/) |
| Reproducibility workflows | [`../.github/workflows/`](../.github/workflows/) |
| Historical experiments | [`archive/`](archive/) |

## Evidence rule

Different datasets, protocols and evaluation scopes are kept separate. Do not combine unrelated results into one synthetic accuracy number, and do not promote historical exploratory results over the current release matrix.
