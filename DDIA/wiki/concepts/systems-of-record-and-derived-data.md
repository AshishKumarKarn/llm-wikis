---
title: Systems of Record & Derived Data
type: concept
chapters: [9, 10]
tags: [derived-data, architecture, part-iii]
status: developing
updated: 2026-05-16
---

# Systems of Record & Derived Data

## Definition

The Part III framing. Real applications combine **many** datastores/indexes/caches/
analytics systems and move data between them. Two broad categories: *(DDIA Part III
intro)*

- **System of record** (source of truth) — holds the **authoritative** version; new
  data written here first; each fact represented once (typically normalized). On any
  discrepancy, this value is correct *by definition*.
- **Derived data** — the result of transforming/processing data from another system;
  **recreatable** from the source if lost. Examples: caches, denormalized values,
  indexes, materialized views, recommendation/predictive summaries.

## Why it matters

Derived data is technically **redundant** (duplicates information) but essential for
read performance, and commonly **denormalized** — one source can yield several
derived datasets for different "points of view". The distinction is **not a property
of the tool** (a DB is just a tool) but of **how you use it**. Making it explicit
clarifies dataflow — which parts have which inputs/outputs and how they depend on
each other — a running theme of Part III ([[ch10-batch-processing]],
[[ch11-stream-processing]], [[ch12-the-future-of-data-systems]]).

## Related concepts

- [[normalization-and-denormalization]] · [[materialized-views-and-data-cubes]] ·
  [[change-data-capture]] · [[ch10-batch-processing]] · [[ch11-stream-processing]]

## Sources

DDIA Part III introduction ("Derived Data").
