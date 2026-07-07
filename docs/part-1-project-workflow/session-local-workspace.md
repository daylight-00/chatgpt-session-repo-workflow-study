# Session-local workspace

## Observable filesystem

The session exposes a Linux filesystem namespace and permits shell and Python execution against it. The main user-facing workspace used in the experiment was `/mnt/data`.

Observed operations included:

```text
ls
find
mkdir
cp
mv
rm
Python Path.write_text(...)
```

This workspace supports ordinary file lifecycle operations: create, read, overwrite, rename, move, package, and delete.

## Separation from File Library

The session-local workspace is not the File Library.

Observed behavior:

- creating a file under `/mnt/data` did not by itself make it searchable in File Library;
- deleting a promoted file from `/mnt/data` did not remove the persistent Library object;
- overwriting a local path did not mutate the already promoted Library snapshot.

The most useful operational distinction is:

```text
session-local file = mutable runtime object
Library artifact    = persistent promoted object/snapshot
```

## Tool surface

Two execution modes were used:

- shell execution through the container runtime;
- visible Python execution through `python_user_visible.exec`.

Neither tool, by file creation alone, caused File Library registration. A path printed in tool stdout also did not trigger promotion.

## Practical role

The local workspace is suitable for generating reports, code, data, and archives; using shell utilities and Python for transformation; preparing files before artifact handoff; and assembling deterministic repository payloads.

It should be treated as ephemeral runtime storage unless a file is explicitly handed off to a persistent user-facing surface. Later runtime-reset observations strengthen this conclusion; see Part III `runtime-lifecycle.md`.
