# Foundry PR #306 rerun compact evidence index

This directory records compact anchors for the complete rerun evidence bundle. The full raw tree is intentionally kept outside Git history.

## Main archive

```text
name
foundry_pr306_rerun_20260707.tar.zst

size_bytes
422189189

sha256
ff0a2aac97c04aa8f0c785a25ea43dcb275b4678c852b84c88c88a94f2d431f9
```

## Verification sidecar

```text
name
foundry_pr306_rerun_archive_verification_20260707.tar.zst

size_bytes
79259

sha256
d4b258fe456761a945245e552cd002b9afe19d1dc2e9a5e3754d32bf8c0428e1
```

## Main archive verification

```text
regular-file manifest       11774 / 11774 verified
inventory entries checked   13263
missing                     0
type mismatches             0
mtime_ns mismatches         0
symlink-target mismatches   0
regular-file size mismatch  0
```

## Drive chunk hashes

```text
acc7279a1a4476ab82d0d2b7549772b633308907195acb8aba18909ae38f176d  part-00
d64f84ac914fa0c34f2a8eb76dc23f119b56cfc30bc2b4d7b5c4011c1ed82968  part-01
22e0756ac2d988b44a1dc2a1dc1c4b4ad9a222c3d1b681ba0fdca17d629e3e6b  part-02
b5386eacc6a8e86b983a0d99d182a22e2090145334910924d51044470fb14e76  part-03
9c5fdf2eb785857a05ab571638a8c01a8041c887aad89302fb48a54fd70128a5  part-04
b1b4bb68f57a1171feb6a07b6b38bdb4cba926496768d5c46e893881fb243693  part-05
44fe152e6c8018ce1b9aab4baf8edc39f9e05cc8f2ef87d83b8c39e708da090c  part-06
```

## Drive round-trip verification

All seven chunks were raw-downloaded from Drive. Their hashes matched the pre-upload chunk manifest. Concatenating the downloaded chunks reproduced SHA-256:

```text
ff0a2aac97c04aa8f0c785a25ea43dcb275b4678c852b84c88c88a94f2d431f9
```

The reassembled archive matched the local archive byte-for-byte and passed Zstandard integrity verification.

## Reproducibility anchors

```text
Foundry branch archive
89da2d9fa3807ea2481b02ae834a0aebc14a3aee8d74375bd352216b3cad1c3c

LigandMPNN repository archive
2bba629e0f2ac6425e5885eabd1a9741ecec4c649afbbd46d5ce011c6650c3fa

1BC8.pdb
d3e844d818f24e8a1b6a39f7d97816306f0b69433ec8b9ea2cb6af6ba08e1284

ligandmpnn_v_32_010_25.pt
161cd264061fda9680cbb940255522ae42f2966c552d045d87913d9452a80970
```

Public Release publication remains pending redistribution review. Backup durability and redistribution permission are treated as separate questions.
