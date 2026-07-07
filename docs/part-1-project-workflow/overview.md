# Part I — Project workflow and persistence

## Question

Can a ChatGPT session organize project state, persist outputs, and work with real remote services and repositories?

## Scope

Part I covers session-local workspace behavior, `sandbox:` artifact handoff, File Library visibility and object behavior, local-file versus persistent-object boundaries, Google Drive connector file handoff, GitHub connector reads/writes, branch and ref behavior, commit attribution, pull requests, merges, Release capability limits, and repository packaging.

Detailed reports:

- `scope-and-method.md`
- `session-local-workspace.md`
- `file-library-handoff.md`
- `connector-boundaries.md`
- `github-connector.md`
- `assessment.md`

Experiment records:

- `experiments/part-1-project-workflow/matrix.md`
- `experiments/part-1-project-workflow/timeline.md`
- `experiments/part-1-project-workflow/github-probe.md`
- `experiments/part-1-project-workflow/repository-packaging.md`
- `experiments/part-1-project-workflow/google-drive-file-handoff.md`
- `experiments/part-1-project-workflow/github-release-capability.md`

The last two connector-boundary experiments were performed in separate sessions and later integrated with explicit provenance under `evidence/part-1-project-workflow/external-session-reports/`.

## Core model

```text
session runtime filesystem
        ↓ explicit artifact handoff
persistent artifact layer
        ↓ transferable file reference
Google Drive backup

assistant
        ↓ GitHub connector / API operations
remote GitHub repository

curated large evidence
        ↓ supported publication path
GitHub Release target
```

The project distinguishes representation boundaries from capability-surface boundaries:

```text
Drive upload failure
  can occur because a local path is not yet a transferable connector file reference

GitHub Release failure
  can occur because the connector does not expose the required mutation primitive
```

## Conclusion

The session is sufficiently capable for repository-oriented and evidence-preserving project work when state domains and connector boundaries are treated separately. `/mnt/data` is an active but ephemeral workspace, Drive is a verified emergency durability layer, GitHub connector operations provide remote repository mutation, and Release publication remains a separate curated step requiring an available direct or validated indirect publication path.
