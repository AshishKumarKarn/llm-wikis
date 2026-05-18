---
title: Riak (Bitcask)
type: system
chapters: [3]
tags: [system, storage, hash-index, leaderless]
status: stub
updated: 2026-05-16
---

# Riak (Bitcask)

Riak is a distributed key-value store; **Bitcask** is its default storage engine —
the canonical [[hash-index]] (in-memory hash map → byte offset in an append-only log;
keys must fit in RAM). LevelDB ([[leveldb-rocksdb]]) is an alternative Riak engine.

Ch 5: Riak is Dynamo-style **[[leaderless-replication]]** —
[[quorum-consistency|quorums]], **sloppy quorums on by default**, cross-DC async
(multi-leader-like), **CRDTs** for auto-merge (Riak 2.0), **dotted version vectors**
("causal context") for [[happens-before-and-concurrency|concurrent-write detection]]
and sibling merging.

> `status: stub` — Bitcask engine covered in Ch 3; leaderless detail here.

## Related

- [[hash-index]] · [[leveldb-rocksdb]] · [[leaderless-replication]] ·
  [[quorum-consistency]] · [[happens-before-and-concurrency]] · [[dynamo-paper]]
