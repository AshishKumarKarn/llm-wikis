---
title: "Linearizability vs. Serializability"
type: comparison
chapters: [9]
tags: [distributed, consistency, transactions, comparison]
status: solid
updated: 2026-05-16
---

# Linearizability vs. Serializability

Easily confused (both ≈ "arrange in a sequential order") but **different
guarantees**. *(DDIA Ch 9)*

| | [[serializability]] | [[linearizability]] |
|---|---|---|
| Property of | transaction **isolation** | recency on a single object (register) |
| Scope | multiple objects, multiple operations per transaction | one register, single operations |
| Guarantees | result = *some* serial order of transactions (order may differ from real time) | operations respect **real-time** order; reads see latest write |
| Prevents | write skew, phantoms, etc. | stale reads | 
| Does *not* prevent | stale-by-design snapshot reads (SSI) | write skew (no transaction grouping) |

## The combination

**Strict serializability** (a.k.a. strong-1SR) = both. 2PL and actual serial
execution are typically *linearizable* serializability. **SSI is serializable but
NOT linearizable** — by design it reads from a consistent snapshot that excludes the
most recent writes, so reads aren't a recency guarantee.

## Why it matters

A database can be one, the other, both, or neither. Knowing which you need: recency
of a single value (a lock, a uniqueness check) → linearizability; correctness of
multi-object transactions under concurrency → serializability.

## Related

- [[linearizability]] · [[serializability]] · [[serializable-snapshot-isolation]] ·
  [[two-phase-locking]] · [[actual-serial-execution]]

## Sources

DDIA Ch 9 ("Linearizability Versus Serializability" box). Ref: Bailis,
"Linearizability Versus Serializability" (2014).
