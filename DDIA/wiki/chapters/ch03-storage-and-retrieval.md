---
title: "Ch 3 — Storage and Retrieval"
type: chapter
chapters: [3]
tags: [storage, indexes, lsm-tree, b-tree, oltp, olap, column-storage]
status: solid
updated: 2026-05-16
---

# Ch 3 — Storage and Retrieval

## One-paragraph thesis

A database fundamentally does two things: store data you give it and return it later.
This chapter takes the database's-eye view of how. An application developer won't
write a storage engine but must *choose and tune* one, which requires a rough mental
model of what it does. Two great divides organize the chapter: **OLTP vs. OLAP**
(transaction processing vs. analytics — different access patterns, different engines),
and within OLTP, **log-structured (LSM)** vs. **update-in-place (B-tree)** storage.
The unifying trade-off: indexes speed reads but slow writes; append-only designs turn
random writes into fast sequential writes. *(DDIA Ch 3)*

## Key ideas

- **[[storage-engine-index-tradeoff]]** — well-chosen indexes speed reads; *every*
  index slows writes; an index is derived data on the side. The "world's simplest
  database" (append to a file) has great writes, O(n) reads.
- **[[log-structured-storage]]** — a *log* = append-only sequence of records; the
  recurring building block of the whole book.
- **[[hash-index]]** — in-memory hash map → byte offset (Bitcask); compaction +
  segment merge; keys must fit in RAM; no range queries.
- **[[sstables-and-lsm-trees]]** — sorted segments + memtable + WAL + Bloom filters;
  size-tiered vs. leveled compaction; high write throughput.
- **[[b-tree]]** — fixed-size pages, balanced tree, in-place overwrite, WAL/redo log,
  latches, copy-on-write variants.
- **[[btree-vs-lsm-tree]]** — the central comparison; [[write-amplification]],
  read/write/space trade-offs, predictability.
- **[[secondary-indexes]]** — clustered vs. nonclustered vs. covering; heap files.
- **[[multi-dimensional-indexes]]**, **[[full-text-and-fuzzy-indexes]]** (Lucene,
  Levenshtein automata).
- **[[in-memory-databases]]** — durability via log/snapshot/replication;
  anti-caching; the real speedup is avoiding disk-encoding overhead, not avoiding
  disk reads.
- **[[oltp-vs-olap]]** → **[[data-warehousing]]** (ETL) → **[[star-and-snowflake-schema]]**
  → **[[column-oriented-storage]]** (bitmap/run-length compression, vectorized
  processing, sort orders, C-Store/Vertica) → **[[materialized-views-and-data-cubes]]**.

## Concepts introduced

- [[storage-engine-index-tradeoff]] · [[log-structured-storage]] · [[hash-index]]
- [[sstables-and-lsm-trees]] · [[b-tree]] · [[write-amplification]]
- [[secondary-indexes]] · [[multi-dimensional-indexes]] · [[full-text-and-fuzzy-indexes]]
- [[in-memory-databases]] · [[data-warehousing]] · [[star-and-snowflake-schema]]
- [[column-oriented-storage]] · [[materialized-views-and-data-cubes]]

## Comparisons introduced

- [[btree-vs-lsm-tree]] · [[oltp-vs-olap]]

## Systems / papers referenced

- [[cassandra]] (LSM, both compaction strategies), [[leveldb-rocksdb]],
  [[lucene]] (term dictionary), [[vertica-cstore]], [[riak-bitcask]]
- [[bigtable-paper]] (coined SSTable/memtable), [[lsm-tree-paper]] (O'Neil 1996),
  [[mapreduce-paper]] (SQL-on-Hadoop lineage), Dremel/Parquet

## Trade-offs & tensions

- **Reads vs. writes vs. space** — the "RUM conjecture" framing; B-tree reads vs.
  LSM writes; compaction interferes with tail latency ([[response-time-percentiles]]).
- **Append-only vs. update-in-place** — crash-recovery & concurrency simplicity &
  sequential-write speed vs. one-place-per-key (better for range locks/transactions).
- **OLTP (seek-time bound) vs. OLAP (bandwidth bound)** — drives row vs. column
  storage.
- **Materialized aggregates** — precomputed speed vs. lost query flexibility (keep
  raw data).

## Connections to other chapters

- Append-only log is the seed of replication logs ([[ch05-replication]]), event
  sourcing & stream processing ([[ch11-stream-processing]]).
- B-tree range locks → transaction isolation ([[ch07-transactions]]); copy-on-write →
  snapshot isolation.
- [[materialized-views-and-data-cubes]] is denormalized derived data →
  [[normalization-and-denormalization]], Part III.
- Column storage / SQL-on-Hadoop → [[ch10-batch-processing]].

## Open questions / things to revisit

- How does the WAL here relate to replication logs in Ch 5 and the "log" of Ch 11?
- Where do materialized views resurface as stream-processing outputs (Ch 11)?
