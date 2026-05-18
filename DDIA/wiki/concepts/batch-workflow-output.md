---
title: The Output of Batch Workflows
type: concept
chapters: [10]
tags: [batch, derived-data, fault-tolerance]
status: developing
updated: 2026-05-16
---

# The Output of Batch Workflows

## What batch jobs produce

Neither OLTP nor analytics-report: a batch job's output is **derived data** — a new
structure built from a bounded, immutable input. *(DDIA Ch 10)*

- **Search indexes**: Google's original MapReduce use (5–10 jobs); document-
  partitioned [[lucene]]-style indexes built in parallel, immutable once written.
  Rebuild wholesale, or incrementally (Lucene segments → Ch 11).
- **Read-only key-value stores**: ML classifiers / recommendation systems output a
  DB. **Don't** write to the production DB from inside the job (slow per-record
  network calls, overwhelms the DB, non-atomic side effects). Instead **build
  immutable DB files in the job**, bulk-load, atomically switch over (Voldemort,
  Terrapin, ElephantDB, HBase bulk load) — easy to roll back to old files.

## Why this philosophy works

Same as [[unix-philosophy]]: immutable input, output fully replaces previous, no
side effects. ⇒

- **Human fault tolerance**: deploy buggy code → just roll back code & rerun (or keep
  old output dir); read-write DBs *can't* do this.
- Minimizing irreversibility → faster, safer feature development (Agile).
- Safe automatic task retry (inputs immutable, failed-task output discarded).
- Same files reusable by many jobs (incl. monitoring jobs); logic separated from
  wiring. Structured formats (Avro/Parquet) replace Unix's untyped-text parsing.

## Related concepts

- [[unix-philosophy]] · [[mapreduce]] · [[systems-of-record-and-derived-data]] ·
  [[materialized-views-and-data-cubes]] · [[lucene]] · [[voldemort]]

## Sources

DDIA Ch 10 ("The Output of Batch Workflows").
