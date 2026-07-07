# Refactor audit summary

## Purpose

Before restructuring the repository, the current Git tree, early study archive, loose probe files, CPython pilot bundle, Foundry verification sidecar, and large-asset metadata were audited read-only.

## Main findings

### 1. The problem was structural imbalance, not lack of evidence

Part I and Part II were represented in the repository, while Part III had much stronger evidence architecture but lived mostly in external pilot bundles. The refactor therefore organized one three-part capability study rather than creating independent pilot projects.

### 2. The early tar.gz is a historical snapshot

`chatgpt-session-repo-workflow-study-20260707.tar.gz` predates Part II integration and Part III. It must not be described as a current whole-project archive.

### 3. Binary probe packaging had a gap

The legacy artifact README said PNG and ZIP probes were preserved in the tar.gz. Audit of the currently available snapshot found that they were not present. Their exact bytes are now represented as hexadecimal text plus original SHA-256 under `evidence/part-1-project-workflow/binary-probes/`.

### 4. Part II has the weakest raw-evidence preservation

The conclusions are documented, but representative raw query/response records were not comprehensively preserved. The backdated-mtime source probe and timeline anchor were collected into `evidence/part-2-meta-observability/`.

### 5. CPython evidence is currently reopenable

The CPython pilot archive and verification sidecar survived artifact rehydration after runtime reset. The pilot had full source/build state, logs, documentation, inventory, manifest, and extraction verification.

### 6. Foundry historical verification survived, but the main bytes did not

The Foundry main archive was previously verified against 7,773 file hashes and 9,926 metadata entries. After runtime reset, the checksum and verification sidecar were rehydrated, but the main archive bytes were not.

### 7. Runtime continuity must be part of the evidence model

The runtime was observed to have restarted, with installed packages and original workspaces gone. Some independently handed-off artifacts were selectively rehydrated. This led to the explicit runtime persistence policy in `01-method-and-evidence-policy.md` and the detailed record in Part III `runtime-lifecycle.md`.

## Refactor consequence

The repository now separates:

```text
docs/           narrative and assessment
experiments/    procedure and experiment records
evidence/       compact Git-suitable evidence
release-assets/ large-bundle metadata and readiness
legacy/         historical packaging notes
```

Large pilot bundles remain outside Git history and are tracked by checksum and verification status.
