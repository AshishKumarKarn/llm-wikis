---
title: Log-Structured Storage (the Log)
type: concept
chapters: [3]
tags: [storage, log, append-only]
status: developing
updated: 2026-05-16
---

# Log-Structured Storage (the Log)

## Definition

In DDIA, a **log** is *an append-only sequence of records* — not necessarily
human-readable, possibly binary, intended for other programs. The log-structured
*school* of storage engines only ever appends to files and deletes obsolete files;
it never updates a file in place. (Contrast: the update-in-place school — see
[[b-tree]].) *(DDIA Ch 3)*

## Why it matters

The append-only log is one of the most reused building blocks in the whole book:
storage engines here, replication logs ([[ch05-replication]]), write-ahead logs for
crash recovery, and the event log of [[ch11-stream-processing]].

## Why append-only is good (not wasteful)

- **Sequential writes** (append + segment merge) are much faster than random writes
  on HDDs, and preferable on SSDs too — see [[btree-vs-lsm-tree]] and
  [[write-amplification]].
- **Crash recovery & concurrency are simpler**: an immutable/append-only segment
  can't be left half-overwritten with spliced old/new bytes; segments are read
  concurrently while merges happen in the background and swap atomically.
- **Segment merging** avoids data-file fragmentation over time.

Members of the log-structured school: Bitcask, [[sstables-and-lsm-trees|SSTables/
LSM-trees]], LevelDB, RocksDB, [[cassandra]], HBase, [[lucene]].

## Trade-offs

- Multiple copies of a key may exist across segments (vs. B-tree's one place per
  key) — matters for range locks / transactions ([[ch07-transactions]]).
- Background compaction competes for disk bandwidth and can spike tail latency.

## Related concepts

- [[hash-index]] · [[sstables-and-lsm-trees]] · [[b-tree]] · [[write-amplification]]
- [[btree-vs-lsm-tree]] — log-structured vs. update-in-place head-to-head

## Sources

DDIA Ch 3 ("Data Structures That Power Your Database"; Summary). Ref: Rosenblum &
Ousterhout, log-structured filesystem (1992).
