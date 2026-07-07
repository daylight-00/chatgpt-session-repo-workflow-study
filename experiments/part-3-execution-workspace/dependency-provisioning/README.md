# Dependency-provisioning experiment

## Objective

Determine whether the sandbox can supplement missing dependencies and distinguish theoretical installability from practical usability.

## Candidate paths explored

- uv and internal PyPI mirror;
- apt and internal Debian binary/source mirrors;
- micromamba/Conda environment concepts;
- private-prefix source builds;
- Meson wraps and local package caches;
- Conan manager plus offline/private cache concepts;
- Spack;
- Nix.

## Key finding

The shell's public-network restrictions do not imply a completely offline environment. Internal mirrors for several ecosystems made practical bootstrap possible. The CPython and Foundry pilots then validated this conclusion using project-specific dependency paths rather than only package-manager smoke tests.
