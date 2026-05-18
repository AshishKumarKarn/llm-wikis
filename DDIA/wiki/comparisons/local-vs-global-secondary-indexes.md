---
title: "Local (Document) vs. Global (Term) Secondary Indexes"
type: comparison
chapters: [6]
tags: [partitioning, secondary-index, comparison]
status: solid
updated: 2026-05-16
---

# Local (Document) vs. Global (Term) Secondary Indexes

[[secondary-indexes|Secondary indexes]] don't map neatly to partitions (they search
*occurrences of a value*, not a unique key). Two ways to partition them. *(DDIA Ch 6)*

| | Document-partitioned (**local**) | Term-partitioned (**global**) |
|---|---|---|
| Index location | with the primary data, per partition | partitioned separately by the indexed term |
| Covers | only that partition's docs | all partitions |
| Write | touches **one** partition | may touch **many** index partitions |
| Read | **scatter/gather** over all partitions | served from **one** partition |
| Read cost | tail-latency amplification ([[response-time-percentiles]]) | low |
| Write cost | low | high; often **async** (read-after-write delay) |
| Examples | MongoDB, Riak, Cassandra, ES, SolrCloud, VoltDB | DynamoDB global SI, Riak search, Oracle DW |

## The core trade-off

**Local**: cheap single-partition writes, expensive scatter/gather reads (worse with
multiple secondary-index filters at once). **Global**: efficient single-partition
reads, but a write to one document updates many index partitions — ideally a
**distributed transaction across partitions** (not universally supported →
[[ch07-transactions]], [[ch09-consistency-and-consensus]]), so in practice updates
are often **asynchronous** (e.g. DynamoDB: usually sub-second, longer under faults).

Term partitioning can itself be by term (range scans) or by hash of term (even load).
Implementation revisited in [[ch12-the-future-of-data-systems]].

## Related

- [[secondary-indexes]] · [[partitioning-basics]] · [[request-routing]]
- [[ch07-transactions]] · [[ch09-consistency-and-consensus]]

## Sources

DDIA Ch 6 ("Partitioning and Secondary Indexes").
