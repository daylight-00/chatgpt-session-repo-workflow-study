# Part III — Active execution workspace

## Question

Can the sandbox function as an active software-development workspace rather than only a file-editing and repository-management surface?

## Investigation sequence

```text
runtime characterization
        ↓
dependency provisioning exploration
        ↓
CPython native build/debug pilot
        ↓
Foundry PR #306 scientific-software pilot
```

## Why two pilots

The pilots validate different failure modes.

### CPython

Validates native systems-development capability: configure, compile, link, test, edit C, incrementally rebuild, restore, and debug with GDB.

### Foundry PR #306

Validates realistic scientific-software work: Python package closure, CPU PyTorch, biomolecular structure parsing, model checkpoint acquisition, real LigandMPNN inference, scientific output validation, reproducibility, fault injection, and recovery.

## Overall result

The sandbox can serve as an active development workspace for targeted workflows. Short edit/test/debug loops are practical. Clean bootstrap of large dependency graphs and long monolithic commands are less convenient and should use cached, incremental, or checkpointed workflows.

The runtime filesystem itself is not durable; see `runtime-lifecycle.md`.
