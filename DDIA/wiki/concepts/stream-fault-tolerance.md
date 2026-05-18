---
title: Stream Fault Tolerance (Exactly-Once)
type: concept
chapters: [11]
tags: [stream, fault-tolerance, exactly-once, idempotence]
status: solid
updated: 2026-05-16
---

# Stream Fault Tolerance (Exactly-Once)

## The goal

Batch gets fault tolerance easily (retry task, discard failed output, only-visible-
on-success) → **exactly-once semantics**: visible output is as if every record was
processed once (better: **"effectively-once"** — records *may* be reprocessed, but
the effect appears once). Harder for streams: you can't wait for a never-ending job
to "finish" before making output visible. *(DDIA Ch 11)*

## Techniques

- **Microbatching** (Spark Streaming) — split the stream into ~1 s blocks, each a
  tiny batch (implicitly a processing-time tumbling window). **Checkpointing**
  (Flink) — periodic rolling state snapshots to durable storage, triggered by
  barriers; on crash, restart from the last checkpoint, discard output since. Both
  give exactly-once **within the framework** — but **not** for external side effects
  (DB write, email): a restarted task does the side effect twice.
- **Atomic commit** — make all outputs & side effects (downstream messages, DB
  writes, state changes, consumer-offset advance) happen atomically or not at all
  (cf. [[distributed-transactions-xa|exactly-once message processing]]). Efficient in
  a *restricted* (single-framework) setting: Google Cloud Dataflow, VoltDB, Kafka
  (KIP-98) — unlike heterogeneous XA; amortized over many messages per transaction.
- **Idempotence** — an operation safe to perform multiple times (set key = value:
  yes; increment: no). Make non-idempotent ops idempotent with metadata (store the
  Kafka **offset** with the written value → skip if already applied). Needs:
  deterministic replay in the same order (log-based broker), no concurrent updates,
  and **[[fencing-tokens|fencing]]** on failover.

## Rebuilding state after failure

Windowed aggregations / join tables/indexes must recover state: remote replicated
store (slow per-message); or **local state replicated periodically** (Flink snapshots
to HDFS; Samza/Kafka Streams replicate to a log-compacted Kafka topic — like CDC;
VoltDB redundantly processes on several nodes). Sometimes just **rebuild from the
input** (short window → replay events; CDC local replica → replay log-compacted
changelog). No universally ideal trade-off (depends on network vs. disk
characteristics).

## Related concepts

- [[distributed-transactions-xa]] (exactly-once) · [[fencing-tokens]] ·
  [[change-data-capture]] · [[stream-joins]] · [[apache-flink]] · [[apache-samza]]

## Sources

DDIA Ch 11 ("Fault Tolerance").
