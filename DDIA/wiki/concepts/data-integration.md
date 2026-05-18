---
title: Data Integration
type: concept
chapters: [12]
tags: [data-integration, derived-data, dataflow]
status: solid
updated: 2026-05-16
---

# Data Integration

## The problem

No single tool fits all uses (storage engines Ch 3, replication Ch 5, …). Complex
apps use data many ways, so you **compose several tools** — and must keep them in
sync. "99% of people only need X" says more about the speaker than the technology.
The need becomes apparent only when you zoom out to **dataflows across the whole
organization**. *(DDIA Ch 12)*

## Deriving data from a single source

Be explicit about **inputs vs. outputs**: where is data written first, what is derived
from what? Funnel all writes through **one system that decides a total order**, then
derive other representations by processing writes in that order (state machine
replication, [[total-order-broadcast]]) — via [[change-data-capture|CDC]] or
[[event-sourcing]]. Updates to derived systems can be **deterministic & idempotent**
→ easy fault recovery. Direct dual writes cause the [[change-data-capture|dual-writes
race]].

## Derived data vs. distributed transactions

Both keep systems consistent. Distributed transactions: order via locks (2PL), effect
via atomic commit; provide **linearizability** (read-your-writes). Log-based derived
data: order via the log, effect via deterministic retry + idempotence; **async** (no
timing guarantee). Since XA has poor fault tolerance/perf
([[distributed-transactions-xa]]), **log-based derived data is the most promising
integration approach** — but "eventual consistency, suck it up" isn't productive
either (→ [[timeliness-vs-integrity]]).

## Limits of total ordering & causality

A single totally-ordered log needs all events through one leader — breaks down with
partitioned throughput, geo-distributed datacenters, microservices, offline clients
(total order broadcast = consensus, doesn't scale past one node — open research).
Where there's no causal link, arbitrary order is fine; subtle causal deps
(unfriend-then-message) need logical timestamps / event-ID references / conflict
resolution — no simple answer ([[causal-consistency]]).

## Batch & stream both serve integration

Goal: data in the right form everywhere. Stream → low-delay derived views; batch →
**reprocess history** to derive new views (essential for application evolution /
schema migration — the "railway dual-gauge" gradual migration, every step
reversible). Maintain derived state with deterministic functions; async makes it
robust (fault contained, vs. distributed transactions amplifying failures).

## Related concepts

- [[change-data-capture]] · [[event-sourcing]] · [[lambda-architecture]] ·
  [[unbundling-databases]] · [[systems-of-record-and-derived-data]] ·
  [[distributed-transactions-xa]] · [[total-order-broadcast]]

## Sources

DDIA Ch 12 ("Data Integration").
