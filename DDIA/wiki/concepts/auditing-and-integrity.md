---
title: Auditing & Integrity ("Trust, but Verify")
type: concept
chapters: [12]
tags: [correctness, auditing, integrity, merkle-tree]
status: developing
updated: 2026-05-16
---

# Auditing & Integrity ("Trust, but Verify")

## Why verify

System models take a **binary** view of faults (this can happen, that can't); reality
is **probabilistic**. Disk data corrupts untouched; network corruption evades TCP
checksums; memory bit-flips happen (even *rowhammer* — pathological access flipping
bits in fault-free RAM). Software bugs slip past checksums — even MySQL (uniqueness)
and PostgreSQL (SSI write skew) have had integrity bugs; app code far more so (and
often misuses DB integrity features — "feral concurrency control"). ACID
"consistency" only holds if transactions are bug-free. ⇒ data corruption is
inevitable eventually; we need to **detect** it (= **auditing**). *(DDIA Ch 12)*

## Don't blindly trust

Mature systems (HDFS, S3) **don't fully trust disks** — background processes
continually re-read, compare replicas, and move data to mitigate silent corruption.
Test backup restores. The ACID-database culture bred **blind trust** + neglected
auditability; then weaker NoSQL guarantees made blind trust more dangerous.

## Designing for auditability

Mutating transactions obscure *why* changes happened. **Event-based systems audit
better**: user input = one immutable event; state derived by deterministic,
repeatable functions. Hash the event log to detect corruption; re-derive state to
check it matches (or run a redundant derivation in parallel). Explicit dataflow makes
**provenance** clear and enables time-travel debugging. Best done **end-to-end**
([[end-to-end-argument]]) — checking a whole derived pipeline implicitly covers every
disk/network/service/algorithm along the path; continuous checks → confidence → move
faster.

## Cryptographic auditing

**Merkle trees** (hash trees) efficiently prove a record is in a dataset; used by
**certificate transparency** (TLS cert validity) and distributed ledgers/blockchains
(mutually-untrusting replicas check each other + consensus). DDIA is skeptical of
blockchain Byzantine/proof-of-work aspects ([[byzantine-faults]]) but finds the
integrity-checking ideas promising for general data systems if made scalable.

## Related concepts

- [[end-to-end-argument]] · [[timeliness-vs-integrity]] · [[byzantine-faults]] ·
  [[system-models]] · [[reliability]]

## Sources

DDIA Ch 12 ("Trust, but Verify").
