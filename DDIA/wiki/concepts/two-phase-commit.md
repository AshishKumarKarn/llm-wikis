---
title: Two-Phase Commit (2PC) & Atomic Commit
type: concept
chapters: [9]
tags: [distributed, transactions, atomic-commit, 2pc]
status: solid
updated: 2026-05-16
---

# Two-Phase Commit (2PC) & Atomic Commit

## The problem: distributed atomic commit

On a single node, commit hinges on one moment: the disk writing the commit record.
Across **multiple nodes** (partitioned multi-object transaction, term-partitioned
secondary index), naïvely committing on each independently can succeed on some and
fail on others → violates atomicity. A **commit is irrevocable** (others read
committed data — read-committed isolation), so a node must only commit once *certain*
all others will too. *(DDIA Ch 9)*

> **2PC ≠ 2PL.** 2PC = distributed atomic commit; [[two-phase-locking|2PL]] =
> serializable isolation. Entirely unrelated despite the names.

## How 2PC works

Introduces a **coordinator** (transaction manager). After the app reads/writes on
**participants**:

1. **Phase 1 (prepare)**: coordinator asks each participant "can you commit?". A
   participant that replies **"yes"** has written all data to disk and **surrenders
   the right to abort** — an irrevocable promise.
2. Coordinator collects votes; writes its **commit/abort decision** to its own log
   (the **commit point** — a single-node atomic commit on the coordinator).
3. **Phase 2**: send commit/abort to all; **retry forever** until each acks. A
   crashed participant that voted "yes" must commit on recovery.

Two points of no return (participant's "yes"; coordinator's logged decision) give
atomicity.

## Coordinator failure → blocking

If the coordinator crashes **after** a participant voted "yes" but before sending the
outcome, that participant is **in doubt / uncertain** — it cannot unilaterally commit
or abort (timeout doesn't help). It **must wait** for the coordinator to recover and
read its log. This is why 2PC is a **blocking** atomic commit protocol. 3PC needs
bounded delay + a perfect failure detector (unrealistic — Ch 8), so 2PC persists.

## Related concepts

- [[distributed-transactions-xa]] (XA, in-practice limitations) · [[consensus]]
  (atomic commit reduces to consensus; 2PC is a poor consensus algo — coordinator not
  elected, needs *all* votes) · [[single-vs-multi-object-transactions]] ·
  [[two-phase-locking]]

## Sources

DDIA Ch 9 ("Atomic Commit and Two-Phase Commit (2PC)").
