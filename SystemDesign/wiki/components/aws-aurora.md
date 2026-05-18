---
title: AWS Aurora
type: component
tags: [aws, aurora, database, rds, sql]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# AWS Aurora

Cloud-native relational DB (MySQL / PostgreSQL compatible) with separated compute
and storage planes (see [[sources/system-design-study-guide]]).

## Core architectural insight
Doesn't ship dirty pages — ships only **redo log records**. Storage nodes reconstruct
pages themselves. Reduces network I/O ~6×, enabling sub-10ms replica lag and sub-30s
failover. Up to 5× MySQL throughput, 3× PostgreSQL throughput.

## Storage internals
Cluster volume = 10 GB segments, each replicated **6 copies across 3 AZs (2/AZ)**.
**4-of-6 write quorum**, 3-of-6 read. Tolerates an entire AZ + one node down and still
serves reads/writes. Auto-heals degraded segments from peers. Auto-scales 10 GB → 128 TB.

## Endpoints (often missed in interviews)
- **Cluster (writer):** always points to current writer; DNS updates after failover.
- **Reader:** round-robin load balances across all read replicas.
- **Instance:** specific instance; diagnostics only.
- **Custom:** subset of instances; e.g. large replicas for analytics.

## Scaling options
Vertical (instance size) · read replicas (up to 15, near-real-time replication) · Auto
Scaling (add/remove replicas) · **Serverless v2** (scales compute in ACUs <1s, not
connections — need **RDS Proxy** for Lambda fan-out to avoid connection exhaustion).

## Advanced features (interview gold)
- **Global Database:** 1 primary + up to 5 secondaries, replication lag <1s, RPO ~1s,
  planned failover RTO <1 min. Secondaries are read-only; write forwarding adds RTT.
- **Backtrack:** rewind cluster without snapshot restore (append-only redo log, up to 72h
  window). Fast undo for human error. Not available on Global DB clusters.
- **Parallel Query:** offloads processing to storage layer.
- **ML integrations:** native SQL calls to SageMaker / Comprehend.

## When to use Aurora
✅ High-throughput OLTP · HA with sub-30s failover · read-heavy (replicas) · growing
storage · global apps.
❌ Small/simple apps (cost not justified) · tight budget · need exact MySQL/PG minor
version features.

## Related
[[comparisons/aurora-vs-rds]] · [[components/aws-lambda]] (Lambda+Aurora needs RDS Proxy)
