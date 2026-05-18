---
title: Consistent Prefix Reads
type: concept
chapters: [5]
tags: [consistency, replication, causality]
status: developing
updated: 2026-05-16
---

# Consistent Prefix Reads

## Definition

A guarantee that if a sequence of writes happens in a certain order, anyone reading
them sees them in the **same order**. Prevents causality violations. *(DDIA Ch 5)*

## The anomaly it fixes

Mr. Poons asks a question; Mrs. Cake answers. An observer reading through followers
with *different* lag per partition may see the **answer before the question** —
apparent psychic powers. A causality violation.

## Why & how

Particularly a problem in **partitioned/sharded** databases (→
[[ch06-partitioning]]): partitions operate independently with **no global write
order**, so reads can mix older and newer parts. Solutions: keep causally-related
writes in the same partition; or explicitly track causal dependencies (→
[[happens-before-and-concurrency]], deepened in
[[ch09-consistency-and-consensus]]).

## Related concepts

- [[replication-lag]] · [[eventual-consistency]] · [[monotonic-reads]] ·
  [[read-after-write-consistency]]
- [[happens-before-and-concurrency]] · [[ch06-partitioning]]

## Sources

DDIA Ch 5 ("Consistent Prefix Reads").
