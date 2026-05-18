---
title: Slow DB Writes
type: scenario
tags: [database, performance, cqrs, sharding, scenarios]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Slow DB Writes

Systematic approach to diagnosing and fixing database write performance degradation
(see [[sources/scenario-questions-study-guide]]).

## Step 1: Confirm & measure
Tools: `pg_stat_statements` (PostgreSQL), Performance Schema (MySQL), CloudWatch.
Metrics to check:
- Write latency (P95/P99)
- Throughput (writes/sec)
- CPU / memory / disk IOPS
- Lock waits / deadlocks
- Slow query logs

## Diagnostic questions
- Are writes single-row or batch? Batching reduces per-row overhead.
- Are there too many indexes? Each index slows every write.
- Are transactions too large? Break into smaller ones.
- Is there lock contention? Hot rows = serialized writes.
- Are triggers or constraints slowing writes? Disable/redesign.

## Solutions (in order of increasing complexity)

### 1. Query & schema optimization
- **Remove unused indexes** — each index is an overhead on every write.
- **Break large transactions** into smaller, faster units.
- **Batch writes** — fewer round trips, better throughput.

### 2. Handle lock contention
- Row-level locking instead of table locks.
- Reduce hot rows (e.g., avoid all writes updating the same counter row).
- Partition tables to distribute write load.

### 3. Table partitioning
Partition by Date, Region, or Customer ID. Large tables slow every write path.

### 4. Async write processing
Don't write synchronously under high traffic.
```
Application → Kafka/SQS → Worker → Database
```
Smooths traffic spikes; improves perceived throughput. See [[components/kafka]].

### 5. Write sharding
Split writes across multiple database instances.
```
UserID 1–1M → DB1
UserID 1M–2M → DB2
```
See [[patterns/data-partitioning]].

### 6. CQRS
Separate read and write models. Reads go to replicas; writes only hit the primary.
Heavy reads no longer slow writes. See [[patterns/cqrs]].

### 7. Database configuration tuning
PostgreSQL: `wal_buffers`, `checkpoint_timeout`, `shared_buffers`
MySQL: `innodb_log_file_size`, `innodb_flush_log_at_trx_commit`

### 8. Infrastructure upgrades
- HDD → NVMe SSD
- Higher IOPS (AWS Provisioned IOPS storage)
- More memory for buffer pool

### 9. Architecture change for extreme write-heavy workloads
```
App → Kafka → Stream Processor → DB
```
Or switch to a write-optimized store: **Apache Cassandra**, **Amazon DynamoDB**.

## Summary checklist
1. Measure with monitoring tools
2. Analyze queries and indexes
3. Reduce lock contention
4. Batch writes
5. Partition tables
6. Introduce async processing
7. Scale with sharding
8. Tune database config
9. Upgrade hardware
10. Consider architecture redesign

## Related
[[patterns/cqrs]] · [[patterns/data-partitioning]] · [[components/kafka]] ·
[[patterns/n-plus-1-query]] · [[components/aws-aurora]]
