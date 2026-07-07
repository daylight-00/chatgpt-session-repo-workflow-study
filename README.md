# ChatGPT Session Workspace Study

*An empirical capability study and workflow playbook for practical, evidence-preserving software project work in ChatGPT sessions.*

> **Central question:** Can a ChatGPT session function as a practical, evidence-preserving software project workspace?
>
> **Result:** **Yes, conditionally.** The workflow becomes practical when execution, durable evidence, repository state, external compute, and publication are treated as distinct systems with explicit handoff and verification boundaries.

## What was studied

1. **Project workflow and persistence** — session-local work, artifact handoff, connector boundaries, Google Drive backup, and real GitHub repository operations.
2. **Meta-observability and partial recovery** — what artifact/session history can be observed and how much chronology can be reconstructed.
3. **Active execution workspace capability** — runtime characterization, dependency provisioning, native build/test/debug work, and realistic scientific-software execution.

Part III used two complementary pilots:

- **CPython** — dependency provisioning, configure/build, targeted tests, source modification, incremental rebuild, restoration, and GDB debugging.
- **Foundry PR #306 / LigandMPNN** — real structure parsing, checkpoint provenance, CPU inference, output validation, determinism checks, error-injection testing, recovery, and batch scaling. After the original raw archive was lost during a runtime reset, the pilot was rerun and a new independently verified evidence bundle was produced and recovered byte-for-byte from off-runtime backup.

## Key findings

- Repository-oriented project work is viable through explicit state boundaries and connector/API operations.
- The sandbox can build, test, incrementally rebuild, debug native software, and run realistic CPU scientific workflows under resource constraints.
- Runtime continuity is not guaranteed; execution state must not be confused with durable artifact storage.
- Resource visibility, authorization, connector action availability, and file/argument representation are separate failure boundaries.
- Large raw evidence can be preserved off-runtime and verified by download, hashing, reassembly, and archive integrity checks.
- The useful model is **hybrid**, not “run everything inside the sandbox.” Session execution, local workstations, CI, GPU/HPC systems, Git, and durable storage should be composed according to workload and lifecycle.

## Repository map

```text
docs/           study narrative, methods, interpretation, and assessment
experiments/    procedures and experiment records
evidence/       compact raw or near-raw evidence suitable for Git history
playbook/       reusable workflow-selection guidance for new projects
release-assets/ evidence bundle metadata, checksums, notices, and publication policy
legacy/         historical packaging and repository-evolution records
artifacts/      legacy Part I probe compatibility tree
archive/        historical compatibility pointer
```

## Start here

For the study:

- [`docs/00-study-overview.md`](docs/00-study-overview.md)
- [`docs/01-method-and-evidence-policy.md`](docs/01-method-and-evidence-policy.md)
- [`docs/99-overall-assessment.md`](docs/99-overall-assessment.md)

For practical use in a new project:

- [`playbook/README.md`](playbook/README.md)
- [`playbook/CHATGPT_SESSION_BOOTSTRAP.md`](playbook/CHATGPT_SESSION_BOOTSTRAP.md)
- [`playbook/CAPABILITY_MATRIX.tsv`](playbook/CAPABILITY_MATRIX.tsv)
- [`playbook/WORKFLOW_DECISION_TREE.md`](playbook/WORKFLOW_DECISION_TREE.md)

The playbook treats the study as prior operational knowledge, not as a prescription to use the session sandbox for every workload.

## Evidence model

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

The planned **Study Evidence Bundle v1.0** includes the complete CPython pilot bundle, the independently verified Foundry rerun bundle, their verification sidecars, and supplemental study-level raw evidence. See [`release-assets/README.md`](release-assets/README.md), [`release-assets/RELEASE_ASSETS.tsv`](release-assets/RELEASE_ASSETS.tsv), [`release-assets/SHA256SUMS`](release-assets/SHA256SUMS), and [`release-assets/THIRD_PARTY_NOTICES.md`](release-assets/THIRD_PARTY_NOTICES.md).

## Scope and caveats

The study establishes practical capability under observed conditions; it does not claim permanent platform contracts. Connector surfaces, runtime resources, mirrors, and service behavior are environment-sensitive and should be rechecked when relevant. Capabilities that were not observed are not treated as impossible.

See [`RIGHTS.md`](RIGHTS.md) for the project rights note. Third-party source, packages, model parameters, and other bundled materials remain subject to their own licenses and notices.
