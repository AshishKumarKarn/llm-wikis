---
title: The CAP Theorem
type: concept
chapters: [9]
tags: [distributed, consistency, availability, cap]
status: developing
updated: 2026-05-16
---

# The CAP Theorem

## The actual trade-off

Any **linearizable** system has this problem: if some replicas are disconnected by a
network fault, they **must either become unavailable** (wait / error) **or give up
linearizability**. A non-linearizable system (e.g. multi-leader) can keep each replica
serving independently. So: apps that don't need linearizability tolerate network
problems better. Named by Brewer (2000); known to designers since the 1970s.
*(DDIA Ch 9)*

## Why "pick 2 of 3" is unhelpful

CAP as "Consistency, Availability, Partition tolerance — pick 2" is **misleading**:
network partitions are a *fault*, not a choice — they happen regardless. Better
phrasing: **"Consistent or Available when Partitioned"** (only when a partition
occurs must you choose; otherwise you can have both). The formal theorem is **very
narrow**: only linearizability, only network partitions — says nothing about delays,
dead nodes, etc. Multiple contradictory definitions of "availability"; many
"highly available" systems don't meet CAP's idiosyncratic one.

> DDIA's verdict: CAP is historically influential (encouraged shared-nothing NoSQL)
> but **has little practical value today** and is superseded by more precise results
> — best avoided. (Avoid the "CP/AP" labels too.)

Note: linearizability is often dropped for **performance**, not fault tolerance
(multi-core RAM isn't linearizable either — CAP doesn't apply there).

## Related concepts

- [[linearizability]] (the cost of) · [[causal-consistency]] (the partition-tolerant
  strong alternative) · [[eventual-consistency]] · [[unreliable-networks]]

## Sources

DDIA Ch 9 ("The CAP theorem", "The Unhelpful CAP Theorem" box). Refs: Gilbert &
Lynch 2002; Brewer 2012; Kleppmann, "A Critique of the CAP Theorem" (2015).
