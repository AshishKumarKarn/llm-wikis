---
title: Write Conflict Resolution
type: concept
chapters: [5]
tags: [replication, conflicts, crdt, convergence]
status: solid
updated: 2026-05-16
---

# Write Conflict Resolution

## Definition

When two writes concurrently modify the same record on different leaders (multi-
leader) or replicas (leaderless), a **conflict** arises that single-leader avoids.
Replicas must **converge** — all arrive at the same final value once all changes
propagate. *(DDIA Ch 5)*

## Detection timing

Single-leader: second writer blocks or aborts+retries. Multi-leader: both writes
succeed, conflict detected **asynchronously later** (too late to ask the user).
Making detection synchronous would forfeit multi-leader's whole point (use
single-leader instead).

## Strategies

- **Conflict avoidance** (most recommended): route all writes for a given record to
  the same leader/"home" datacenter → effectively single-leader per record. Breaks
  down when the home changes (DC failure, user moves).
- **Convergent resolution**: **LWW** (highest timestamp/ID wins — popular but
  *dangerously prone to data loss*, see [[happens-before-and-concurrency]]);
  highest-replica-ID wins (also loses data); merge values (e.g. concatenate "B/C");
  record the conflict explicitly and resolve later (prompt user).
- **Custom logic**: on write (background handler, e.g. Bucardo Perl — can't prompt)
  or on read (store all conflicting versions, app/user resolves — CouchDB).
  Resolution is per row/document, not per transaction.

## Automatic resolution research

- **CRDTs** (conflict-free replicated datatypes): sets/maps/lists/counters that
  auto-merge sensibly, including deletions (Riak 2.0).
- **Mergeable persistent data structures**: Git-like explicit history, 3-way merge.
- **Operational transformation**: the algorithm behind Google Docs/Etherpad
  (ordered-list editing).

## What is a conflict?

Not always obvious — e.g. two non-overlapping room bookings on different leaders that
*together* double-book a room (an availability check on each leader passes). Scalable
detection/resolution revisited in [[ch07-transactions]] and Ch 12.

## Related concepts

- [[multi-leader-replication]] · [[leaderless-replication]] ·
  [[happens-before-and-concurrency]] (LWW, version vectors)
- [[ch07-transactions]] — write skew / the booking example

## Sources

DDIA Ch 5 ("Handling Write Conflicts", "Automatic Conflict Resolution").
