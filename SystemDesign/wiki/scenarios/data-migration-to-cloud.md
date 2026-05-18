---
title: Data Migration to Cloud
type: scenario
tags: [migration, aws, dms, datasync, snowball, scenarios]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Data Migration to Cloud

Moving large datasets from on-premises/legacy systems to cloud-hosted microservices
without impacting availability. The strategy: phased migration with parallel operation,
not big-bang cutover (see [[sources/scenario-questions-study-guide]]).

## Decision matrix

| Tool | Best for | Scale | Downtime | Key mechanism |
|---|---|---|---|---|
| **AWS DMS** | Databases (structured data) | GBs–TBs | Minimal (CDC) | Bulk load + Change Data Capture |
| **AWS DataSync** | Files & object storage | TBs–PBs | Minimal | Parallel transfer; NFS/SMB/HDFS → S3/EFS |
| **AWS Snowball** | Massive offline datasets | 10PB+ | Requires device shipping | Physical appliance; avoids network bottlenecks |
| **AWS Direct Connect** | Continuous hybrid workloads | Ongoing | None | Private high-bandwidth dedicated link |
| **Storage Gateway** | Hybrid sync | TBs | None | Cache + sync on-prem ↔ cloud |

## Phased migration approach
1. **Assess** — inventory data types, sizes, access patterns; separate hot vs. cold data.
2. **Bulk transfer** — copy cold/historical data first (Snowball or DataSync).
3. **Continuous sync** — CDC keeps source and target in sync during migration window.
4. **Dual-write period** — both legacy and cloud apps write data; reconcile regularly.
5. **Gradual traffic shift** — blue-green or canary; validate before full cutover.
6. **Decommission** — retire legacy system once new system is stable.

## AWS DMS workflow (database migrations)
```
Legacy DB (source endpoint)
    ↓ Full load (bulk copy)
Replication Instance (DMS)
    ↓ CDC (continuous delta sync)
Cloud DB — RDS / Aurora / DynamoDB (target endpoint)
    ↓ Validate
Switch traffic (Route 53 + load balancer)
    ↓
Decommission legacy
```
Supports heterogeneous migrations: Oracle → Aurora, SQL Server → PostgreSQL.

## Ensuring performance during migration
- **Staging buckets:** transfer to S3 first; microservices consume from there.
- **Caching layer:** Redis/ElastiCache reduces latency during migration window.
- **Run DMS in separate VPC** to avoid interfering with microservice traffic.
- **Monitoring:** CloudWatch metrics for throughput, lag, error rates.

## Security
- Encrypt data in transit (TLS) and at rest (SSE-KMS).
- IAM roles for controlled access during migration.
- CloudTrail audit logs for compliance.

## Interview-ready summary
"For huge data migrations, I'd use a hybrid approach: bulk transfer with Snowball or
DataSync into S3, continuous CDC sync for deltas via DMS, and a blue-green deployment
to ensure zero downtime. Performance is maintained via caching and parallel sync, while
availability is ensured by running both systems in parallel until cutover."

## Related
[[patterns/deployment-strategies]] (blue-green, canary) · [[patterns/strangler-fig]] ·
[[components/aws-aurora]] · [[components/object-storage-s3]]
