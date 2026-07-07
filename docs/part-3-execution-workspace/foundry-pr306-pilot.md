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

## Key results

- PR-specific confidence tests: `6 passed` on the CPU path.
- Test wall time was approximately 27 seconds; peak RSS approximately 1.23 GiB.
- `1BC8.pdb` parsed to a real annotated MPNN input.
- Legacy `ligandmpnn_v_32_010_25.pt` checkpoint was extracted from a cached internal container-registry weight layer.
- Legacy LigandMPNN model loaded successfully on CPU.
- Actual inference completed with FASTA and CIF outputs.
- Observed CPU inference wall time was approximately 27.5 seconds with peak RSS approximately 2.41 GiB.
- FASTA confidence fields and CIF confidence data were verified.
- Common polymer backbone B-factors were preserved exactly in the checked comparison.
- Same-seed FASTA outputs were byte-identical; CIF differences were limited to serialization time metadata.
- A deliberate `exp(+NLL)` semantic fault caused the expected confidence test failure and was then restored to a clean passing state.
- A batch-size 4 probe completed successfully with similar observed peak RSS.

## Environment caveat

The project branch declared Python `>=3.12,<3.13`, while the successful lightweight CPU compatibility path used the session's Python 3.13 + CPU Torch environment. This validates functionality, not exact official-environment parity.

## Evidence and current limitation

The experiment originally produced a complete archive verified against 7,773 file hashes and 9,926 metadata entries. After a later runtime reset, the main archive bytes were no longer available, although its checksum and verification sidecar were rehydrated.

See `runtime-lifecycle.md` and `release-assets/MANIFEST.tsv`.
