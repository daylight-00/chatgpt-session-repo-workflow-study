# Release asset policy

Large experiment bundles are kept out of Git history.

`MANIFEST.tsv` records expected asset name, experiment role, byte size when known, SHA-256 when known, current availability, verification state, and redistribution-review state.

A historical checksum alone is not enough to mark an asset Release-ready. The actual bytes must be available and reverified before publication.

## Backup durability and Release publication are separate stages

The current workflow intentionally separates immediate preservation from curated publication:

```text
experiment completes
→ archive and verify
→ emergency Google Drive backup
→ raw-download round-trip verification
→ documentation and redistribution review
→ GitHub Release publication when a supported publication path is available
```

The Google Drive path has been verified end-to-end for both CPython and the Foundry rerun bundles.

A separate GitHub connector capability assessment found that private repository access and admin permission were available while the observed connector surface exposed no direct Release creation, Release asset upload, explicit generic tag creation, or workflow-dispatch primitive. A repository-native Actions path remains plausible but has not yet been validated end-to-end.

Therefore `MANIFEST.tsv` should describe evidence readiness independently from publication-path readiness.

See:

- `docs/part-1-project-workflow/connector-boundaries.md`
- `experiments/part-1-project-workflow/github-release-capability.md`
