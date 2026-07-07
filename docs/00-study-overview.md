# Study overview

## Central question

**Can a ChatGPT session function as a practical, evidence-preserving software project workspace?**

The study progressed from weaker to stronger claims.

```text
Part I
Can the session maintain project state and work with a repository?

Part II
Can the session's own artifact/session history be observed and partially reconstructed?

Part III
Can the sandbox execute, build, test, debug, and run realistic scientific software?
```

## Part I — Project workflow and persistence

Part I maps the state domains available to a session:

```text
runtime filesystem
persistent artifact / File Library layer
remote GitHub repository state
```

It tests local file creation, artifact handoff behavior, File Library visibility, GitHub connector writes, branch/ref behavior, commit attribution, PR publication, and archive packaging.

The main conclusion is that repository-oriented work is coherent when these domains are treated separately and handoffs are explicit.

## Part II — Meta-observability and partial recovery

Part II is intentionally meta: it studies the environment's ability to observe or recover information about prior artifacts and sessions rather than directly advancing a software project.

Observed useful signals include File Library object metadata, session-level timestamps, session titles, selected historical content, and event ordering. Message-level wall-clock timestamps and an enumerable conversation-object search/list/get surface were not observed.

The practical conclusion is:

> Complete recovery was not observed, but approximate chronology can be reconstructed by combining File Library object metadata, session-level contextual information, and event ordering.

## Part III — Active execution workspace

Part III starts with runtime characterization and dependency-provisioning exploration, then uses two complementary pilots.

### CPython pilot

Role: **native systems-development pilot**.

Validated source acquisition through an internal Debian mirror, dependency provisioning, configure/build with incremental resume, native interpreter execution, extension-module imports, targeted regression tests, C source modification and incremental rebuild, restoration of the source state, and GDB breakpoint/backtrace.

### Foundry PR #306 pilot

Role: **realistic scientific-software pilot**.

Validated PR branch acquisition, CPU PyTorch execution, PR-specific tests, real protein-structure parsing, legacy LigandMPNN checkpoint acquisition, model loading and CPU inference, FASTA/CIF confidence output, B-factor preservation, per-residue confidence semantics, same-seed reproducibility, controlled semantic fault injection and recovery, and a batch-size scaling probe.

## Why the three parts belong together

```text
state and persistence
        ↓
observability and evidence recovery
        ↓
execution and development capability
```

A useful project workspace needs all three: somewhere to work, a way to preserve and reason about evidence, and enough execution capability to validate code rather than only edit it.
