---
title: Unbundling Databases
type: concept
chapters: [12]
tags: [data-integration, unbundling, dataflow, architecture]
status: solid
updated: 2026-05-16
---

# Unbundling Databases

## The premise

Databases, Hadoop, and operating systems all do the same things — store data, let you
process/query it. Unix (low-level byte-stream abstraction, pipes) vs. relational DB
(high-level declarative abstraction) — an unresolved 40-year tension; NoSQL ≈ a
Unix-esque approach to distributed OLTP. Reconcile both. *(DDIA Ch 12)*

## Databases' features ↔ derived-data systems

`CREATE INDEX` = scan a consistent snapshot, build the index, process the backlog,
keep it updated — **the same process as setting up a follower / bootstrapping CDC**.
So the org-wide dataflow looks like **one huge database**: batch/stream processors
are elaborate triggers/stored-procedures/materialized-view maintainers; derived
systems are different "index types" provided by separate software, machines, teams.

## Two ways to compose (federation vs. unbundling)

See [[federated-vs-unbundled-databases]]:

- **Federated databases (unify reads)** — one query interface over many engines
  (polystore; PostgreSQL foreign data wrappers). Relational tradition: elegant
  high-level query, complex implementation.
- **Unbundled databases (unify writes)** — reliably synchronize writes across systems
  via **CDC + event logs** (unbundle the DB's index-maintenance feature). Unix
  tradition: small tools, uniform low-level API, composed by a higher-level language.

## Making unbundling work

Keeping writes in sync is the hard problem. Reject heterogeneous distributed
transactions ([[distributed-transactions-xa]]); use an **asynchronous event log with
idempotent writes** — far simpler & feasible across heterogeneous systems. Big
advantage: **loose coupling** — system-level (a log buffers for a slow/failed
consumer, fault contained, vs. distributed transactions escalating local faults) and
human-level (independent teams, well-defined log interface).

## Unbundled vs. integrated; what's missing

Unbundling doesn't replace databases (still needed for state/serving; MPP for
analytics). It's about **breadth of workloads, not depth** — if one tool does
everything you need, use it (premature unbundling = premature optimization). Missing:
the **unbundled-database equivalent of the Unix shell** — a high-level declarative
language to compose storage/processing (imagine `mysql | elasticsearch` ≈ `CREATE
INDEX` across systems; differential dataflow is early research).

## Related concepts

- [[federated-vs-unbundled-databases]] · [[data-integration]] ·
  [[dataflow-applications]] · [[change-data-capture]] · [[the-log-kreps]] ·
  [[unix-philosophy]]

## Sources

DDIA Ch 12 ("Unbundling Databases").
