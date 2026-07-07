# Experiment artifacts

Raw probe files produced during the session are retained here as compact evidence.

They are intentionally separated from the narrative report:

- `probes/` contains files that existed in the session-local workspace.
- `manifest.tsv` records the experimental role and observed outcome.
- `files.tsv` records filename and byte size.

The retained probe set is small (19 files, 806 bytes before filesystem and archive overhead), so complete retention is cheaper and less ambiguous than selective deletion. Raw files alone are not sufficient to reproduce platform behavior; the manifest and reports provide the necessary execution context.

Directory-URI and nonexistent-path probes produced no regular file artifact and are represented only in `manifest.tsv`.

## Repository versus archive

Text probe artifacts are committed directly. The PNG and ZIP probes are preserved as original bytes in the tar.gz archive; a direct binary `create_blob` connector call was blocked by the platform safety layer in this session. This is recorded as an observed connector-path limitation, not as a GitHub API limitation.
