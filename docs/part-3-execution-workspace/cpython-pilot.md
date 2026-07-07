# CPython pilot — native build and debug capability

## Role

The CPython experiment is a native systems-development pilot, not a CPython-specific product goal.

## Source provenance

The build used Debian source package state:

```text
package          python3.13
version          3.13.5-2+deb13u2
upstream base    CPython 3.13.5
source state     Debian patch series applied during source-package unpack
```

## Validated workflow

```text
source acquisition
→ dependency installation
→ configure
→ incremental configure recovery/cache reuse
→ make -j4 with incremental resume
→ built interpreter execution
→ native extension imports
→ targeted regression tests
→ C source edit
→ incremental rebuild
→ execution of modified binary
→ source restoration
→ rebuild
→ GDB breakpoint and native backtrace
```

## Key results

- CPython interpreter built and executed successfully.
- Standard extension imports included SSL, SQLite, ctypes, curses, gdbm, bz2, lzma, readline, Tk, zlib, hashlib, decimal, and shared memory.
- Targeted suite: `1,462` tests run, `58` skipped, success.
- A one-file C change in `Python/getversion.c` propagated through incremental rebuild to `python -VV`.
- Incremental rebuild was approximately 12 seconds in the observed run.
- GDB stopped in `Py_GetVersion` with source line and native backtrace.

## Practical interpretation

The experiment established more than theoretical buildability. Edit → rebuild → run → debug loops are practical for targeted native development. Full clean bootstrap is possible but less convenient because long serial configure/build commands can cross execution-call boundaries; cached and incremental workflows are preferable.

## Evidence bundle

The complete pilot archive and independent verification sidecar are tracked in `release-assets/MANIFEST.tsv`.
