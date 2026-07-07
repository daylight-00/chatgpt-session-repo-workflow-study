# Google Drive connector file-handoff experiment

## Provenance

This report summarizes a low-level experiment performed in a separate session and later incorporated into the project.

Original report:

```text
chatgpt_drive_connector_file_handoff_low_level_en.md
sha256 5e5de0d262468a8e215aa43c86c6e3e2c3f52257aff7623dce5ac74be4a00af7
```

The original Markdown report is backed up in the project's canonical Google Drive backup folder.

## Question

Why can a file exist at `/mnt/data/...` but still fail when passed to a Google Drive connector upload/import action?

## Direct observations

1. Passing a raw local path to the document import action failed validation.
2. Passing a raw local path to `upload_file.file_uri` failed with an error requiring a connector file reference.
3. A file materialized through a connector fetch/download path was later accepted by `upload_file`.
4. The resulting Drive file was created with the intended plain-text MIME type.

## Conclusion

The failure occurred at the file-handoff boundary, not at the Google Drive provider API layer.

```text
raw filesystem path
    !=
connector-transferable file reference
```

The successful path is modeled conservatively as:

```text
local or fetched bytes
→ runtime-managed transferable file asset
→ connector-compatible file reference
→ Drive upload
```

The exact internal mechanism is not observable and should not be described more specifically than the evidence permits.

## Project relevance

Later project work independently confirmed that Drive upload requires a transferable runtime artifact/reference rather than an arbitrary local path string. The project subsequently used this path for emergency archive backup, with deterministic chunking for objects that exceeded the observed connector relay limit.

## Operational rule

When uploading session-generated files to Drive:

1. do not assume that path existence implies connector readability;
2. first obtain a transferable runtime artifact/file reference;
3. use a minimal probe before a large upload;
4. verify filename, MIME type, destination, and raw-download hash after upload;
5. classify failures by boundary before changing the workflow.
