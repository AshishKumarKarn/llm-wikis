---
title: Lamport Timestamps & Sequence Number Ordering
type: concept
chapters: [9]
tags: [distributed, ordering, causality, logical-clocks]
status: solid
updated: 2026-05-16
---

# Lamport Timestamps & Sequence Number Ordering

## Definition

A compact way to totally order operations *consistent with causality*: each operation
gets a **(counter, node ID)** pair. Compare by counter, break ties by node ID. From a
**logical clock** (incrementing counter), not a physical clock. *(DDIA Ch 9)*

## The key mechanism

Plain per-node counters (odd/even, block allocation, physical timestamps) give a
total order but **inconsistent with causality** (nodes process at different rates;
clock skew; block ranges). Lamport's fix: **every node and client tracks the max
counter it has seen and includes it on every request; on receiving a higher max, a
node jumps its counter forward**. Every causal dependency thus increases the
timestamp ⇒ the total order is consistent with causality.

- vs. **version vectors** ([[happens-before-and-concurrency]]): version vectors
  distinguish concurrent vs. causally-dependent; Lamport timestamps **always force a
  total order** (can't tell concurrent from dependent) but are more compact.
- In single-leader replication, the replication log already is such a
  causally-consistent total order (leader increments a counter).

## Why timestamp ordering is not sufficient

A total order only emerges **after collecting all operations**. To enforce a
**uniqueness constraint** (username), a node must decide *right now* whether to
accept — but it can't know if another node is concurrently claiming the same name
with a lower timestamp without checking every node (fails if any is down). You need
to know **when the order is finalized** → [[total-order-broadcast]].

## Related concepts

- [[causal-consistency]] · [[happens-before-and-concurrency]] (version vectors) ·
  [[total-order-broadcast]] · [[unreliable-clocks]] · [[lamport-clocks-paper]]

## Sources

DDIA Ch 9 ("Sequence Number Ordering").
