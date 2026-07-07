# File Library handoff mechanism

## Summary

The experiment identified `sandbox:` references in assistant-authored messages as the observable trigger for artifact handoff from session-local storage.

The following did **not** cause promotion:

```text
file creation only
basename only
/mnt/data/absolute/path
path printed in tool stdout
```

The following did cause artifact handoff:

```text
sandbox:/mnt/data/file.txt
[label](sandbox:/mnt/data/file.txt)
```

Both commentary and final response stages were capable of triggering handoff.

## Stage behavior

A file referenced in commentary was not searchable through `file_search` immediately in the same assistant turn. It remained absent after approximately 20 seconds while the same assistant turn was held open. It became searchable in a subsequent turn.

At the same time, platform events showed that persistent file objects could be created before turn completion.

The best observable model is therefore:

```text
runtime file
  -> assistant message contains sandbox URI
  -> artifact resolver / handoff
  -> persistent file object
  -> publication or visibility barrier
  -> File Library search visibility
```

The experiment cannot distinguish whether the final visibility barrier is implemented as turn-bound publication, search snapshot refresh, or another transaction boundary.

## Deduplication scope

Two identical references to the same URI in one commentary segment produced one observable logical Library object.

Referencing the same path in commentary and final stages produced two logical file objects with the same filename and content but different identities.

This implies a deduplication scope no broader than the individual message segment, while later reuse may also involve attachment binding or cache behavior.

## Snapshot semantics

Promotion is not a live binding to the local path.

Observed state transitions:

```text
promote V1
local delete
Library still contains V1
```

and:

```text
promote V1
local overwrite path with V2
Library still contains V1
```

A later re-reference of the modified same path did not produce a searchable V2 object in the controlled probe. The strongest interpretation is that the service may reuse an existing attachment binding for a previously materialized path/reference rather than re-snapshotting current local bytes on every later reuse.

This behavior is distinct from the cross-segment duplicate-object observation. The resolver therefore appears to have more than one scope or cache layer.

## File-type probes

For zero-byte text, PNG, and ZIP probes, artifact handoff events occurred, but the files were not observed through the text-oriented `file_search` surface used in the experiment.

This establishes an important distinction:

```text
artifact object existence != file_search retrievability
```

Directory URI and nonexistent-path URI probes did not produce an observable artifact object.

## Operational rule

For reliable user-facing persistence from the session-local workspace:

1. create the file under `/mnt/data`;
2. reference it in an assistant-authored message with a `sandbox:` URI or Markdown link;
3. treat the resulting persistent object as a snapshot independent from later local changes;
4. do not use `file_search` absence as proof that a non-text artifact object was never created.
