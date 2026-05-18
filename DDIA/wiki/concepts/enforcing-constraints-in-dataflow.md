---
title: Enforcing Constraints in Dataflow
type: concept
chapters: [12]
tags: [correctness, constraints, consensus, dataflow]
status: developing
updated: 2026-05-16
---

# Enforcing Constraints in Dataflow

## Uniqueness requires consensus

Enforcing a uniqueness constraint (username, seat, no-negative-balance, no-overlap)
in a distributed setting **requires [[consensus]]** — decide which of several
conflicting requests wins. Usual method: a single leader. Scale by **partitioning on
the value that must be unique** (route same-username requests to one partition).
Async multi-master is ruled out (concurrent conflicting accepts). To reject violations
immediately, synchronous coordination is unavoidable. *(DDIA Ch 12)*

## Uniqueness via log-based messaging

A partitioned log gives total order broadcast (= consensus). Partition by the
unique-value hash; a **single-threaded stream processor** reads that partition
sequentially, tracks taken values in a local DB, emits success/rejection to an output
stream; the client watches for its outcome. Scales by adding partitions. Works for
*any* constraint (the processor runs arbitrary validation logic) — same idea as
[[total-order-broadcast|implementing linearizable CAS via TOB]] / Bayou.

## Multi-partition request processing without atomic commit

Money transfer touching request-ID, payer, payee partitions: traditionally needs an
atomic commit across all three. Instead:

1. Client appends the request (unique ID) to a log partitioned by request ID —
   **single-object atomic write**, no multi-partition commit.
2. A deterministic stream processor emits a debit (partitioned by payer) and a
   credit (partitioned by payee), carrying the request ID.
3. Downstream processors apply changes, **deduplicating by request ID**
   ([[end-to-end-argument|end-to-end op IDs]]); crash → resume from checkpoint,
   reprocess deterministically, dedup handles duplicates.

Same correctness (each request applied exactly once to both accounts) **without an
atomic commit protocol**. Add a payer-balance-validating processor to reject
overdrafts before step 1.

## Related concepts

- [[consensus]] · [[total-order-broadcast]] · [[end-to-end-argument]] ·
  [[timeliness-vs-integrity]] · [[stream-fault-tolerance]] · [[two-phase-commit]]

## Sources

DDIA Ch 12 ("Enforcing Constraints").
