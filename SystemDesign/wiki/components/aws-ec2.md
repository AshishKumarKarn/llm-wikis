---
title: AWS EC2 & Auto Scaling
type: component
tags: [aws, ec2, compute, auto-scaling]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# AWS EC2 & Auto Scaling Groups

Resizable virtual servers (see [[sources/system-design-study-guide]]).

## Instance families
General Purpose (T, M) · Compute Optimized (C) · Memory Optimized (R, X, z) ·
Storage Optimized (I, D, H) · Accelerated (P, G, Inf — GPU/ML) · Mac/bare-metal (niche).

## Purchasing models
On-Demand (max flexibility) · Reserved 1–3yr (up to 75% savings) · Savings Plans
(flexible commitment, ~72%) · **Spot** (up to 90% cheaper, interruptible — not for
critical) · Dedicated Hosts/Instances (compliance/licensing).

## Storage options
EBS (persistent block, snapshots; gp3/io2/st1/sc1 types) · Instance Store (ephemeral,
very fast — caching/temp buffers only) · EFS (shared NFS, multi-instance) · S3 (object,
see [[components/object-storage-s3]]) · FSx (managed Windows/Lustre).

## Placement groups
Cluster (low-latency HPC; reduces fault tolerance) · Spread (fault tolerance; hardware
racks) · Partition (large distributed systems: HDFS, Cassandra).

## Auto Scaling Groups
Launch template → Desired/Min/Max capacity → scaling policies (dynamic/scheduled/
predictive). Health checks replace unhealthy instances. Lifecycle hooks for custom
warm-up scripts. Termination policies control which instance is removed first.
Stateful apps need session persistence (Redis/RDS) before scaling horizontally —
see [[concepts/horizontal-scaling]].

## Key CloudWatch metrics
CPU Utilization · Network In/Out · Disk I/O · Status Checks · EBS VolumeQueueLength ·
GroupInServiceInstances (ASG).
