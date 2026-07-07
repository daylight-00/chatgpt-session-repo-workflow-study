# Runtime lifecycle experiment

## Trigger

The Foundry PR #306 main pilot archive unexpectedly disappeared from `/mnt/data` during later project-refactor work. Rather than assuming size-based deletion, the runtime was inspected for continuity.

## Checks performed

- PID 1 start time and `/proc/uptime`;
- PID 1 process identity;
- presence of GDB installed during the CPython pilot;
- presence of native development packages installed during the CPython pilot;
- presence of original CPython and Foundry workspaces;
- presence of each pilot main archive, checksum, and verification sidecar;
- contents of directories whose README files had been individually handed off;
- timestamp clustering of reappearing `/mnt/data` artifacts;
- File Library search for the missing Foundry archive filename and SHA-256.

## Result

The observations support a runtime-reset plus selective-artifact-rehydration model rather than a continuously running container that lost only one file.

```text
container runtime reset
        +
selective rehydration of persisted artifact objects
```

Exact internal platform mechanics remain unobserved.

## Consequence

Runtime continuity and artifact persistence must be tested separately. The result changed the project evidence policy: critical outputs should be persisted as individual durable objects, and long experiments should preserve compact evidence before final packaging.
