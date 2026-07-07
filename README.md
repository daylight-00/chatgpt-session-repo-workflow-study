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

## From capability study to practical use

The study now also includes a practical operational layer for new real projects:

- [`playbook/README.md`](playbook/README.md) — purpose and design principles of the Session Workflow Playbook.
- [`playbook/CAPABILITY_MATRIX.tsv`](playbook/CAPABILITY_MATRIX.tsv) — verified capabilities, conditions, tradeoffs, known failures, workarounds, and evidence pointers.
- [`playbook/WORKFLOW_DECISION_TREE.md`](playbook/WORKFLOW_DECISION_TREE.md) — topology selection for source, execution, dependencies, checkpointing, artifact durability, and publication.
- [`playbook/PRACTICAL_GUIDE_OUTLINE.md`](playbook/PRACTICAL_GUIDE_OUTLINE.md) — structure for the full field guide, reusable recipes, and future session-facing forms.

The playbook is not a claim that every project should run inside the session sandbox. It treats the study as a capability map and helps a new project session choose among sandbox execution, GitHub-native automation, Google Drive durability, local workstations, external cluster/HPC execution, or hybrid compositions.

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

The CPython pilot bundle remains recoverable and externally backed up. The original Foundry PR #306 main archive was lost during a runtime reset, but a complete rerun has now reproduced the scientific workflow and produced a new fully verified raw bundle. The rerun archive is backed up in seven Google Drive chunks, was raw-downloaded and reassembled, and matched the local archive byte-for-byte. Public Release publication remains pending redistribution review for bundled source archives, runtime/package material, and the model checkpoint. The historical loss and the successful recovery-by-rerun are documented in [`docs/part-3-execution-workspace/runtime-lifecycle.md`](docs/part-3-execution-workspace/runtime-lifecycle.md).
