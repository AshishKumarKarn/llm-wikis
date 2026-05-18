---
title: "Ch 4 — Encoding and Evolution"
type: chapter
chapters: [4]
tags: [encoding, serialization, schema-evolution, rpc, rest, messaging]
status: solid
updated: 2026-05-16
---

# Ch 4 — Encoding and Evolution

## One-paragraph thesis

Applications change, so their data changes. Because code changes can't happen
instantaneously (server rolling upgrades, un-updated client apps), **old and new
code and old and new data formats coexist**. The system must preserve **backward
compatibility** (new code reads old data) and **forward compatibility** (old code
reads new data). This chapter surveys encoding formats (language-specific, textual
JSON/XML/CSV, binary schema-driven Thrift/Protobuf/Avro) by their compatibility
properties, then the **modes of dataflow** (databases, services/RPC/REST, async
message passing) where encoding matters. The payoff: with care, rolling upgrades and
evolvability ([[maintainability]] Ch 1) are achievable. *(DDIA Ch 4)*

## Key ideas

- **[[backward-forward-compatibility]]** — the core property; enabled by, and
  enabling, rolling upgrades / staged rollouts.
- **[[data-encoding-formats]]** — language-specific (Java Serializable, pickle —
  insecure, language-locked); textual (JSON/XML/CSV — vague numbers, no binary,
  optional schemas); binary JSON variants (MessagePack — small win, not worth it).
- **[[thrift-and-protocol-buffers]]** — schema + IDL + code generation; **field
  tags** are the compatibility mechanism; required/optional rules.
- **[[avro]]** — **writer's vs. reader's schema** resolved by name; no tag numbers;
  great for **dynamically generated schemas**; works without code generation.
- **[[schema-evolution]]** — the merits of schemas; per-format add/remove/retype
  rules; "schema evolution gives schema-on-read flexibility *with* guarantees and
  tooling" (links to [[schema-on-read-vs-schema-on-write]]).
- **[[modes-of-dataflow]]** — databases ("data outlives code", preserve unknown
  fields); services; message passing.
- **[[rpc-vs-rest]]** — why RPC's location transparency is "fundamentally flawed";
  REST vs. SOAP; modern RPC (gRPC/Thrift/Finagle).
- **[[message-passing-dataflow]]** — message brokers, distributed actor frameworks.

## Concepts introduced

- [[backward-forward-compatibility]] · [[data-encoding-formats]] ·
  [[thrift-and-protocol-buffers]] · [[avro]] · [[schema-evolution]]
- [[modes-of-dataflow]] · [[message-passing-dataflow]]

## Comparisons introduced

- [[rpc-vs-rest]]
- [[shared-memory-vs-shared-disk-vs-shared-nothing]] *(Part II intro)*

## Part II intro (Distributed Data)

Why distribute: scalability, fault-tolerance/HA, latency. Architectures:
**shared-memory** (vertical, cost grows superlinearly, one location),
**shared-disk** (warehousing; locking/contention limits), **shared-nothing**
(horizontal — the book's focus; max caution required, DB can't hide trade-offs). Two
mechanisms, often combined: **[[replication-vs-partitioning|replication]]** (copies
on several nodes → Ch 5) and **partitioning/sharding** (split into subsets → Ch 6).
See [[shared-nothing-architecture]].

## Trade-offs & tensions

- Backward compat (easy — you know the old format) vs. **forward compat** (tricky —
  old code must ignore unknown additions).
- Compact binary + schema docs/codegen vs. not human-readable.
- Tag-based (Thrift/Protobuf, hand-assigned tags) vs. name-based (Avro, dynamic
  schemas).
- RPC performance vs. REST's tooling/debuggability/ubiquity.
- Message broker decoupling/buffering vs. one-way async (no return value).

## Connections to other chapters

- Evolvability/rolling upgrades originate in [[ch01-reliable-scalable-maintainable]]
  ([[maintainability]]); schema-on-read link → [[ch02-data-models-and-query-languages]].
- Archival → column formats (Parquet) → [[ch03-storage-and-retrieval]],
  [[ch10-batch-processing]].
- RPC timeout/uncertainty → [[ch08-the-trouble-with-distributed-systems]];
  idempotence/dedup → [[ch11-stream-processing]].
- Message brokers compared in depth → [[ch11-stream-processing]] ([[apache-kafka]]).
- Part II intro sets up [[ch05-replication]], [[ch06-partitioning]].

## Open questions / things to revisit

- How do log-based message brokers (Ch 11) change the encoding-evolution story vs.
  traditional brokers?
- Relation between the WAL/replication log (Ch 3/5) and message-passing logs.
