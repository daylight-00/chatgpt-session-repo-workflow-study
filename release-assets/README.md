# Release asset policy

Large experiment bundles are kept out of Git history.

`MANIFEST.tsv` records expected asset name, experiment role, byte size when known, SHA-256 when known, current availability, verification state, and redistribution-review state.

A historical checksum alone is not enough to mark an asset Release-ready. The actual bytes must be available and reverified before publication.
