---
title: Google Spanner
type: system
chapters: [2, 8]
tags: [system, relational, distributed, globally-distributed, truetime]
status: developing
updated: 2026-05-16
---

# Google Spanner

Google's globally-distributed database. First appears in DDIA Ch 2 for **locality**:
it provides document-like [[data-locality]] *within a relational model* by letting a
schema declare a table's rows be interleaved (nested) within a parent table.

Ch 8: **TrueTime API** — uniquely **exposes the clock's confidence interval** as
`[earliest, latest]` (most APIs don't). Spanner deliberately **waits out the
interval** before committing a read-write transaction so causally-later transactions
get non-overlapping intervals → correct ordering → distributed
[[snapshot-isolation]] across datacenters. Needs GPS/atomic clocks per DC (~7 ms
uncertainty). See [[unreliable-clocks]].

> `status: developing` — consensus / externally-consistent distributed transactions
> expand at [[ch09-consistency-and-consensus]].

## Related

- [[unreliable-clocks]] · [[snapshot-isolation]] · [[data-locality]] ·
  [[ch09-consistency-and-consensus]]
