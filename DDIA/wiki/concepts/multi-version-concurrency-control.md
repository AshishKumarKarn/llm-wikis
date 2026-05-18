---
title: Multi-Version Concurrency Control (MVCC)
type: concept
chapters: [7]
tags: [transactions, mvcc, isolation, concurrency-control]
status: developing
updated: 2026-05-16
---

# Multi-Version Concurrency Control (MVCC)

## Definition

Keeping **several committed versions of an object side by side** so different
in-progress transactions can see the database state at different points in time. The
standard mechanism behind [[snapshot-isolation]] (and read committed). *(DDIA Ch 7)*

## How it works

- Read committed alone needs only **2 versions** (committed + uncommitted). Snapshot
  isolation keeps **many** — read committed uses a fresh snapshot *per query*,
  snapshot isolation the *same* snapshot for the whole transaction.
- PostgreSQL-style: each row tagged `created_by` / `deleted_by` txid; **update =
  delete + create**; a garbage-collection process reclaims versions no transaction
  can still see. Visibility decided by [[snapshot-isolation|visibility rules]].
- Append-only/copy-on-write [[b-tree]] variant (CouchDB, Datomic, LMDB): each write
  creates a new immutable tree root = a point-in-time snapshot; needs background
  compaction.
- Never updating in place ⇒ consistent snapshots at small overhead. Reused by
  [[serializable-snapshot-isolation]] to detect stale reads.

## Related concepts

- [[snapshot-isolation]] · [[read-committed]] · [[serializable-snapshot-isolation]]
- [[b-tree]] (copy-on-write variant) · [[happens-before-and-concurrency]]
  (the replicated multi-version analogue)

## Sources

DDIA Ch 7 ("Implementing snapshot isolation").
