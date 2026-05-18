---
title: Scheduled Jobs in Microservices
type: scenario
tags: [scheduling, distributed-lock, cron, kubernetes, scenarios]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Scheduled Jobs in Microservices

When multiple instances of a service run, naive schedulers execute the job N times.
The goal: ensure the job runs **once** (or in a controlled distributed way)
(see [[sources/scenario-questions-study-guide]]).

## Strategies

### 1. Leader Election
Only one instance becomes the leader and runs scheduled jobs. Others stay idle.
Tools: Kubernetes leader election, ZooKeeper, etcd.
Best for: cron jobs, periodic cleanup, batch processing.

### 2. Distributed Locking
All instances try to acquire a lock; only one succeeds and runs the job.
```
Acquire Lock (job_cleanup)
  If success → run job
  Else → skip
Release Lock
```
Tools: Redis (SET NX PX), ShedLock (Spring Boot annotation-based lock via DB or Redis).
Simple and widely used. Prevents duplicate execution.

### 3. External Scheduler (recommended for microservices)
Move scheduling out of the service — a centralized orchestrator triggers the job
via an API call or queue message.
```
Kubernetes CronJob
    ↓
Calls API endpoint
    ↓
Microservice processes job (idempotently)
```
Tools: Kubernetes CronJob, Apache Airflow, Temporal, Camunda.
Benefits: centralized scheduling, easy monitoring, scales better.

### 4. Queue-Based Scheduling
Scheduler pushes tasks into a message queue; workers consume them.
```
Scheduler Service → Kafka/SQS → Worker Services → DB
```
Workers must be **idempotent** (in case of duplicate deliveries).

## Comparison

| Strategy | Complexity | Best for |
|---|---|---|
| Leader Election | Medium | Monolithic jobs per cluster |
| Distributed Lock | Low | Simple one-at-a-time jobs |
| External Scheduler | Low operational | Cloud-native, microservices |
| Queue-Based | Medium | High-volume, parallelizable jobs |
| Multiple instances running same job | — | → Distributed Lock |
| Complex multi-step workflows | — | → Temporal/Camunda |

## Modern recommended approach
- **Kubernetes CronJob** for scheduling
- **Kafka** for task distribution to workers
- **Idempotent workers** for safe retries

## Related
[[components/kafka]] · [[components/redis]] (distributed lock) ·
[[components/kubernetes]] (CronJob) · [[concepts/delivery-semantics]] (idempotency)
