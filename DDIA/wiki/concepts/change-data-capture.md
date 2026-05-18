---
title: Change Data Capture (CDC)
type: concept
chapters: [5, 11]
tags: [replication, cdc, stream-processing, derived-data]
status: solid
updated: 2026-05-16
---

# Change Data Capture (CDC)

## Definition

Observing **all data changes written to a database** and extracting them as a
**stream**, in the order written, so they can be replicated to other systems.
Introduced in [[ch05-replication|Ch 5]] (logical replication log); fully developed in
[[ch11-stream-processing|Ch 11]]. *(DDIA Ch 5 & 11)*

## The problem it solves: dual writes

Apps combine many stores (OLTP DB, cache, search index, warehouse) that must be kept
**in sync**. **Dual writes** (app writes to each system itself) have two failures:
a **race condition** (concurrent writes A,B applied in different orders at different
systems → permanent inconsistency, silently) and **partial failure** (one write
succeeds, one fails — the atomic-commit problem). With no single leader across
systems, conflicts occur ([[multi-leader-replication]]).

## How CDC fixes it

Make **one database the leader** (the system of record), capture its change log, and
make all derived systems **followers** that apply changes in the same order (state
machine replication, [[total-order-broadcast]] — a replication log *is* an event
stream). A **log-based message broker** transports the changes (preserves
order, avoids redelivery reordering).

## Implementing CDC

- Triggers (fragile, slow) or **parsing the replication log** (robust; schema-change
  challenges). LinkedIn Databus, Facebook Wormhole, Yahoo Sherpa; Bottled Water
  (Postgres WAL), Maxwell/Debezium (MySQL binlog), Mongoriver (oplog), GoldenGate
  (Oracle). Kafka Connect integrates many.
- **Asynchronous** (operational upside; downside: [[replication-lag]] applies).
- **Initial snapshot** at a known log offset (full state can't come from recent
  changes alone), then apply changes from there.
- **Log compaction**: keep only the latest write per key (tombstones for deletes) →
  a compacted log holds a *full copy* of current DB contents (size ∝ contents, not
  write count) → rebuild a new derived system from offset 0 without a fresh snapshot.
  Supported by [[apache-kafka]].
- **First-class change streams**: RethinkDB/Firebase/CouchDB change feeds, Meteor
  (Mongo oplog), VoltDB export streams.

## Related concepts

- [[replication-log-implementations]] · [[log-based-message-brokers]] ·
  [[event-sourcing]] · [[cdc-vs-event-sourcing]] · [[state-streams-immutability]] ·
  [[systems-of-record-and-derived-data]] · [[materialized-views-and-data-cubes]]

## Sources

DDIA Ch 5 ("Trigger-based replication"); Ch 11 ("Change Data Capture").
