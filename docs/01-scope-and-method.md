# Scope and method

## Objective

This experiment evaluated whether a ChatGPT session can support repository-based project work without assuming a conventional local developer workstation.

The investigation focused on three execution and persistence surfaces:

- session-local filesystem and tools;
- File Library persistence and retrieval;
- GitHub connector read/write behavior.

The target question was practical rather than architectural: **can a repository-oriented project be carried out coherently inside the service, and what are the observable boundaries?**

## Method

The investigation used controlled probes with one variable changed at a time.

For session-local and File Library behavior, files were created under `/mnt/data` and then exposed through different response forms. Promotion was tested with:

- creation only;
- tool stdout containing a path;
- basename only;
- absolute local path;
- raw `sandbox:` URI;
- Markdown link whose target was a `sandbox:` URI;
- commentary-stage references;
- final-stage references;
- same-segment duplicate references;
- cross-segment references;
- local overwrite and local deletion after promotion;
- zero-byte, PNG, ZIP, directory, and nonexistent-path probes;
- same-turn and next-turn search visibility checks.

For GitHub behavior, a private sandbox repository was used to avoid changing unrelated repositories. A dedicated probe branch was created from `main`, then a text file was created, read, updated, deleted, and the branch ref was moved backward and forward. Commit metadata was read back to inspect authorship.

## Evidence model

The report distinguishes:

- **directly observed behavior**: a tool call returned a specific result or a subsequent read confirmed state;
- **working hypothesis**: an internal mechanism inferred from multiple observations but not directly exposed by the service;
- **unobservable implementation detail**: behavior that cannot be separated without server-side logs or a lower-level API surface.

The report does not assume undocumented backend internals beyond what is required to explain observed state transitions.
