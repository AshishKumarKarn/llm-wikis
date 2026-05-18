---
title: PostgreSQL
type: system
chapters: [2, 7]
tags: [system, relational, sql, mvcc, ssi]
status: developing
updated: 2026-05-16
---

# PostgreSQL

Open-source relational database used throughout DDIA as a canonical
[[relational-model]] exemplar. Ch 2 highlights it as a *converging* system: full SQL
plus JSON (≥9.3) and XML support with in-document indexing/querying, recursive CTEs
(`WITH RECURSIVE`) for graph-like traversal, and the `json` datatype used to model
property graphs.

Ch 7: **[[multi-version-concurrency-control|MVCC]]** implementation (txid,
`created_by`/`deleted_by`, visibility rules) for [[snapshot-isolation]] (called
"repeatable read"); **automatic lost-update detection** in repeatable read; the
reference implementation of **[[serializable-snapshot-isolation|SSI]]** (serializable
level since 9.1, Ports & Grittner). WAL shipping replication (Ch 5).

> `status: developing` — also recurs at Ch 9 (two-phase commit). Expand there.

## Related

- [[relational-model]] · [[multi-version-concurrency-control]] ·
  [[serializable-snapshot-isolation]] · [[snapshot-isolation]] ·
  [[replication-log-implementations]] · [[mongodb]] · [[neo4j]]
