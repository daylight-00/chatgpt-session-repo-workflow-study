# Legacy Part I probe set

This directory retains the original compact Part I probe set and tabular manifests created before the three-part project refactor.

It remains in place as a compatibility evidence location so that existing Git history and references do not need to churn solely for path normalization.

- `probes/` contains text probe files retained from the session-local workspace.
- `manifest.tsv` records experimental role and observed outcome.
- `files.tsv` records filename and byte size.
- `inventory.tsv` records the earlier filesystem inventory.

The retained probe set is small, so complete retention is cheaper and less ambiguous than selective deletion. Raw files alone are not sufficient to reproduce platform behavior; the experiment matrix and narrative reports provide the execution context.

Directory-URI and nonexistent-path probes produced no regular file artifact and are represented only in the manifest.

## Binary probe correction

An earlier version of this README said the PNG and ZIP originals were preserved inside the available tar.gz archive. A later audit found that the currently available early tar.gz snapshot does **not** contain those bytes.

The original tiny PNG and ZIP probe bytes are now preserved in reconstructible text form under:

`evidence/part-1-project-workflow/binary-probes/`

That directory records exact hexadecimal encodings and the original SHA-256 values.

## Future migration

A later cleanup may move the remaining text probes into `evidence/part-1-project-workflow/`. This refactor intentionally keeps them here for compatibility while the new study structure stabilizes.
