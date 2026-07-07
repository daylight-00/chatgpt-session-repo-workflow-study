# Release asset policy

Large experiment bundles stay out of Git history and are published separately as evidence assets.

`MANIFEST.tsv` records the complete known large-asset inventory, including historical objects that are not planned for publication. `RELEASE_ASSETS.tsv` records the narrower planned public asset set, and `SHA256SUMS` records expected hashes.

## Preservation and publication are separate stages

The project preserves valuable raw evidence off-runtime first, verifies recovery, and only then prepares a curated evidence publication.

The off-runtime backup path was verified end-to-end for the CPython and Foundry rerun bundles. Drive chunk files are backup transport objects; the public evidence publication should use the original reassembled single-file archives.

## Planned evidence publication

Tag: `study-v1.0.0`

Title: `Study Evidence Bundle v1.0`

Primary assets:

- complete CPython pilot archive;
- CPython verification sidecar;
- complete Foundry rerun archive;
- Foundry rerun verification sidecar;
- supplemental study-level raw evidence bundle;
- `SHA256SUMS`;
- `RELEASE_ASSETS.tsv`;
- `THIRD_PARTY_NOTICES.md`.

Historical early snapshots and Drive chunk transport files are not part of the primary public asset set.

## Publication procedure

1. Confirm the final cleanup is merged into `main`.
2. Confirm the tag target.
3. Create the publication as a draft.
4. Upload the planned assets.
5. Download the draft assets again.
6. Compare filenames and sizes with `RELEASE_ASSETS.tsv` and verify hashes with `SHA256SUMS`.
7. Confirm archive integrity and review the verification sidecars.
8. Review third-party notices and final publication scope.
9. Publish only after all checks pass.

After publication, repeat a public download and checksum verification once more.
