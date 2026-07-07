# Runtime-profile evidence

This directory tracks the initial sandbox specification and build-capability probe.

## Raw sandbox specification

```text
file: session_sandbox_specs_raw_20260707.txt
size: 129108 bytes
sha256: fac77afd782ce8b7e7aa6473b415d23b3d777ebecc128fe66f963916749fc429
```

The raw 129 KiB profile was handed off as a session artifact. The Git repository keeps the checksum and interpretation while avoiding unnecessary duplication of a large environment dump in contents-API history.

## Build-capability probe

```text
file: build_capability_probe.log
size: 2710 bytes
sha256: b64185ae0bd3b54f24c3cae3a5384cf12ee318bf022d503b768429daa1287da6
```

The concise build probe is committed directly because it captures compiler smoke tests and initial missing-dependency state.
