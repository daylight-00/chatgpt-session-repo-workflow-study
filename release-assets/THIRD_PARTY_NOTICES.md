# Third-party materials and publication review

The evidence Release contains original study records together with materials originating from upstream software projects, package distributions, model parameters, and public scientific data.

This notice records the publication review performed for the planned `Study Evidence Bundle v1.0` asset set.

## CPython pilot

The CPython pilot bundle contains Debian-packaged CPython source, source-package inputs, and build state.

The public bundle retains:

- the upstream CPython `LICENSE` file;
- Python documentation license and acknowledgement material in the source tree;
- Debian `debian/copyright` metadata;
- component-specific license and copyright material present in the source tree.

The release does not claim ownership of CPython, Debian packaging material, or incorporated third-party components. Those materials remain subject to their respective terms.

## Foundry rerun pilot

The public-safe Foundry rerun bundle contains:

- Foundry source under the BSD 3-Clause license included with the source tree;
- a LigandMPNN source archive containing its MIT license;
- the LigandMPNN model checkpoint used by the pilot; the upstream LigandMPNN README states that its code and model parameters are available under the MIT license;
- the `1BC8.pdb` example structure input; PDB archive data are made available under CC0 1.0 by the wwPDB, which encourages attribution to original structure authors where possible;
- a captured Python package environment.

Before publication, the Foundry bundle was remediated for public release:

1. credential-bearing package-registry URL userinfo in the environment capture was redacted;
2. `env/PACKAGE_LICENSE_INDEX.tsv` was added, covering all 50 captured Python distributions and pointing to retained license/notice files;
3. fallback license texts were added for `antlr4-python3-runtime==4.9.3` and `Cython==3.2.8`, whose installed `.dist-info` directories did not contain obvious standalone license files;
4. the archive, internal manifests, metadata inventory, and verification sidecar were regenerated and re-verified.

Package materials remain subject to their individual licenses. License and notice files are retained inside the package snapshot and indexed by `env/PACKAGE_LICENSE_INDEX.tsv`.

## Publication scope decision

The public Release asset set is limited to the five evidence archives listed in `RELEASE_ASSETS.tsv` plus `SHA256SUMS`, `RELEASE_ASSETS.tsv`, and this notice.

The following are not part of the public asset set:

- the unavailable historical original Foundry main archive;
- its historical verification sidecar;
- early historical repository snapshots as standalone Release assets;
- Drive transport chunks and reassembly helper objects.

## Verification note

Redistribution review and technical verification are separate controls. Publication should use only the hashes recorded in the accompanying `SHA256SUMS` and `RELEASE_ASSETS.tsv`. In particular, the public-safe Foundry archive and sidecar hashes replace the earlier pre-review Foundry rerun hashes.
