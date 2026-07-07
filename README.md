# ChatGPT Session Workspace Study

*An empirical capability study and workflow playbook for practical, evidence-preserving software project work in ChatGPT sessions.*

> **Central question:** Can a ChatGPT session function as a practical, evidence-preserving software project workspace?
>
> **Result:** **Yes, conditionally.** The workflow becomes practical when runtime execution, durable evidence, repository state, external compute, and publication are treated as distinct systems with explicit handoff and verification boundaries.

## What was studied

The investigation developed in three parts:

1. **Project workflow and persistence** — session-local work, artifact handoff, connector boundaries, Google Drive backup, and real GitHub repository operations.
2. **Meta-observability and partial recovery** — what artifact/session history can be observed and how much chronology can be reconstructed.
3. **Active execution workspace capability** — runtime characterization, dependency provisioning, native build/test/debug work, and a realistic scientific-software pilot.

Two complementary Part III pilots were used:

- **CPython** — native systems-development pilot covering dependency provisioning, configure/build, targeted tests, source modification, incremental rebuild, restoration, and GDB debugging.
- **Foundry PR #306 / LigandMPNN** — scientific-software pilot covering real structure parsing, checkpoint provenance, CPU model inference, output validation, determinism checks, fault injection, recovery, and batch scaling. After the original raw archive was lost during a runtime reset, the pilot was rerun and a new independently verified evidence bundle was produced and round-trip recovered from off-runtime backup.

## Key findings

- Real repository-oriented project work is viable through explicit state boundaries and connector/API operations.
- The sandbox can build, test, incrementally rebuild, debug native software, and run realistic CPU scientific workflows under resource constraints.
- Runtime continuity is not guaranteed; local execution state must not be confused with durable artifact storage.
- Resource visibility, authorization, connector action availability, and argument/file representation are separate failure boundaries.
- Large raw evidence can be preserved off-runtime and verified by raw download, hash checking, reassembly, and final archive integrity validation.
- The useful model is **hybrid**, not “run everything inside the sandbox.” GPU, HPC, GUI, CI, local workstation, and session execution should be composed according to workload and lifecycle.

## Repository map

```text
docs/           study narrative, methods, interpretation, and assessment
experiments/    procedures and experiment records
evidence/       compact raw or near-raw evidence suitable for Git history
playbook/       reusable workflow-selection guidance for new projects
release-assets/ public evidence bundle metadata, checksums, notices, and release guide
legacy/         historical packaging and repository-evolution records
```

## Start here

### Read the study

- [`docs/00-study-overview.md`](docs/00-study-overview.md) — central question and three-part narrative.
- [`docs/01-method-and-evidence-policy.md`](docs/01-method-and-evidence-policy.md) — observation/inference discipline and evidence policy.
- [`docs/99-overall-assessment.md`](docs/99-overall-assessment.md) — final cross-part assessment.

Part-specific entry points:

- [`docs/part-1-project-workflow/overview.md`](docs/part-1-project-workflow/overview.md)
- [`docs/part-2-meta-observability/overview.md`](docs/part-2-meta-observability/overview.md)
- [`docs/part-3-execution-workspace/overview.md`](docs/part-3-execution-workspace/overview.md)

### Use the findings in a new project

- [`playbook/README.md`](playbook/README.md) — Session Workflow Playbook.
- [`playbook/CHATGPT_SESSION_BOOTSTRAP.md`](playbook/CHATGPT_SESSION_BOOTSTRAP.md) — short entry brief for a fresh ChatGPT project session.
- [`playbook/CAPABILITY_MATRIX.tsv`](playbook/CAPABILITY_MATRIX.tsv) — capability status, conditions, tradeoffs, failures, workarounds, and evidence pointers.
- [`playbook/WORKFLOW_DECISION_TREE.md`](playbook/WORKFLOW_DECISION_TREE.md) — topology-first workflow selection.

The playbook treats the study as prior operational knowledge, not as a prescription to use the session sandbox for every workload.

## Evidence and durability model

```text
Git repository
  narrative, methods, experiment records, compact evidence, playbook

Session runtime
  active but ephemeral execution and development workspace

Off-runtime backup
  immediate durable preservation of valuable raw input/output/log bundles

GitHub Release
  curated publication of verified complete evidence bundles and sidecars
```

Small evidence suitable for Git history lives under [`evidence/`](evidence/). Large complete pilot bundles are tracked under [`release-assets/`](release-assets/) and published separately from the source tree.

## Evidence Release

The planned `Study Evidence Bundle v1.0` Release contains the complete CPython pilot bundle, the independently verified Foundry rerun bundle, their verification sidecars, and supplemental study-level raw evidence. The connector-specific Drive chunk files are backup transport objects and are not part of the public Release asset set.

See:

- [`release-assets/RELEASE_GUIDE.md`](release-assets/RELEASE_GUIDE.md) — draft-first publication and verification procedure.
- [`release-assets/RELEASE_ASSETS.tsv`](release-assets/RELEASE_ASSETS.tsv) — planned public asset set.
- [`release-assets/SHA256SUMS`](release-assets/SHA256SUMS) — expected hashes for primary Release assets.
- [`release-assets/THIRD_PARTY_NOTICES.md`](release-assets/THIRD_PARTY_NOTICES.md) — known third-party origins and publication-review notes.

## Scope and caveats

The study establishes practical capability under observed conditions; it does not claim permanent platform contracts. Connector surfaces, runtime resources, mirrors, and service behavior are environment-sensitive and should be rechecked when relevant. Capabilities that were not observed are not treated as impossible.

The project-wide rights notice is in [`LICENSE.md`](LICENSE.md). Third-party source, packages, model parameters, and other bundled materials remain subject to their own licenses and notices.
