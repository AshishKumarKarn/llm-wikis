---
title: Dual Write Problem
type: pattern
tags: [dual-write, consistency, event-driven, patterns]
sources: [design-patterns]
created: 2026-05-15
updated: 2026-05-15
---

# Dual Write Problem

Occurs when an application must write the same business change to two different
systems (DB + Kafka, DB + cache, DB + Elasticsearch) and cannot guarantee
atomicity across them (see [[sources/design-patterns-study-guide]]).

## The failure scenario
```
1. Save Order to DB     ✅
2. Publish to Kafka     ❌  (network failure, broker down)

Result:
  DB: order exists
  Downstream: never heard about it
  → Broken audit trails, inconsistent state, silent data loss
```

## Why it's hard
- Distributed systems don't support atomic transactions across heterogeneous stores.
- Network failures, retries, timeouts are normal.
- 2PC is slow, locks resources, and fails under partitions.

## Solutions (best → worst)

### ✅ 1. Transactional Outbox Pattern (canonical)
Write domain data and event to the **same DB transaction**.
Separate publisher process publishes the event later.
See [[patterns/outbox-pattern]] for full details.
Used by: Netflix, Uber, AWS.

### ✅ 2. Change Data Capture (CDC)
App writes only to DB. CDC tool (Debezium, AWS DMS) reads the DB transaction log
and streams changes to Kafka. No dual write in application code at all.
- Pro: extremely reliable, scales well.
- Con: event format tied to DB schema; infra complexity.

### ⚠️ 3. Idempotent Writes + Retry (mitigation, not a fix)
Use idempotency keys; retry safely; detect duplicates.
Still inconsistent during failures; complex recovery. Use only as a complement.

### ❌ 4. Two-Phase Commit (2PC)
Slow, locks resources, poor failure handling. Almost always rejected in modern
microservices (see [[patterns/saga]] for the modern alternative).

### ❌ 5. Try-Catch Both Writes (naive)
```java
try { writeDB(); writeKafka(); } catch {}
```
Leads to silent data loss and broken downstream systems in production.

## Summary
The preferred solution is **Outbox** or **CDC**. Everything else is either
a workaround or an anti-pattern at scale.

## Related
[[patterns/outbox-pattern]] · [[patterns/saga]] · [[patterns/cqrs]] ·
[[concepts/delivery-semantics]]
