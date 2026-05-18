---
title: SSTables and LSM-Trees
type: concept
chapters: [3]
tags: [storage, lsm-tree, sstable, log-structured]
status: solid
updated: 2026-05-16
---

# SSTables and LSM-Trees

## Definition

An **SSTable** (Sorted String Table) is a log segment whose key-value pairs are
**sorted by key**, with each key appearing once per merged segment. An **LSM-tree**
(Log-Structured Merge-Tree, O'Neil et al. 1996) is a storage engine built from a
cascade of SSTables merged and compacted in the background. *(DDIA Ch 3)*

## Why sorting helps (advantages over [[hash-index]])

1. **Merging is efficient** even when files exceed memory — mergesort-style: read
   inputs side by side, emit lowest key; newer segment wins on duplicates.
2. **Sparse in-memory index suffices** — to find `handiwork`, jump to the known
   offset of `handbag` and scan to the next known key; one index entry per few KB.
3. **Block compression** — group the scanned range into a block, compress it; the
   sparse index points at compressed-block starts (saves disk + I/O bandwidth).

## How it works (constructing/maintaining)

- Writes go to an in-memory balanced tree (red-black/AVL) — the **memtable**.
- When the memtable exceeds a threshold (few MB), write it out as a new SSTable
  (already sorted); serve reads memtable → newest → oldest segment.
- Background **compaction/merge** combines segments, discarding overwritten/deleted
  values.
- Crash safety: an unsorted **append-only log** records every write to restore the
  memtable; discarded once the memtable is flushed.

Used by LevelDB/RocksDB ([[leveldb-rocksdb]]), [[cassandra]]/HBase (from the
[[bigtable-paper]], which coined SSTable/memtable), and [[lucene]]'s term dictionary
(in-memory index is a finite-state automaton/trie → Levenshtein automaton for fuzzy
search — see [[full-text-and-fuzzy-indexes]]).

## Performance optimizations

- **Bloom filters** — memory-efficient set approximation; avoid disk reads for keys
  that definitely don't exist (the LSM weak spot: a missing key checks every level).
- **Compaction strategies**: **size-tiered** (smaller→larger SSTables; HBase) vs.
  **leveled** (key range split into levels; more incremental, less disk; LevelDB/
  RocksDB; Cassandra supports both).

## Trade-offs

High write throughput (sequential writes) and good range queries; reads can be slower
(check several structures); compaction can spike tail latency and, if it can't keep
up, segments pile up. Full head-to-head: [[btree-vs-lsm-tree]].

## Related concepts

- [[log-structured-storage]] · [[hash-index]] · [[b-tree]] · [[write-amplification]]
- [[column-oriented-storage]] — reuses the LSM memtable+merge trick for column writes

## Sources

DDIA Ch 3 ("SSTables and LSM-Trees"). Refs: [[lsm-tree-paper]] (O'Neil 1996);
[[bigtable-paper]].
