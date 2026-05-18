---
title: Voldemort
type: system
chapters: [5]
tags: [system, leaderless, dynamo, key-value]
status: stub
updated: 2026-05-16
---

# Voldemort

Open-source Dynamo-style ([[leaderless-replication]]) key-value store (LinkedIn).
DDIA Ch 5 notes: **no anti-entropy** process (so rarely-read values have reduced
durability — read repair only); **sloppy quorums off by default**; multi-datacenter
within the normal leaderless model (n spans DCs).

> `status: stub` — Ch 5 only.

## Related

- [[leaderless-replication]] · [[quorum-consistency]] ·
  [[sloppy-quorum-and-hinted-handoff]] · [[dynamo-paper]]
