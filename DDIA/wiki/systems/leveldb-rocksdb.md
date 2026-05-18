---
title: LevelDB & RocksDB
type: system
chapters: [3]
tags: [system, storage, lsm-tree, embedded]
status: stub
updated: 2026-05-16
---

# LevelDB & RocksDB

Embeddable key-value storage-engine libraries implementing the
[[sstables-and-lsm-trees|LSM-tree]] algorithm (memtable + SSTables + background
compaction). Both use **leveled compaction** (the source of "Level"DB's name).
LevelDB can replace [[riak-bitcask|Bitcask]] in Riak.

> `status: stub` — Ch 3 only. The canonical modern LSM engines.

## Related

- [[sstables-and-lsm-trees]] · [[btree-vs-lsm-tree]] · [[cassandra]]
