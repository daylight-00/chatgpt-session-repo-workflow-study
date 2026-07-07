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

## Original recovery assessment

Preserved Foundry evidence:

- expected archive SHA-256: `5c0ce3f241c653bfc1951b7f8823b610d6fa629f0b8fd0e9836df06922539a44`;
- archive verification sidecar;
- source-manifest verification record: `7,773/7,773` files verified;
- extracted-manifest verification record: `7,773/7,773` files verified;
- metadata comparison: `9,926` entries, zero missing/type/mtime_ns/symlink-target mismatches.

Missing:

- original main archive bytes;
- original full Foundry experiment tree;
- the original main tree's standalone manifest and inventory files in the reset runtime.

The sidecar proves that the original archive existed and verified successfully at creation time. It does not contain enough content data to reconstruct the exact original archive bytes.

Therefore the historical state remains:

```text
original exact-byte recovery     not possible from retained evidence
historical verification          preserved
semantic reconstruction          possible by rerunning the pilot
```

## Successful semantic reconstruction by rerun

The pilot was subsequently rerun from reacquired source/input/checkpoint provenance. The rerun reproduced the key functional behavior, including targeted tests, real CPU LigandMPNN inference, confidence output semantics, B-factor preservation, same-seed determinism semantics, batch-size 4 execution, and fault-injection detection/restoration.

The rerun produced a new complete archive:

```text
foundry_pr306_rerun_20260707.tar.zst
size:   422,189,189 bytes
sha256: ff0a2aac97c04aa8f0c785a25ea43dcb275b4678c852b84c88c88a94f2d431f9
```

Verification covered `11,774` regular-file hashes and `13,263` inventory entries, with zero missing/type/mtime_ns/symlink-target/regular-file-size mismatches.

For off-runtime durability, the archive was split into seven chunks of at most 64 MiB and uploaded to Google Drive. All seven chunks were then raw-downloaded, hash-verified, reassembled, and checked against the local archive. The result was byte-for-byte identical and passed Zstandard integrity verification.

This does not recover the original lost bytes. It establishes a new independently verified evidence bundle and demonstrates that semantic recovery of the pilot is practical when source/input/checkpoint provenance is available.

## Policy implication

The runtime filesystem must be treated as ephemeral. Critical evidence should be persisted per object, and large archives should have independently persisted checksums and verification sidecars. Long-running experiments should checkpoint compact evidence during execution.

The rerun adds a stronger operational rule:

```text
experiment completes
→ compact checkpoint immediately
→ full archive and verification
→ chunk if connector limits require it
→ off-runtime upload
→ raw-download round trip
→ chunk hash check
→ reassembly and final archive hash check
```

Release readiness and public redistribution remain separate from backup durability. The rerun bundle is durably backed up, while public publication still requires redistribution review.
