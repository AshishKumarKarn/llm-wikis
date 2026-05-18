---
title: "Change Data Capture vs. Event Sourcing"
type: comparison
chapters: [11]
tags: [stream, cdc, event-sourcing, comparison]
status: solid
updated: 2026-05-16
---

# Change Data Capture vs. Event Sourcing

Both store all changes as a log; they differ in **level of abstraction**. *(DDIA
Ch 11)*

| | [[change-data-capture\|CDC]] | [[event-sourcing\|Event sourcing]] |
|---|---|---|
| App uses DB | mutably (update/delete at will) | append-only event log; updates/deletes discouraged |
| Log extracted | low-level, from replication log; **app unaware** | explicitly built by app logic |
| Event meaning | state changes (new row value) | **intent of a user action** |
| Log compaction | yes — latest value per key suffices | **no** (intent events don't override; need full history) |
| Primary benefit | sync derived systems, app-agnostic | meaningful modeling, auditability, evolvability |

## Shared core

Both rest on **immutability** ([[state-streams-immutability]]): one ordered log of
changes; derived systems are followers; replay reconstructs state; both avoid the
dual-writes race. Both are usually **asynchronous** ([[replication-lag]] applies).

## Choosing

CDC retrofits streaming onto an existing mutable-DB app with minimal change. Event
sourcing is an architectural choice for new systems wanting explicit intent,
auditing, and easy multi-view derivation (CQRS) — at the cost of keeping full history
and synchronous command validation.

## Related

- [[change-data-capture]] · [[event-sourcing]] · [[state-streams-immutability]] ·
  [[log-based-message-brokers]]

## Sources

DDIA Ch 11 ("Event Sourcing").
