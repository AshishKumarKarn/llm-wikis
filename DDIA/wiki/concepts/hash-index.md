---
title: Hash Index (Bitcask)
type: concept
chapters: [3]
tags: [storage, indexes, hash, log-structured]
status: solid
updated: 2026-05-16
---

# Hash Index (Bitcask)

## Definition

The simplest log index: an **in-memory hash map from key → byte offset** in an
append-only data file. On write, append the pair and update the map; on read, look up
the offset, seek, read. Essentially what **Bitcask** (Riak's default engine) does.
*(DDIA Ch 3)*

## How it works

- High-performance reads/writes; a read can be **one disk seek**, or zero if the
  block is in the filesystem cache. Values may exceed RAM, but **all keys must fit in
  memory**.
- Avoid unbounded growth: break the log into fixed-size **segments**; **compaction**
  discards duplicate keys keeping only the latest value; **merge** adjacent segments
  in a background thread (segments are immutable; switch atomically; delete old).
  Each segment has its own hash map; lookups check newest → oldest.
- Real-world details: binary length-prefixed format (not CSV); **tombstone** records
  for deletes; crash recovery via on-disk snapshots of the hash maps; checksums for
  partially-written records; single writer thread, concurrent readers.

## Good fit / limitations

- **Good fit:** many writes per key, *few distinct keys* (e.g. URL → play count).
- **Limitations:** the hash map must fit in RAM (on-disk hash maps perform poorly:
  random I/O, costly to grow, fiddly collisions); **no efficient range queries**
  (can't scan `kitty00000`…`kitty99999`).

[[sstables-and-lsm-trees|SSTables/LSM-trees]] remove both limitations.

## Related concepts

- [[log-structured-storage]] · [[storage-engine-index-tradeoff]]
- [[sstables-and-lsm-trees]] — the sorted successor · [[riak-bitcask]]

## Sources

DDIA Ch 3 ("Hash Indexes"). Ref: Sheehy & Smith, "Bitcask" (2010).
