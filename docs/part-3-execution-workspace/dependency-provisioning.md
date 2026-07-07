# Dependency provisioning

## Question

Can missing build and scientific-software dependencies be provisioned inside the sandbox despite restricted public shell networking?

## Network topology observed

The environment was not simply offline.

```text
public shell DNS / direct public endpoints    restricted
internal PyPI mirror                         usable
internal Debian binary mirror                usable
internal Debian source mirror                usable
internal GitHub/VCS cache                     selectively usable
internal container registry cache            usable for cached images/layers
```

## Provisioning approaches explored

### uv

`uv` was already installed. It successfully created environments and installed Python-side build tools such as Meson, Mako, PyYAML, and Packaging through the internal PyPI mirror.

### apt through internal Debian mirrors

The public Debian endpoint was unreachable from the shell, but internal Debian mirrors supported both binary packages and source packages. This path enabled CPython source acquisition and native development dependencies.

### Conda / micromamba

A single-file manager is structurally attractive, but package-channel access was not demonstrated in the restricted shell network. A pre-packed environment or pre-staged package cache remains a plausible approach.

### Private-prefix source builds

This is a general fallback for native dependencies and is especially suitable for small dependency closures.

### Meson wraps

Useful for projects that support fallback subprojects and pre-staged source archives, but not a universal replacement for system dependencies.

### Conan

The manager itself could be installed through the internal Python package path, but the default public remote was not reachable. Local recipes or an internal/offline cache remain possible.

### Spack and Nix

Technically plausible but operationally heavy for the studied use cases unless mirrors or build caches are pre-staged.

## Conclusion

Dependency provisioning is practical when the environment's internal mirrors are discovered and used deliberately. The main constraint is not compiler availability but acquisition topology and bootstrap cost.
