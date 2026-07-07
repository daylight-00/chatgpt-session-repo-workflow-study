# Runtime-profile experiment

This experiment characterized the session sandbox before attempting realistic builds.

Questions:

1. What hardware-visible resources are exposed?
2. Which compilers and build systems are installed?
3. Can C and C++ programs compile, link, and execute?
4. Is GPU device access exposed?
5. What constraints matter for later build experiments?

Raw and compact evidence is indexed under:

`evidence/part-3-execution-workspace/runtime-profile/`

The key result was a broad native toolchain on an x86_64 Debian runtime with approximately 4 GiB visible memory, no swap, and no exposed GPU device node.
