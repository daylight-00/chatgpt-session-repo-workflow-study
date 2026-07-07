# Foundry PR #306 pilot rerun

## Reason

The original complete Foundry pilot archive was lost during a container reset. Its checksum and verification sidecar survived, but the exact archive bytes were not recoverable. This rerun tests whether the pilot can be semantically reconstructed from reacquired source, input, checkpoint provenance, and documented procedure.

## Procedure

```text
reacquire PR branch archive
→ reacquire LigandMPNN example input repository
→ recover 1BC8.pdb
→ reacquire legacy LigandMPNN checkpoint from cached container weight layer
→ rebuild lightweight CPU execution environment
→ run targeted confidence tests
→ probe model load and real input parsing
→ run real CPU inference
→ verify FASTA/CIF confidence semantics and B-factor preservation
→ repeat same seed under thread cap
→ run batch-size 4 probe
→ inject semantic confidence bug
→ confirm expected test failure
→ restore source and confirm passing tests
→ checkpoint compact raw evidence to Drive
→ freeze complete tree
→ manifest/inventory
→ archive
→ full extraction verification
→ split archive into seven chunks
→ upload to Drive
→ raw-download all chunks
→ verify chunk hashes
→ reassemble and verify final archive SHA-256 and Zstandard integrity
```

## Key results

- targeted confidence tests: 6 passed;
- real input: 2,275 atoms, 115 tokens;
- model parameters: 2,618,501;
- thread-capped model load: ~0.069 s;
- thread-capped input parse: ~3.058 s;
- unconstrained-thread inference: ~4 min 43 s, peak RSS ~2.63 GiB;
- four-thread same-seed inference: ~11.1 s, peak RSS ~2.33 GiB;
- batch size 4 with four-thread cap: ~11.5 s;
- FASTA same-seed output byte-identical;
- CIF differences limited to serialization time metadata;
- confidence semantics and B-factor preservation verified;
- semantic fault injection produced expected failure and clean repair.

## Raw archive

```text
foundry_pr306_rerun_20260707.tar.zst
size:   422,189,189 bytes
sha256: ff0a2aac97c04aa8f0c785a25ea43dcb275b4678c852b84c88c88a94f2d431f9
```

Full verification:

```text
manifest files            11,774 / 11,774 verified
inventory entries checked 13,263
missing                   0
type mismatch             0
mtime_ns mismatch         0
symlink-target mismatch   0
regular-file size mismatch 0
```

## Preservation result

The main archive is backed up in seven Drive chunks of at most 64 MiB. The chunks were independently raw-downloaded, hash-verified, reassembled, and compared with the local main archive. Result: byte-for-byte match and Zstandard integrity pass.

The rerun is a new evidence object. It does not recreate the exact bytes of the original lost archive.
