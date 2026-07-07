# ChatGPT session workspace capability study

This repository studies one practical question:

> **Can a ChatGPT session function as a practical, evidence-preserving software project workspace?**

The investigation developed in three parts.

1. **Project workflow and persistence** — whether a session can create project state, persist artifacts, and operate on a GitHub repository.
2. **Meta-observability and partial recovery** — how much of the session, artifact history, and experiment chronology can later be observed or reconstructed.
3. **Execution workspace capability** — whether the sandbox can provision dependencies, execute code, build native software, run tests, debug failures, and complete realistic scientific-software work.

The overall result is positive but conditional: the session can support real project work when runtime state, durable artifacts, remote repository state, and historical context are treated as separate systems with explicit handoff and verification boundaries.

## Study map

- [`docs/00-study-overview.md`](docs/00-study-overview.md) — unifying question and three-part narrative.
- [`docs/01-method-and-evidence-policy.md`](docs/01-method-and-evidence-policy.md) — observation/inference discipline and evidence policy.
- [`docs/part-1-project-workflow/overview.md`](docs/part-1-project-workflow/overview.md) — repository workflow and persistence.
- [`docs/part-2-meta-observability/overview.md`](docs/part-2-meta-observability/overview.md) — session/file metadata and partial chronology recovery.
- [`docs/part-3-execution-workspace/overview.md`](docs/part-3-execution-workspace/overview.md) — runtime, dependency provisioning, and two execution pilots.
- [`docs/99-overall-assessment.md`](docs/99-overall-assessment.md) — cross-part assessment.

## Evidence model

```text
Git repository
  narrative, method, compact probes, scripts, small logs, manifests

Release assets
  complete pilot archives and verification sidecars

Runtime filesystem
  active but ephemeral working state
```

Large pilot bundles are intentionally not committed to Git history. Their expected metadata and readiness state are tracked in [`release-assets/MANIFEST.tsv`](release-assets/MANIFEST.tsv).

## Current Release readiness note

The CPython pilot archive and its verification sidecar are currently recoverable in the session artifact layer. The Foundry PR #306 verification sidecar and expected main-archive SHA-256 are preserved, but the main archive bytes did not survive a later runtime reset. The runtime lifecycle finding is documented in [`docs/part-3-execution-workspace/runtime-lifecycle.md`](docs/part-3-execution-workspace/runtime-lifecycle.md).
