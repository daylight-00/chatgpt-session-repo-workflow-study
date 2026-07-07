# Runtime lifecycle and selective artifact rehydration

## Trigger

During project refactoring, the Foundry main experiment archive was found to be missing from `/mnt/data`. A runtime continuity check was performed before assuming that a single large file had been selectively deleted.

## Observed facts

At `2026-07-07T11:51:05+00:00`:

- PID 1 had started at `2026-07-07 11:27:01 UTC`;
- `/proc/uptime` was approximately 24 minutes;
- previously installed GDB was absent;
- development packages installed during the CPython experiment were absent;
- the original `/mnt/data/cpython-workspace` tree was absent;
- the Foundry main `.tar.zst` archive was absent;
- the CPython main `.tar.zst` archive was present;
- Foundry checksum and verification-sidecar artifacts were present;
- directories whose README files had been individually handed off were re-created but contained only those README files, not their former recursive trees;
- many other individually handed-off files were recreated with timestamps clustered around the new runtime start.

File Library search for the missing Foundry archive filename and expected SHA-256 returned no result in the available search surface.

## Conservative model

The observations strongly support:

```text
container reset
        +
selective rehydration of persisted artifact objects
```

They do not support the stronger claim that the platform deletes all files above a specific size threshold. Size may be relevant, but the experiment did not isolate it.

## Recovery assessment

Preserved Foundry evidence:

- expected archive SHA-256: `5c0ce3f241c653bfc1951b7f8823b610d6fa629f0b8fd0e9836df06922539a44`;
- archive verification sidecar;
- source-manifest verification record: `7,773/7,773` files verified;
- extracted-manifest verification record: `7,773/7,773` files verified;
- metadata comparison: `9,926` entries, zero missing/type/mtime_ns/symlink-target mismatches.

Missing:

- main archive bytes;
- original full Foundry experiment tree;
- the main tree's standalone manifest and inventory files in the current runtime.

The sidecar proves that the archive existed and verified successfully at creation time. It does not contain enough content data to reconstruct the exact archive bytes.

Therefore:

```text
exact-byte recovery        not currently possible
semantic reconstruction   possible by rerunning the pilot
historical verification   preserved
Release readiness         blocked for the Foundry main asset
```

## Policy implication

The runtime filesystem must be treated as ephemeral. Critical evidence should be persisted per object, and large archives should have independently persisted checksums and verification sidecars. Long-running experiments should checkpoint compact evidence before final packaging.
