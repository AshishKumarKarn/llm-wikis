---
title: In-Memory Databases
type: concept
chapters: [3]
tags: [storage, in-memory, performance, durability]
status: developing
updated: 2026-05-16
---

# In-Memory Databases

## Definition

Databases that keep the dataset entirely in RAM (possibly across machines). Disk
data structures exist only because disks are *durable* and cheaper per GB; as RAM
gets cheaper and many datasets aren't that big, keeping data in memory becomes
feasible. *(DDIA Ch 3)*

## Durability options

Caching-only stores (Memcached) accept data loss on restart. Durable in-memory DBs
achieve durability via: battery-backed RAM; an append-only **log of changes** to
disk; periodic **snapshots**; or **replication** to other machines. On restart,
reload state from disk/replica. Disk is merely a durability log — reads are served
entirely from memory; the log also eases backup/inspection. Examples: VoltDB, MemSQL,
Oracle TimesTen (relational); RAMCloud (durable KV, log-structured in RAM *and* on
disk); [[redis]], Couchbase (weak durability, async disk write).

## The counterintuitive insight

The speedup is **not** from avoiding disk reads (a disk-based engine with enough RAM
rarely reads disk anyway — the OS caches blocks). It's from **avoiding the overhead
of encoding in-memory structures into a disk-writable form**. In-memory DBs can also
offer data models hard to do on disk (Redis: priority queues, sets).

## Beyond memory size

**Anti-caching**: evict LRU data to disk, reload on access — finer-grained than OS
virtual memory (per-record vs. per-page), but indexes must still fit in RAM (like
[[hash-index|Bitcask]]). Non-volatile memory (NVM) may reshape engine design.

## Related concepts

- [[hash-index]] · [[log-structured-storage]] · [[redis]]

## Sources

DDIA Ch 3 ("Keeping everything in memory").
