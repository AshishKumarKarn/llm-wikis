---
title: Total Order Broadcast
type: concept
chapters: [9]
tags: [distributed, ordering, consensus, replication]
status: solid
updated: 2026-05-16
---

# Total Order Broadcast

## Definition

(a.k.a. atomic broadcast / total order multicast — the "atomic" name is confusing,
unrelated to ACID) A message-exchange protocol with two **safety** properties that
always hold (even with faults; messages just wait out a network interruption):

- **Reliable delivery** — no message lost; if delivered to one node, delivered to
  all.
- **Totally ordered delivery** — every node delivers messages in the **same order**.

Crucially, the order is **fixed at delivery time** — no retroactive insertion. This
is what makes it stronger than [[lamport-timestamps|timestamp ordering]] (you know
when the order is final). *(DDIA Ch 9)*

## Uses

- **State machine replication**: each message = a write; all replicas apply the same
  writes in the same order → consistent (→ [[ch11-stream-processing]]). Exactly
  single-leader replication's job.
- **Serializable transactions**: each message = a deterministic stored procedure
  ([[actual-serial-execution]]) → all replicas/partitions consistent (Calvin).
- A way of **creating a log** (replication/transaction/WAL): deliver = append.
- **Fencing-token lock service**: append lock requests; sequence number = monotonic
  [[fencing-tokens|fencing token]] (ZooKeeper's zxid).

## Equivalence to consensus

- **Linearizable storage from TOB**: append a tentative claim, wait for it delivered
  back, first claim wins → linearizable CAS (gives sequential consistency for reads
  unless you sequence reads through the log / `sync()` / read a sync replica).
- **TOB from linearizable storage**: attach an `increment-and-get` sequence number
  (gap-free, unlike Lamport) to each message.
- Making a linearizable increment fault-tolerant **inevitably yields a consensus
  algorithm**. It is *proven* that linearizable CAS, total order broadcast, and
  [[consensus]] are **all equivalent**.

## Related concepts

- [[consensus]] · [[linearizability]] · [[lamport-timestamps]] ·
  [[coordination-services]] · [[fencing-tokens]] · [[ch11-stream-processing]]

## Sources

DDIA Ch 9 ("Total Order Broadcast").
