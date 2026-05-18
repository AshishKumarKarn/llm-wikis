---
title: Object Storage (AWS S3)
type: component
tags: [aws, s3, storage, object-storage]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Object Storage — AWS S3

Objects inside buckets (globally unique name, region-specific). Each object has data,
metadata, unique key (see [[sources/system-design-study-guide]]).

## Core properties
- **Durability:** 11 nines (99.999999999%) — multi-AZ replication.
- **Availability:** 99.99% (Standard).
- **Consistency:** strong read-after-write for PUTs and DELETEs.
- **Not** block storage — unsuitable for low-latency transactional workloads.

## Storage classes
Standard (frequent access) · Intelligent-Tiering (auto cost-optimize) · Standard-IA /
One Zone-IA (infrequent) · Glacier / Glacier Deep Archive (archival, minutes–hours
retrieval).

## Security
IAM policies (identity-centric) · bucket policies (resource-centric) · ACLs (legacy) ·
encryption SSE-S3/SSE-KMS/client-side · Block Public Access · VPC endpoints (private).

## Performance
Multipart uploads (>100 MB) · Transfer Acceleration (CloudFront edge) · prefix
partitioning (avoid hot prefixes) · S3 Byte-Range Fetches · Requester Pays buckets.

## Common architectures
Static website (S3 + CloudFront) · Data lake (S3 + Athena/Redshift Spectrum) ·
Backup/DR (cross-region replication CRR) · event-driven (S3 → Lambda/SNS/SQS on
object events).

## Interview mnemonic: SAFE DATA
**S**ecurity · **A**vailability & Durability · **F**lexibility (storage classes) ·
**E**vent-driven integrations · **D**ata lifecycle · **A**ccess optimization ·
**T**otal cost control · **A**rchitecture use cases.

## Related
[[components/aws-aurora]] · [[components/aws-lambda]] ·
[[scenarios/multi-tenant-usage-dashboard]] (cold storage tier)
