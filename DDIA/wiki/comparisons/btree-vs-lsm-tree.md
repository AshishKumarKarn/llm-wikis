---
title: "B-Tree vs. LSM-Tree"
type: comparison
chapters: [3]
tags: [storage, b-tree, lsm-tree, comparison, performance]
status: solid
updated: 2026-05-16
---

# B-Tree vs. LSM-Tree

The central storage-engine comparison of [[ch03-storage-and-retrieval]]:
update-in-place ([[b-tree]]) vs. log-structured ([[sstables-and-lsm-trees]]).

> Rule of thumb: **LSM-trees are typically faster for writes; B-trees are typically
> faster for reads.** But benchmarks are inconclusive and workload-sensitive — *test
> with your own workload*. *(DDIA Ch 3)*

| Dimension | LSM-tree | B-tree |
|---|---|---|
| Write path | Sequential SSTable writes; high throughput | In-place page overwrite + WAL; random writes |
| Read path | Slower — check memtable + several SSTables (Bloom filters help) | Faster — one place per key |
| [[write-amplification]] | Compaction rewrites data, but often lower overall | WAL + page + splits; whole-page writes for few bytes |
| Space | Better compression, periodic defrag, lower overhead (esp. leveled) | Page fragmentation leaves unused space |
| Tail latency | Compaction can interfere → high p99/p999 | More predictable |
| Transactions | Key in many segments | **Key in exactly one place → range locks attach to the tree** (see [[ch07-transactions]]) |
| Maturity | Newer, increasingly popular in new datastores | Very mature, ingrained, in all major RDBMSs |

## Why LSM writes win

Sequential SSTable writes vs. overwriting several B-tree pages; lower
[[write-amplification]] (workload-dependent) — critical on HDDs (sequential ≫
random) and SSDs (limited block-erase cycles; less amplification = more usable I/O
bandwidth even though SSD firmware itself is log-structured).

## Why LSM can lose

Background **compaction competes for finite disk bandwidth** with foreground writes;
the bigger the DB, the more bandwidth compaction needs. If it can't keep up,
unmerged segments pile up (disk fills, reads slow) and SSTable engines typically
**don't throttle writes** → needs explicit monitoring. B-tree latency is more
predictable at high [[response-time-percentiles|percentiles]].

## Verdict

No quick rule — test empirically. B-trees won't disappear (predictable, transactional
fit); log-structured indexes increasingly popular in new datastores.

## Related

- [[b-tree]] · [[sstables-and-lsm-trees]] · [[write-amplification]] ·
  [[log-structured-storage]]
- [[ch07-transactions]] — why one-place-per-key matters for isolation

## Sources

DDIA Ch 3 ("Comparing B-Trees and LSM-Trees"). Refs: Callaghan, "Advantages of an
LSM vs a B-Tree" (2016); the RUM Conjecture (EDBT 2016).
