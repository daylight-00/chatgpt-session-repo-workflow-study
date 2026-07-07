# Foundry PR #306 pilot — realistic scientific-software work

## Role

This experiment tests whether the sandbox can support realistic work close to the user's actual research profile: protein-design software, PyTorch, structure parsing, model checkpoints, scientific output validation, tests, and debugging.

Target case: RosettaCommons/foundry PR #306, which added LigandMPNN/ProteinMPNN confidence metrics and propagated them to inference outputs.

## Validated workflow

```text
PR branch acquisition
→ Python execution environment
→ CPU PyTorch path
→ minimal dependency closure
→ PR-specific tests
→ real PDB parsing
→ checkpoint provenance and extraction
→ legacy LigandMPNN model load
→ CPU inference
→ FASTA/CIF verification
→ B-factor preservation check
→ per-residue confidence semantics check
→ same-seed reproducibility
→ controlled semantic fault injection
→ expected test failure
→ source restoration and passing tests
→ batch-size scaling probe
```

## Original pilot result

- PR-specific confidence tests: `6 passed` on the CPU path.
- `1BC8.pdb` parsed to a real annotated MPNN input.
- Legacy `ligandmpnn_v_32_010_25.pt` checkpoint was extracted from a cached internal container-registry weight layer.
- Legacy LigandMPNN model loaded successfully on CPU.
- Actual inference completed with FASTA and CIF outputs.
- FASTA confidence fields and CIF confidence data were verified.
- Common polymer backbone B-factors were preserved exactly in the checked comparison.
- Same-seed FASTA outputs were byte-identical; CIF differences were limited to serialization time metadata.
- A deliberate `exp(+NLL)` semantic fault caused the expected confidence test failure and was then restored to a clean passing state.
- A batch-size 4 probe completed successfully.

The original run produced a complete archive verified against 7,773 file hashes and 9,926 metadata entries. That archive later disappeared during a container reset; its exact bytes remain unrecoverable, while its checksum and verification sidecar survived.

## Recovery-by-rerun

A complete rerun was performed after the runtime-loss event, using the same PR branch, the same `1BC8.pdb` example, and a checkpoint reacquired through the same internal container-registry provenance path.

### Source, input, and checkpoint anchors

```text
Foundry branch archive SHA-256
89da2d9fa3807ea2481b02ae834a0aebc14a3aee8d74375bd352216b3cad1c3c

LigandMPNN repository archive SHA-256
2bba629e0f2ac6425e5885eabd1a9741ecec4c649afbbd46d5ce011c6650c3fa

1BC8.pdb SHA-256
d3e844d818f24e8a1b6a39f7d97816306f0b69433ec8b9ea2cb6af6ba08e1284

ligandmpnn_v_32_010_25.pt SHA-256
161cd264061fda9680cbb940255522ae42f2966c552d045d87913d9452a80970
```

### Rerun functional results

- Targeted confidence tests: `6 passed`, `16 deselected`.
- Real data path: `2,275` atoms and `115` tokens.
- Model parameter count: `2,618,501`.
- Thread-capped model/data probe: model load approximately `0.069 s`; data parse approximately `3.058 s`.
- Actual legacy LigandMPNN CPU inference succeeded and produced FASTA/CIF outputs.
- FASTA output contained global and ligand-interface confidence values.
- Raw CIF confidence range: `0.0` to approximately `0.92755`.
- DNA and ZN rows carried zero confidence values in the checked output.
- Protein designed rows carried positive confidence values.
- Common protein-backbone B-factor comparison: maximum absolute difference `0.0`.
- Same-seed FASTA outputs were byte-identical; CIF differences were limited to the `_entry.time` serialization field.
- Batch size 4 completed successfully.
- The deliberate `exp(+NLL)` semantic fault again produced the expected test failure and was followed by source restoration and a clean passing test run.

### Thread oversubscription finding

The first successful rerun inference used unconstrained thread settings and completed in approximately `4 min 43 s`, with peak RSS about `2.63 GiB`. Repeating the same-seed inference with a four-thread cap reduced wall time to approximately `11.1 s`, with peak RSS about `2.33 GiB`, while preserving scientific output semantics. Batch size 4 with the same thread cap completed in approximately `11.5 s`.

This is a practical runtime finding: visible CPU count should not be translated directly into unconstrained numerical-library parallelism in the approximately 4 GiB sandbox.

## Environment caveat

The project branch declared Python `>=3.12,<3.13`, while the successful lightweight CPU compatibility path used Python 3.13.5 with Torch 2.10.0+cpu. This validates functionality, not exact official-environment parity.

## Rerun raw evidence bundle

The rerun produced a new complete raw archive:

```text
asset
foundry_pr306_rerun_20260707.tar.zst

size
422,189,189 bytes

SHA-256
ff0a2aac97c04aa8f0c785a25ea43dcb275b4678c852b84c88c88a94f2d431f9
```

Archive verification result:

```text
regular-file manifest       11,774 / 11,774 verified
inventory entries checked   13,263
missing                     0
type mismatches             0
mtime_ns mismatches         0
symlink-target mismatches   0
regular-file size mismatch  0
```

The archive was then split into seven chunks of at most 64 MiB, uploaded to Google Drive, raw-downloaded, hash-checked individually, reassembled, and verified against the original archive. The reassembled archive matched the local archive byte-for-byte and passed Zstandard integrity verification.

The original lost archive remains historically unrecoverable. The rerun is a new, independently verified evidence bundle rather than an exact reconstruction of the original bytes.

See `runtime-lifecycle.md`, the rerun experiment record, and `release-assets/MANIFEST.tsv`.
