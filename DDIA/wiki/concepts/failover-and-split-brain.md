---
title: Failover & Split Brain
type: concept
chapters: [5]
tags: [replication, distributed, fault-tolerance, failover]
status: solid
updated: 2026-05-16
---

# Failover & Split Brain

## Definition

**Failover** = promoting a follower to leader after a leader failure, reconfiguring
clients and other followers to the new leader. Can be manual or automatic. *(DDIA
Ch 5)*

## Automatic failover steps

1. **Detect failure** — no foolproof method; usually a **timeout** (no response for
   ~30 s ⇒ assumed dead).
2. **Choose a new leader** — election by majority, or appointed by a controller;
   best candidate has the most up-to-date data. Agreeing on a leader is a
   **consensus** problem → [[ch09-consistency-and-consensus]].
3. **Reconfigure** — clients write to the new leader; the old leader must step down
   and become a follower if it returns.

## What goes wrong (the hazards)

- **Lost writes**: with async replication the new leader may lack the old leader's
  recent writes; the common fix — discard them — violates durability. Dangerous when
  coordinated with outside systems (GitHub incident: a promoted stale MySQL follower
  reused autoincrement primary keys already used in Redis → private data leaked to
  wrong users).
- **Split brain**: two nodes both believe they're leader and both accept writes →
  data lost/corrupted. Safety mechanism: shut one down — **fencing** /
  **STONITH** ("Shoot The Other Node In The Head"); if poorly designed, can shut
  *both* down (deepened in [[ch09-consistency-and-consensus]] "the leader and the
  lock").
- **Timeout tuning**: too long ⇒ slow recovery; too short ⇒ unnecessary failovers
  (a load spike/network glitch), which worsen an already-struggling system.

> No easy solutions → some ops teams prefer **manual** failover. These (node
> failures, unreliable networks, consistency/durability/availability/latency
> trade-offs) are *fundamental* distributed-systems problems → Ch 8–9.

## Related concepts

- [[single-leader-replication]] · [[ch08-the-trouble-with-distributed-systems]]
- [[ch09-consistency-and-consensus]] — consensus, fencing tokens

## Sources

DDIA Ch 5 ("Handling Node Outages").
