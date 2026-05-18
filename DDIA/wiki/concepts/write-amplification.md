---
title: Write Amplification
type: concept
chapters: [3]
tags: [storage, performance, ssd]
status: developing
updated: 2026-05-16
---

# Write Amplification

## Definition

**One logical write to the database resulting in multiple physical writes to disk
over the database's lifetime.** *(DDIA Ch 3)*

## Why it matters

- In **write-heavy** apps the bottleneck is disk write rate: more physical writes per
  logical write ⇒ fewer logical writes/sec within finite disk bandwidth.
- Especially critical on **SSDs**, which tolerate only a limited number of block
  overwrites before wearing out.

## Sources of amplification

- **B-tree**: writes data ≥ twice (WAL + the tree page), plus page splits, plus
  whole-page writes even for a few changed bytes, plus some engines double-write
  pages to survive partial-page power failures.
- **LSM-tree**: repeated compaction/merge rewrites SSTables — but often *lower*
  overall amplification (workload/config dependent) and sequential, which is
  faster.

## Trade-offs

Lower write amplification and reduced fragmentation help even on SSDs (whose firmware
is itself log-structured): more compact data = more read/write requests within the
available I/O bandwidth.

## Related concepts

- [[btree-vs-lsm-tree]] — the comparison this metric decides
- [[b-tree]] · [[sstables-and-lsm-trees]] · [[log-structured-storage]]

## Sources

DDIA Ch 3 ("Advantages of LSM-trees"). Ref: Goossaert, "Coding for SSDs" (2014).
