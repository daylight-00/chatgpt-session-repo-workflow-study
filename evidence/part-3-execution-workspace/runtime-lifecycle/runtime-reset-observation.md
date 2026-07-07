# Runtime lifecycle findings

## Scope

This note records an unexpected runtime reset observed while preparing the project refactor. It distinguishes direct observations from inference.

## Observed facts

At `2026-07-07T11:51:05+00:00`:

- PID 1 reported start time `2026-07-07 11:27:01 UTC`.
- `/proc/uptime` was about 1,445 seconds (~24 minutes).
- PID 1 was a fresh `supervisord` process.
- `gdb` was no longer installed.
- development packages previously installed during the CPython pilot were no longer present according to `dpkg-query`.
- `/mnt/data/cpython-workspace`, the original CPython working tree used during the pilot, was absent.
- `/mnt/data/foundry_pr306_workspace_probe_20260707.tar.zst` was absent.
- `/mnt/data/cpython_workspace_pilot_20260707.tar.zst` was present and had been recreated at approximately `11:27 UTC`.
- the Foundry main-archive checksum file and archive-verification sidecar were present.
- the rehydrated directories `foundry_pr306_workspace_probe_20260707/` and `cpython_workspace_pilot_20260707/` contained only their individually handed-off `README.md` files, not their original full directory trees.
- many other individually handed-off `/mnt/data` files were recreated with timestamps clustered around `11:27 UTC`.
- File Library search for the missing Foundry main-archive filename and SHA-256 returned no result in the available search surface.

## Strong inference

The observations are consistent with:

```text
container runtime reset
        +
selective rehydration of persisted artifact objects
```

They are not consistent with a continuously running container whose filesystem merely lost one large file.

The most important distinction is therefore:

```text
runtime filesystem lifetime
!=
artifact persistence lifetime
```

A file or directory path that once existed in `/mnt/data` is not, by itself, durable evidence. Durability depends on successful persistence/handoff of the individual artifact object.

## Foundry archive recovery assessment

Known retained evidence:

- expected main archive filename;
- expected SHA-256:
  `5c0ce3f241c653bfc1951b7f8823b610d6fa629f0b8fd0e9836df06922539a44`;
- archive verification sidecar;
- source-manifest verification log reporting `7,773/7,773` files verified;
- extracted-manifest verification log reporting `7,773/7,773` files verified;
- metadata comparison reporting `9,926` entries checked with zero missing/type/mtime_ns/symlink-target mismatches.

Missing:

- the main archive bytes;
- the original full Foundry experiment tree;
- the original `MANIFEST.sha256` and `FILE_INVENTORY.tsv` as standalone files in the current runtime.

The verification logs contain successful path-by-path checks, but they do not contain enough content data to reconstruct the exact lost archive bytes. Therefore:

```text
exact-byte recovery        not currently possible
semantic reconstruction   possible by rerunning the pilot
historical verification   preserved by sidecar
release readiness         blocked until a main archive is rebuilt/restored
```

## Implication for project policy

The project should record runtime lifecycle as a first-class constraint:

1. treat the shell/container filesystem as ephemeral;
2. persist critical outputs explicitly and early;
3. do not assume that a referenced directory implies recursive persistence;
4. verify that each intended Release asset exists as a durable artifact object;
5. retain small checksums/manifests/verification sidecars independently from large archives;
6. for long experiments, checkpoint compact evidence during the experiment rather than only at the end.
