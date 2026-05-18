---
title: Linearizability
type: concept
chapters: [9]
tags: [distributed, consistency, linearizability, recency]
status: solid
updated: 2026-05-16
---

# Linearizability

## Definition

(a.k.a. atomic / strong / immediate / external consistency) Make a system **appear as
if there is only one copy of the data and all operations on it are atomic**. It is a
**recency guarantee**: as soon as one client's write completes, all subsequent reads
(by any client) must see that value — no stale replica/cache. *(DDIA Ch 9)*

## What makes it linearizable

Each operation appears to take effect atomically at one point between its start and
end; once any read returns a new value, all later reads must too (the value flips
once, never back). The football example (Figure 9-1): Bob, hitting reload *after*
hearing Alice's score, must not see an older state. Testable (expensively) by checking
recorded request/response timings form a valid sequential order.

## Linearizability ≠ serializability

See [[linearizability-vs-serializability]]: serializability is a *transaction
isolation* property (multi-object); linearizability is a *recency* property on a
single register. Both together = strict serializability. 2PL & serial execution are
typically linearizable; **SSI is not** (reads from a stale-by-design snapshot).

## When it's needed

- **Locking & leader election** — the lock must be linearizable so all nodes agree
  who holds it (avoid [[failover-and-split-brain|split brain]]); ZooKeeper/etcd.
- **Uniqueness constraints** (username, no negative balance, no double-booking) — a
  hard constraint needs a single agreed up-to-date value (≈ CAS).
- **Cross-channel timing dependencies** — the image-resizer race (file storage +
  message queue): without recency, two channels race.

## Implementing & cost

- Single-leader (reads from leader/sync followers): *potentially* linearizable (but
  not if it uses snapshot isolation, or a delusional leader). **Consensus**:
  linearizable safely (ZooKeeper/etcd). **Multi-leader**: not (concurrent writes).
  **Leaderless**: probably not — LWW & sloppy quorums ruin it; even strict quorums
  can be nonlinearizable ([[quorum-consistency]]) unless you do synchronous read
  repair + read-quorum-before-write (and a linearizable CAS needs
  [[consensus]] regardless).
- **Cost** ([[cap-theorem]]): under a network partition, a linearizable system must
  become unavailable on the minority side. And it is **slow even without faults** —
  Attiya & Welch: response time ≥ network-delay uncertainty. Weaker models (causal)
  can be much faster.

## Related concepts

- [[linearizability-vs-serializability]] · [[causal-consistency]] (weaker, faster) ·
  [[cap-theorem]] · [[total-order-broadcast]] · [[consensus]]
- [[quorum-consistency]] · [[eventual-consistency]] · [[coordination-services]]

## Sources

DDIA Ch 9 ("Linearizability"). Refs: Herlihy & Wing 1990; Attiya & Welch 1994;
Gilbert & Lynch (CAP).
