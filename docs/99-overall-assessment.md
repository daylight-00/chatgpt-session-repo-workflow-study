# Overall assessment

## Answer to the central question

A ChatGPT session can function as a practical, evidence-preserving software project workspace, with explicit constraints.

The study supports three complementary conclusions.

### 1. Project workflow is viable

The session can generate and transform files, hand off persistent artifacts, and operate on real GitHub repositories through API-mediated connector actions including branches, commits, comparisons, pull requests, and merges.

### 2. Historical reconstruction is partial, not complete

File Library metadata and session-level historical context can support useful approximate chronology reconstruction. Exact message-by-message wall-clock history and an enumerable conversation-object API were not observed.

### 3. Active development is viable

The sandbox can provision dependencies through available internal mirrors, compile native software, execute tests, perform incremental rebuilds, run native debugging, and execute realistic scientific Python/model-inference workflows on CPU.

## Main limitations

- runtime filesystem continuity is not guaranteed;
- artifact persistence must be verified explicitly;
- long monolithic commands are less convenient than incremental workflows;
- public shell networking is restricted while internal mirrors may be available;
- exact official environment parity may require extra work even when functional compatibility is demonstrated;
- heavy GPU/HPC capability remains outside the scope of the current sandbox evidence;
- public redistribution of bundled checkpoints/runtimes must be reviewed separately from technical reproducibility.

## Recommended operating model

```text
Git repository
  source of truth for narrative, methods, compact evidence, scripts, manifests

session sandbox
  active development and experiment workspace

artifact / Release layer
  durable complete evidence bundles and verification sidecars

external cluster or GPU systems
  heavy compute backend when required
```

The decisive lesson is not that one surface replaces all others. The workflow becomes practical when the surfaces are composed deliberately and evidence handoffs are treated as first-class operations.
