---
title: "Key-Range vs. Hash Partitioning"
type: comparison
chapters: [6]
tags: [partitioning, comparison]
status: solid
updated: 2026-05-16
---

# Key-Range vs. Hash Partitioning

The two main partitioning schemes of [[ch06-partitioning]]. *(DDIA Ch 6)*

| | [[key-range-partitioning\|Key-range]] | [[hash-partitioning\|Hash]] |
|---|---|---|
| Partition owns | a contiguous key range | a range of key *hashes* |
| Key order | preserved (sorted) | destroyed |
| Range queries | **efficient** | inefficient (scatter to all) |
| Load distribution | risk of hot spots on sequential keys | even |
| Boundaries | adapt to data (admin/DB) | evenly or pseudo-randomly spaced |
| Rebalancing | dynamic split/merge | usually fixed count / proportional |
| Examples | Bigtable, HBase, RethinkDB, Mongo <2.4 | Cassandra, Mongo hash mode, Voldemort |

## The hybrid

**Cassandra's compound primary key**: hash only the first column (even load across
partitions), use remaining columns as a concatenated sort index (range scans *within*
a fixed first column). Best of both for one-to-many data like
`(user_id, update_timestamp)`. DDIA Summary: hybrid approaches via compound keys are
a general option.

## Choosing

Need range scans / sorted access → key-range (accept hot-spot risk, mitigate with
key prefixes). Need even load and only point lookups → hash. Neither fully fixes a
single hot key — see [[skewed-workloads-and-hot-spots]].

## Related

- [[key-range-partitioning]] · [[hash-partitioning]] · [[rebalancing-partitions]] ·
  [[skewed-workloads-and-hot-spots]]

## Sources

DDIA Ch 6 ("Partitioning of Key-Value Data", Summary).
