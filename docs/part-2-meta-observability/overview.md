# Part II — Meta-observability and partial recovery

## Question

How much information about artifacts, sessions, and historical project chronology can be observed or reconstructed from the surfaces available to the current session?

## Why this is a separate meta part

This part does not directly test whether software can be built. It studies the observability of the environment itself: File Library object metadata, search-result behavior, timestamp semantics, session-level historical context, event ordering, conversation/session enumeration limits, and practical chronology reconstruction.

Detailed report and experiment record currently remain at:

- `docs/06-search-and-timeline-reconstruction.md`
- `experiments/search-recovery.md`

Compact source probes are collected under `evidence/part-2-meta-observability/`.

## Conclusion

Observed resolution:

```text
session-level timestamp    available
session title              available
selected event ordering    available
message-level timestamps   not observed
turn IDs                   not observed
branch-node IDs            not observed
generation IDs             not observed
```

Complete historical recovery was not observed. Approximate experimental chronology remains practical by combining File Library object metadata, session-level contextual information, and semantic event ordering.
