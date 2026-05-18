---
title: Replication Log Implementations
type: concept
chapters: [5]
tags: [replication, log, wal, cdc]
status: solid
updated: 2026-05-16
---

# Replication Log Implementations

How leader-based replication works under the hood — four methods. *(DDIA Ch 5)*

## Statement-based

Leader logs and ships every write **statement** (INSERT/UPDATE/DELETE); followers
re-execute. Compact, but breaks on **nondeterminism**: `NOW()`/`RAND()`,
autoincrement / `WHERE`-dependent statements (need identical order), side effects
(triggers/SPs). Workaround: leader substitutes deterministic values. MySQL pre-5.1;
now falls back to row-based on nondeterminism. VoltDB makes it safe by requiring
deterministic transactions.

## Write-ahead log (WAL) shipping

Ship the storage engine's append-only log itself (LSM segments, or B-tree WAL — see
[[ch03-storage-and-retrieval]]). Used by PostgreSQL, Oracle. Downside: **very
low-level** (which bytes in which disk blocks) → replication tightly coupled to
storage format → leader & follower usually can't run different versions ⇒ no
zero-downtime upgrade (which would need upgrade-followers-then-failover).

## Logical (row-based) log

A separate log format, **decoupled** from the storage engine, at row granularity
(insert: all new values; delete: PK/identifying values; update: identifier + changed
values) + a commit record. MySQL binlog (row-based). Benefits: leader/follower can
differ in version/engine; **easy for external systems to parse** → **change data
capture** (→ [[change-data-capture]], [[ch11-stream-processing]]).

## Trigger-based

Replication moved up into the application via DB **triggers/stored procedures** (or
log readers like Oracle GoldenGate). Flexible (replicate a subset, cross-DB, custom
conflict logic) but higher overhead and more bug-prone. Databus (Oracle), Bucardo
(Postgres).

## Trade-offs

Compactness (statement) vs. determinism; coupling/efficiency (WAL) vs.
upgradability/parseability (logical); built-in robustness vs. trigger flexibility.

## Related concepts

- [[single-leader-replication]] · [[log-structured-storage]] (Ch 3 log) ·
  [[change-data-capture]] · [[ch11-stream-processing]]

## Sources

DDIA Ch 5 ("Implementation of Replication Logs").
