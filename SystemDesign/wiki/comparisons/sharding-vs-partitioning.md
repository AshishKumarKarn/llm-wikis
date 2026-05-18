---
title: Sharding vs Partitioning
type: comparison
tags: [sharding, partitioning, database, scaling, comparisons]
sources: [miscellaneous, system-design]
created: 2026-05-16
updated: 2026-05-16
---

# Sharding vs Partitioning

Often confused but distinct approaches. Partitioning organizes data **within** a single
database instance; sharding distributes data **across** multiple instances
(see [[sources/miscellaneous-study-guide]]).

## Key comparison

| Feature | Partitioning | Sharding |
|---|---|---|
| Scope | Within a single database | Across multiple databases/servers |
| Goal | Improve query performance | Improve scalability |
| Physical location | Same server (usually) | Different servers |
| Complexity | Lower | Higher |
| Scaling type | Vertical / logical | Horizontal |
| Cross-boundary queries | Easy (same DB optimizer) | Difficult (requires routing logic) |

## Partitioning types (within one DB)

| Type | How | Best for |
|---|---|---|
| **Range** | Rows split by value ranges | Logs/time-series by date |
| **List** | Rows split by discrete values | Regional data by country/region |
| **Hash** | Rows distributed using hash function | Even distribution; avoids hot spots |
| **Composite** | Combine two methods (e.g., range + hash) | Large systems needing balance |
| **Vertical** | Columns split into separate tables | Wide tables with rarely-accessed columns |
| **Functional** | Split by domain/service | Microservices DB isolation |

```sql
-- Range partition example (PostgreSQL)
CREATE TABLE orders_2024 PARTITION OF orders
FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```

Benefits: query pruning (only scan relevant partition), faster range scans, easier archival.
Trade-offs: range partitions can be unbalanced if data skews; hash partitions make range
queries harder.

## Sharding
Data split across multiple server instances by a **shard key**.
```
User IDs 1–1M    → Shard 1 (Server A)
User IDs 1M–2M   → Shard 2 (Server B)
```
Each shard handles a subset of data independently. Requires application-level or
middleware routing to the correct shard.

### Shard key selection (critical)
- High cardinality: many distinct values → even distribution
- Avoid hot spots: don't shard on a column that concentrates writes (e.g., `created_at`
  with sequential time in a busy table)
- See [[concepts/hot-key-hot-partition]] for the failure mode

## When to use each

**Use Partitioning when:**
- You need better query performance on large tables
- You want to prune scans by time, region, or category
- You need easier archival (drop a partition instead of deleting rows)
- Within a single service's database

**Use Sharding when:**
- A single DB instance can't handle the write or storage load
- You need true horizontal scaling across machines
- You've exhausted vertical scaling options

## Practical guidance
1. Start with partitioning within a single DB — simpler, no routing complexity.
2. Add read replicas for read scaling.
3. Only shard when single-node DB is a proven bottleneck.
4. FAANG pattern: range partitioning for time-series; hash partitioning for load balancing.

## Related
[[patterns/data-partitioning]] · [[concepts/hot-key-hot-partition]] ·
[[concepts/consistent-hashing]] (shard routing strategy) · [[scenarios/slow-db-writes]]
