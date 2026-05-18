---
title: Serializability
type: concept
chapters: [7]
tags: [transactions, serializability, isolation, concurrency-control]
status: solid
updated: 2026-05-16
---

# Serializability

## Definition

The strongest isolation level: even though transactions run in parallel, the end
result is **as if they ran one at a time, serially**. If each transaction is correct
alone, it stays correct concurrently — **all** race conditions
([[lost-updates]], [[write-skew-and-phantoms]], dirty reads/writes, read skew)
prevented. *(DDIA Ch 7)*

## Why it isn't universal

The researchers' answer since the 1970s has been "just use serializable isolation" —
but it has a performance cost, so weak [[isolation-levels]] persist. Isolation levels
are hard to understand, inconsistently implemented, and there are no good tools to
detect race conditions (nondeterministic, timing-dependent). Three implementations:

- **[[actual-serial-execution]]** — literally one transaction at a time on one
  thread (simple; one-CPU-core throughput).
- **[[two-phase-locking]]** (2PL) — the standard for ~30 years; pessimistic; poor
  performance.
- **[[serializable-snapshot-isolation]]** (SSI) — newer, optimistic; near
  snapshot-isolation performance.

Compared in [[serializability-implementations]]. Single-node here; distributed
serializability → [[ch09-consistency-and-consensus]].

## Related concepts

- [[isolation-levels]] · [[actual-serial-execution]] · [[two-phase-locking]] ·
  [[serializable-snapshot-isolation]] · [[serializability-implementations]]
- [[acid]] (the "I") · [[ch09-consistency-and-consensus]] (distributed)

## Sources

DDIA Ch 7 ("Serializability").
