---
title: Concurrency in Microservices
type: scenario
tags: [concurrency, locking, idempotency, race-condition, scenarios]
sources: [scenario-questions]
created: 2026-05-15
updated: 2026-05-15
---

# Concurrency in Microservices

Concurrent reads/writes to shared data cause race conditions, lost updates, and
duplicate processing. No single technique suffices — real systems combine several
(see [[sources/scenario-questions-study-guide]]).

## Strategies

### 1. Optimistic Concurrency Control (OCC)
Use when conflicts are rare. Each record has a version number.
```sql
UPDATE orders
SET status = 'PAID', version = version + 1
WHERE id = 101 AND version = 3;
-- If 0 rows updated → version changed; retry
```
Best for: high-read REST APIs, distributed systems.

### 2. Pessimistic Locking
Lock the resource; others wait. Use when conflicts are frequent.
```sql
SELECT * FROM orders WHERE id = 101 FOR UPDATE;
```
Cons: reduces scalability; can cause deadlocks.

### 3. Distributed Locks
Required when multiple service instances run on different nodes.
```
SET resource_lock <unique_id> NX PX 30000
```
Tools: Redis (`SET NX PX`), ZooKeeper, etcd. Java libraries: Redisson, ShedLock.
Use cases: prevent duplicate job processing, single-instance scheduled tasks.

### 4. Idempotency Keys
Ensure repeated requests produce the same result.
```
POST /payments
Idempotency-Key: pay_12345
```
Server checks: if key exists → return cached response; else → process + store.
Critical for retry mechanisms and message queue consumers.
See [[concepts/delivery-semantics]].

### 5. Event Sourcing
Instead of updating a row directly, append events. No lost updates.
`UserEmailUpdated`, `UserPhoneUpdated` — state rebuilt from event log.
See [[patterns/event-sourcing]].

### 6. Saga Pattern
Replace distributed transactions with compensating transactions.
See [[patterns/saga]].

### 7. Queue-Based Serialization
Process operations one at a time by routing them through a queue.
```
User balance updates → Kafka → single consumer processes sequentially
```
Tools: Kafka, RabbitMQ, SQS.

### 8. Database-Level Constraints
Unique constraints, atomic updates, transactions as last-line defense.
```sql
INSERT INTO users(email) VALUES ('a@b.com') ON CONFLICT DO NOTHING;
```

### 9. CQRS
Separate reads (scalable) from writes (carefully controlled).
See [[patterns/cqrs]].

## 5 real-world concurrency problems

| Problem | Root cause | Solution |
|---|---|---|
| Duplicate payment | Network timeout → client retry | Idempotency key |
| Inventory overselling | Two reads of `stock=1` | Optimistic locking with version |
| Multiple workers on same job | Queue fan-out | Distributed lock |
| Distributed transaction failure | Payment fails after order created | Saga + compensating tx |
| Concurrent user profile updates | Last write wins | Event Sourcing |

## Production architecture
```
API Gateway → Microservices → Message Queue (Kafka)
                                    ↓
                              Workers
                                    ↓
                         Database (Optimistic Locking)
+ Idempotency keys + Distributed locks + Saga orchestration
```

**Golden rule:** never rely on a single technique. Combine locking + messaging +
idempotency + compensation workflows.

## Related
[[concepts/delivery-semantics]] · [[patterns/saga]] · [[patterns/event-sourcing]] ·
[[components/redis]] (distributed locks) · [[patterns/cqrs]]
