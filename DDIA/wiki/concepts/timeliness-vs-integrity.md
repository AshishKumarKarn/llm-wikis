---
title: Timeliness vs. Integrity (Coordination-Avoiding Systems)
type: concept
chapters: [12]
tags: [correctness, consistency, coordination, integrity]
status: solid
updated: 2026-05-16
---

# Timeliness vs. Integrity (Coordination-Avoiding Systems)

## The distinction

"Consistency" conflates two separable requirements: *(DDIA Ch 12)*

- **Timeliness** — users observe an up-to-date state. Violations are *temporary*
  (resolve by waiting) → **"eventual consistency"**. Linearizability is a strong way
  to get timeliness; read-after-write is a weaker useful one.
- **Integrity** — absence of corruption: no data loss, no contradictory/false data;
  derived data correctly reflects its source. Violations are **permanent**
  ("perpetual inconsistency") → need explicit check & repair. ACID atomicity &
  durability preserve integrity.

> **Integrity ≫ timeliness in most applications.** A credit-card transaction missing
> for 24 h is fine; a statement balance ≠ sum of transactions, or money charged but
> not paid, is catastrophic.

## Dataflow decouples them

ACID transactions bundle both, so the distinction seems inconsequential there. But
event-based dataflow **decouples** them: async → no timeliness guarantee (unless you
explicitly wait for an output message), but **integrity is central** —
exactly-once/idempotence preserves it. Reliable stream processing achieves integrity
**without distributed transactions** via: write = a single atomic message
([[event-sourcing]]); derive all else with deterministic functions; pass an
end-to-end request ID for dedup; immutable messages + reprocessing.

## Loosely interpreted constraints & coordination-avoidance

Many real constraints can be **temporarily violated and fixed by apologizing** —
**compensating transactions** (overbooking flights/hotels, overdraft fees,
back-ordering stock, "choose another username"). The apology workflow is already part
of business. So strict linearizable constraint-checking-before-write is often
unnecessary: write optimistically, validate after (before anything expensive to
recover from). These apps need **integrity**, not **timeliness** of the constraint.

⇒ **Coordination-avoiding data systems**: maintain strong integrity *without* atomic
commit / linearizability / synchronous cross-partition coordination — better
performance & fault tolerance, can run multi-datacenter multi-leader. Add
coordination only where strictly needed. Coordination reduces inconsistency
apologies but increases outage apologies — find the sweet spot.

## Related concepts

- [[end-to-end-argument]] · [[enforcing-constraints-in-dataflow]] ·
  [[eventual-consistency]] · [[linearizability]] · [[cap-theorem]] ·
  [[write-conflict-resolution]] · [[acid]]

## Sources

DDIA Ch 12 ("Timeliness and Integrity").
