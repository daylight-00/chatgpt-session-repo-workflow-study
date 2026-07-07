# Search and timeline reconstruction

## Objective

This experiment examined how much historical project activity can be reconstructed from the search and context surfaces available inside a ChatGPT session.

The practical target was not full conversation export. The target was retrospective experimental reporting: determining whether it is possible to recover that a particular activity happened in a particular session at an approximate time, and how far that reconstruction can be refined.

## Observed search surfaces

Two distinct mechanisms were observed.

### File Library search

File Library exposes an explicit object-retrieval surface. Search results can expose:

```text
file object ID
title
created_at
modified_at
content source
query-relevant snippet
```

The external retrieval unit is the file object. The same object can be returned for different content queries with the same file ID while the snippet changes according to the query.

Duplicate filenames and duplicate contents can remain separate objects with independent IDs and object timestamps.

`mclick` expanded file content but did not expose additional conversation provenance such as source session, source turn, assistant segment, branch, or generation ID.

### Session-level conversation context

Historical conversation information can also be available as context supplied to the current session.

Observed fields were sufficient to identify, for some sessions:

```text
session-level timestamp
session title
selected message content
message/content ordering
```

No callable conversation-object search/list/get surface was observed in the assistant tool set used for this study.

Historical conversation information therefore cannot be treated like File Library objects. It is usable when present in current context, but it is not exposed as an enumerable object store with conversation IDs, message IDs, or direct transcript-fetch operations.

## Timeline resolution

The current observable resolution is:

```text
session-level timestamp    available
session title              available
selected event ordering    available
message-level timestamps   not observed
turn IDs                   not observed
branch-node IDs            not observed
generation IDs             not observed
```

A session timeline can therefore be reconstructed semantically, but not as a precise wall-clock sequence.

The session-level timestamp visible in context should not be labeled `created_at`, `updated_at`, or `last_modified_at` without additional evidence. This experiment established only that a session-level timestamp is exposed.

## File timestamp semantics

A controlled probe separated source filesystem time from Library object time.

The local file mtime was forced to:

```text
2020-01-02T03:04:05Z
```

After handoff, the File Library object showed 2026-07-07 creation/modification timestamps corresponding to the Library object lifecycle, not the source filesystem mtime.

```text
source filesystem mtime
!=
Library object created_at
```

This distinction is useful for retrospective reconstruction: Library timestamps are evidence for artifact-object creation or publication timing, not necessarily for when the underlying local experiment or source file was originally created.

## Cross-session reconstruction

A practical retrospective workflow can combine two incomplete evidence sources:

```text
session-level context
  -> session title
  -> session-level timestamp
  -> semantic event order

File Library metadata
  -> artifact object timestamps
  -> filenames
  -> content/snippets
  -> duplicate object identity
```

Together they can support approximate reconstruction such as:

```text
session X at time T
  -> activity A
  -> activity B
  -> generated artifact C shortly afterward
```

This does not provide complete forensic reconstruction. Missing message timestamps, unavailable conversation objects, partial context selection, and lack of branch/generation identifiers prevent exact recovery.

## Final assessment

Complete recovery or direct enumeration of historical conversations was not observed.

However, partial reconstruction is practical by combining File Library object metadata, session-level contextual information, and message/event ordering.

The result is sufficient for approximate experimental chronology and report reconstruction, but not for exact turn-by-turn auditing.
