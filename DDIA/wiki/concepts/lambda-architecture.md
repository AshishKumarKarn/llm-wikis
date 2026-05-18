---
title: Lambda Architecture (and Unifying Batch + Stream)
type: concept
chapters: [12]
tags: [data-integration, lambda, batch, stream]
status: developing
updated: 2026-05-16
---

# Lambda Architecture (and Unifying Batch + Stream)

## The idea

Record incoming data as immutable events (like [[event-sourcing]]); derive
read-optimized views by running **two systems in parallel**: a **stream processor**
(fast, approximate, immediate update) and a **batch processor** (slower, exact,
corrected version later). Reasoning: batch is simpler/less buggy; streams thought
less reliable. *(DDIA Ch 12)*

## Why it shaped thinking — and its problems

Influential: popularized deriving views from immutable event streams and reprocessing
when needed. But practical problems:

- Maintain the **same logic twice** (batch + stream frameworks) — operational
  complexity even with abstractions like Summingbird.
- Outputs must be **merged** to serve requests — easy for tumbling-window
  aggregations, hard for joins/sessionization.
- Reprocessing all history frequently is expensive → incremental batches →
  reintroduces the streaming-layer complexity (stragglers, cross-batch windows),
  defeating the "keep batch simple" goal.

## Unifying batch and stream

Get lambda's benefits without the downsides by doing both **in one system**:

- **Replay** historical events through the same engine as recent events (log-based
  brokers replay; stream processors read HDFS).
- **Exactly-once** semantics ([[stream-fault-tolerance]]).
- **Event-time windowing** (processing time is meaningless when reprocessing) —
  Apache Beam over Flink / Cloud Dataflow.

"Batch is a special case of streaming" — Spark microbatches streams, Flink batches on
a stream engine; the distinction is blurring.

## Related concepts

- [[data-integration]] · [[event-sourcing]] · [[batch-vs-stream-vs-online]] ·
  [[stream-fault-tolerance]] · [[reasoning-about-time-in-streams]]

## Sources

DDIA Ch 12 ("Batch and Stream Processing"). Ref: Marz & Warren, *Big Data* (2015);
Kreps, "Questioning the Lambda Architecture" (2014).
