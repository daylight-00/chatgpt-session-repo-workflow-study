# Method and evidence policy

## Observation versus inference

This study separates direct observation from inference.

Direct observations include command exit status, file hashes, test counts, GitHub API results, PID start time, and path presence or absence. Inferences include explanations of undocumented platform selection behavior, the cause of missing artifacts, and internal persistence architecture.

Inference is used only when the supporting observations are recorded and the statement is labeled conservatively.

## Evidence hierarchy

The project uses four evidence levels.

1. **Narrative documentation** in `docs/`: questions, methods, interpretation, and conclusions.
2. **Experiment records** in `experiments/`: procedures, matrices, timelines, commands, and experiment-specific interpretation.
3. **Compact evidence** in `evidence/`: small probes, selected raw logs, environment records, checksums, and reconstructible encodings suitable for Git history.
4. **Large evidence bundles** tracked in `release-assets/MANIFEST.tsv`: complete source/build trees, environments, full logs, large outputs, and verification sidecars intended for Releases rather than Git history.

## Runtime persistence policy

The runtime filesystem is treated as ephemeral.

A later runtime reset demonstrated that:

```text
container lifetime
!=
artifact lifetime
```

Some independently persisted artifacts were rehydrated after reset, while original build trees, installed packages, and one large Foundry archive were not.

Therefore:

1. critical outputs should be handed off explicitly and early;
2. persistence should be verified per object, not assumed recursively from a directory reference;
3. checksums and verification sidecars should be stored independently from large archives;
4. long experiments should checkpoint compact evidence during execution;
5. Release readiness requires verifying that the actual asset bytes remain accessible, not merely that a historical checksum exists.

## Archive verification policy

Preferred complete-pilot procedure:

```text
freeze experiment tree
→ create FILE_INVENTORY.tsv
→ create MANIFEST.sha256
→ verify source tree
→ create PAX tar stream
→ compress with Zstandard
→ hash archive
→ extract to a new path
→ verify content hashes
→ compare type / mtime_ns / symlink targets
→ preserve verification logs as a sidecar
```

## Redistribution policy

Technical reproducibility and public redistribution are separate questions. Before publishing Release assets containing third-party checkpoints, runtimes, source archives, or binary environments, licensing and redistribution status must be reviewed explicitly.
