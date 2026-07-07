# Search and recovery probe record

## Questions

The experiment asked:

1. What metadata can be retrieved for File Library objects?
2. Is search externally chunk-level or file-object-level?
3. Can File Library reveal the source conversation/session?
4. What temporal resolution is available for historical sessions?
5. Can a useful experiment timeline be reconstructed without complete conversation-object access?

## Library search observations

### Object metadata

Search results exposed:

```text
file ID
title
created_at
modified_at
content source
snippet
```

No source conversation ID, source turn ID, role, assistant segment, branch ID, or generation attempt was observed.

### Retrieval unit

The same long file was queried with different terms. Results kept the same file object ID while the returned snippet changed to match the query.

Interpretation:

```text
internal relevance may be chunk/content based
external result unit is the file object
```

No chunk ID, chunk offset, relevance score, or segment index was exposed.

### Duplicate objects

Controlled duplicate handoff experiments produced separate file objects in some cross-stage cases. Search distinguished those objects by object ID and creation time even when filename and content matched.

### Content expansion

Opening search results expanded file content but did not reveal additional origin provenance.

## Timestamp probe

Probe file:

```text
searchmeta_backdated_mtime_20260707.txt
```

Controlled local timestamp:

```text
mtime = 2020-01-02T03:04:05Z
```

Observed Library object timestamps were in 2026 at handoff time rather than 2020.

Result:

```text
Library created_at is not inherited source filesystem mtime
```

## Conversation/session context observations

Historical session information available to the current conversation included session-level fields and ordered selected content.

Observed usable level:

```text
session timestamp
title
selected user/assistant content
content order
```

Not observed:

```text
message timestamp
turn ID
conversation object ID
branch graph
parent/child node relation
generation attempt ID
```

The timestamp exposed for a session was not proven to mean final modification time. It is recorded conservatively as a session-level timestamp.

## Natural-case comparison

### Organ session

A session mentioning Bach, organ, and harpsichord could be identified from supplied historical context by title, session-level timestamp, and content sequence.

This was context access, not an explicit conversation search API call.

### Mesa activity

Mesa-related historical sessions could also be referenced from available context even though the user later clarified that those sessions belonged to a different UI project.

This invalidated a simple model in which only same-project conversations are available to current context.

The experiment did not establish the selection mechanism that made those sessions available.

### MLEC negative case

A requested MLEC session could not be located from the available context or File Library results.

This result does not prove that the session does not exist. It establishes only that the available surfaces did not provide an enumerable account-wide conversation lookup path in this experiment.

## Reconstruction model

The strongest practical model is:

```text
historical session context
  -> approximate session time
  -> session title
  -> semantic event sequence

Library metadata
  -> artifact object time
  -> filenames/content
  -> object identity

combined
  -> approximate experiment chronology
```

## Conclusion

The available surfaces do not support complete recovery of an arbitrary historical session or an exact message-by-message timeline.

They do support partial retrospective reconstruction by combining File Library object metadata with session-level contextual information and event ordering.
