# Sandbox runtime characterization

## Observed baseline

The runtime profile observed during the execution study included:

```text
OS              Debian GNU/Linux 13 (trixie)
Architecture    x86_64
glibc           2.41
visible CPUs    56 logical CPUs
memory          approximately 4 GiB
swap            0
GPU device      no /dev/dri exposed
```

Available build tools included GCC, Clang, GNU Make, CMake, Ninja, pkg-config, Git, Python, Node.js, Go, Java, Swift, and Kotlin. Initial probes showed that GCC, Clang, G++, and Clang++ could compile, link, and run native binaries.

Evidence pointers:

- `evidence/part-3-execution-workspace/runtime-profile/README.md`
- `evidence/part-3-execution-workspace/runtime-profile/build_capability_probe.log`

## Interpretation

The environment should not be modeled as a GPU workstation. It is better modeled as a CPU-only, memory-constrained native build container with broad compiler availability and selective internal package mirrors.

The 56 visible logical CPUs should not be translated directly into `-j56`: the observed memory budget and lack of swap make conservative parallelism preferable for large native builds.
