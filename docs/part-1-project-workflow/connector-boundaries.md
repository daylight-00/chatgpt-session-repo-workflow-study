# Connector boundaries: representation and capability

## Why this belongs in Part I

Two additional experiments performed in separate sessions clarified two different connector boundary classes.

```text
Google Drive upload
  desired upload action exists
  but a raw local path is not sufficient input
  → file-handoff / representation boundary

GitHub Release creation
  repository access and admin permission exist
  but the connector exposes no direct Release mutation primitive
  → connector capability-surface boundary
```

The shared lesson is:

```text
provider capability
!= connector action availability
!= argument representation compatibility
```

## Layer model

A practical connector workflow should distinguish:

```text
session runtime object
        ↓
runtime handoff / materialization
        ↓
connector-compatible file or object reference
        ↓
connector-exposed action surface
        ↓
provider API and resource permissions
```

A failure at one layer should not be attributed automatically to another.

## Case A — Google Drive file handoff

A file could exist at `/mnt/data/...` while direct use of that path as the Drive upload/import input failed.

The separate-session experiment directly observed that:

- a raw path string was not accepted as a sufficient import source;
- `upload_file.file_uri` required a connector-compatible file reference;
- a file materialized through a connector fetch/download path was accepted by the upload action.

Therefore:

```text
file exists in sandbox
        !=
connector can receive that file
```

A conservative semantic model is:

```text
LOCAL_PATH_ONLY
        ↓ materialize / resolve
RUNTIME_MANAGED_FILE_ASSET
        ↓ handoff
CONNECTOR_FILE_REFERENCE
        ↓ validation
UPLOAD_READY_PAYLOAD
        ↓ provider request
DRIVE_FILE_CREATED
```

The exact internal implementation remains unobserved.

### Subsequent project corroboration

Later project work used Drive as emergency backup storage. Session-generated artifacts had to become runtime-managed file references before the Drive upload action accepted them. The project also observed an approximately 100 MiB connector relay ceiling for a single handed-off object, motivating deterministic chunking of larger archives.

This later project observation is separate from the external-session report but consistent with the same representation-boundary model.

## Case B — GitHub Release capability

The tested private repository was visible to the connector and the authenticated identity had repository admin permission. However, direct Release creation was unavailable because the observed connector tool surface did not expose:

```text
create Release
upload Release asset
explicit generic tag creation
workflow dispatch
```

No existing release workflow or manual dispatch configuration was found in the test repository.

The correct conclusion is:

```text
repository visibility       yes
repository admin permission yes
direct Release primitive    not exposed
```

This is a finding about the connector surface observed in that session, not about GitHub provider capability in general.

### Indirect path

A repository-native automation path remains plausible:

```text
connector-supported repository mutation
        ↓
repository event
        ↓
GitHub Actions workflow
        ↓
tag / Release / asset operations
```

This path was not executed in the external-session assessment and remains unverified.

## Failure taxonomy

| Boundary | Typical evidence | Correct interpretation |
|---|---|---|
| Resource visibility | repository or file not found | installation scope or resource access |
| Authorization | provider permission denial | permission problem |
| Representation | path/reference validation error | handoff or argument-shape problem |
| Connector capability | desired action absent | capability-surface limitation |
| Provider API | provider-originated request error | provider request semantics |
| Repository automation | workflow missing or not triggerable | indirect path unavailable |

## Operational consequences for this project

### Emergency durability

Google Drive is the verified emergency preservation layer for large experiment archives:

```text
experiment artifact
→ transferable runtime asset
→ chunk if required
→ Drive upload
→ raw download
→ hash verification
→ reassembly
→ final archive verification
```

### Release publication

The GitHub connector should not be assumed to create Releases directly. Until a direct primitive or validated repository-native automation path exists, Release publication remains a separate operational step from raw evidence backup.

```text
Drive
  immediate durable backup and recovery validation

Git repository
  narrative, methods, compact evidence, manifests

GitHub Release
  curated publication after redistribution review and a supported publication path
```

## General rule

> Verify resource access, authorization, representation compatibility, and exposed connector capability separately.
