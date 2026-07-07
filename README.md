# ChatGPT session repository workflow study

Personal experiment record on whether a current ChatGPT session is suitable for repository-based project work.

## Conclusion

The session is sufficiently capable for small to medium repository-oriented work when the workflow is designed around three distinct surfaces:

1. **Session-local workspace** for file generation, transformation, packaging, and shell/Python operations.
2. **File Library** for persistent user-facing artifacts promoted from session-local files through `sandbox:` references.
3. **GitHub connector** for remote repository reads and writes through GitHub API abstractions rather than a local Git checkout.

The model is usable, but it is not equivalent to a conventional terminal development environment. The main constraints observed were:

- File Library management is not exposed as a writable filesystem or CRUD API.
- Artifact promotion and Library search visibility are distinct stages.
- GitHub writes are API-mediated; local `git pull` / `git push` credentials were not present in the shell.
- The available GitHub connector surface did not expose repository creation or Release creation/upload functions in this session. This study repository was therefore created manually, after which the connector could populate it through repository content operations.
- Direct default-branch ref mutation was safety-blocked in the packaging probe; feature branch, PR, and squash-merge publication succeeded.

Detailed findings are in `docs/` and the experiment record is in `experiments/`.

## Layout

```text
.
├── README.md
├── docs/
│   ├── 01-scope-and-method.md
│   ├── 02-session-local-workspace.md
│   ├── 03-file-library-handoff.md
│   ├── 04-github-connector.md
│   └── 05-suitability-assessment.md
├── experiments/
│   ├── matrix.md
│   ├── timeline.md
│   ├── github-probe.md
│   └── repository-packaging.md
├── artifacts/
│   ├── README.md
│   ├── manifest.tsv
│   ├── files.tsv
│   ├── inventory.tsv
│   └── probes/
└── archive/
    ├── README.md
    └── MANIFEST.sha256
```

## Session date

2026-07-07, Asia/Seoul.
