# Part I — Project workflow and persistence

## Question

Can a ChatGPT session organize project state, persist outputs, and work with a real remote repository?

## Scope

Part I covers session-local workspace behavior, `sandbox:` artifact handoff, File Library visibility and object behavior, local-file versus persistent-object boundaries, GitHub connector reads/writes, branch and ref behavior, commit attribution, pull requests, merges, and repository packaging.

Detailed reports currently remain at the historical paths:

- `docs/01-scope-and-method.md`
- `docs/02-session-local-workspace.md`
- `docs/03-file-library-handoff.md`
- `docs/04-github-connector.md`
- `docs/05-suitability-assessment.md`

The corresponding experiment records currently remain under `experiments/` and will be moved only after the new study-level structure is stable.

## Core model

```text
session runtime filesystem
        ↓ explicit handoff
persistent artifact / File Library layer

assistant
        ↓ GitHub connector / API operations
remote GitHub repository
```

## Conclusion

The session is sufficiently capable for repository-oriented project work when the state domains are treated separately and transitions are explicit. GitHub operations are remote API mutations through the connector, while `/mnt/data` serves as an active local workspace whose durability must be handled explicitly.
