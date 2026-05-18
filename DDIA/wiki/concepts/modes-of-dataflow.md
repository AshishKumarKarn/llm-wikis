---
title: Modes of Dataflow
type: concept
chapters: [4]
tags: [encoding, dataflow, databases, rpc, messaging]
status: solid
updated: 2026-05-16
---

# Modes of Dataflow

## Definition

Compatibility is a relationship between an **encoding** process and a **decoding**
process. The three most common ways encoded data flows between processes. *(DDIA
Ch 4)*

## 1. Through databases

Writer encodes, reader decodes. A single process = "a message to your future self" →
**backward compat** essential. Multiple processes (rolling upgrade) → **forward
compat** too: a value written by new code may be read by old code still running.

- **Preserve unknown fields**: if old code reads a record (with a new field it
  doesn't understand), modifies it, and writes it back, it must keep the unknown
  field intact — easy to lose if decoding into model objects then re-encoding
  (handle at app level).
- **Data outlives code**: a deployment replaces code in minutes; 5-year-old data
  stays in its original encoding unless explicitly rewritten. Migrating is expensive,
  so DBs avoid it (relational: add nullable column without rewrite — except MySQL).
  Schema evolution makes the whole DB *appear* uniformly schema'd. **Archival
  snapshots**: dump in the latest schema, immutable → Avro container files / Parquet.

## 2. Through services (REST & RPC)

Clients call a server's application-specific API (vs. a DB's arbitrary queries —
encapsulation). SOA/microservices: independently deployable/evolvable services →
old/new clients & servers coexist → encoding must be version-compatible. See
[[rpc-vs-rest]].

## 3. Through async message passing

See [[message-passing-dataflow]] — message brokers and distributed actors; between
RPC (low latency) and DB (stored via an intermediary).

## Related concepts

- [[backward-forward-compatibility]] · [[schema-evolution]] · [[rpc-vs-rest]]
- [[message-passing-dataflow]]
- [[ch11-stream-processing]] — logs/brokers as dataflow; idempotence

## Sources

DDIA Ch 4 ("Modes of Dataflow").
