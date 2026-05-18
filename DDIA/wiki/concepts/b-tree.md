---
title: B-Tree
type: concept
chapters: [3]
tags: [storage, b-tree, indexes, update-in-place]
status: solid
updated: 2026-05-16
---

# B-Tree

## Definition

The most widely used indexing structure (introduced 1970, called "ubiquitous" by
1979; standard in nearly all relational DBs and many non-relational). Like SSTables
it keeps key-value pairs **sorted by key** (efficient lookups + range queries), but
its design philosophy is **update-in-place**: the database is broken into fixed-size
**pages/blocks** (traditionally 4 KB), read/written one page at a time. *(DDIA Ch 3)*

## How it works

- Pages reference each other by on-disk address, forming a tree. One **root**; each
  page holds keys + child references; each child owns a contiguous key range; descend
  until a **leaf page** (value inline or a reference).
- **Branching factor** = child references per page (typically several hundred). Tree
  stays balanced: depth O(log n); 3–4 levels is usually enough (4-level, 4 KB pages,
  branching 500 → 256 TB).
- **Update**: find leaf, change value, write page back (references stay valid).
  **Insert**: find the range's page; if full, **split** into two half-full pages and
  update the parent.

## Making B-trees reliable

- The basic op **overwrites a page in place** (a real hardware op; SSDs erase/rewrite
  large blocks). Multi-page ops (a split rewrites 2 children + parent) risk a
  **corrupted index** (orphan page) if it crashes midway.
- **Write-ahead log (WAL / redo log)** — append-only; every modification written
  there *before* being applied; replayed to restore consistency after a crash.
- **Latches** (lightweight locks) protect the tree under concurrent access (a
  complication the background-merging log-structured approach avoids).

## Optimizations

Copy-on-write (LMDB — no WAL, also enables snapshot isolation, see
[[ch07-transactions]]); abbreviated keys in interior pages (higher branching → fewer
levels; the B+ tree variant); laying leaf pages sequentially on disk (hard to
maintain as the tree grows); leaf sibling pointers; fractal trees (borrow
log-structured ideas).

## Trade-offs vs. LSM

Each key exists in **exactly one place** → attractive for **range locks** in
transactional databases; reads thought faster; but [[write-amplification]] (WAL +
page + splits, whole-page writes for few changed bytes) and fragmentation. Full
comparison: [[btree-vs-lsm-tree]].

## Related concepts

- [[sstables-and-lsm-trees]] · [[log-structured-storage]] · [[write-amplification]]
- [[secondary-indexes]] · [[ch07-transactions]] (range locks, snapshot isolation)

## Sources

DDIA Ch 3 ("B-Trees"). Refs: Bayer & McCreight 1970; Comer "The Ubiquitous B-Tree"
1979; Graefe "Modern B-Tree Techniques" 2011.
