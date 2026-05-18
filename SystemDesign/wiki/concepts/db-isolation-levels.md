---
title: Database Isolation Levels & MVCC
type: concept
tags: [database, isolation, mvcc, transactions, consistency, concepts]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# Database Isolation Levels & MVCC

The ACID "I" (Isolation) is a spectrum, not binary. Isolation levels are the knobs to
trade off consistency vs. throughput. Understanding anomalies and MVCC is a
differentiator at senior/SA level (see [[sources/miscellaneous-study-guide]]).

## The 4 Read Anomalies

### 1. Dirty Read
Reading **uncommitted** data from another transaction.
```
T1: UPDATE balance = 500  (not committed)
T2: SELECT balance → 500  ← dirty read
T1: ROLLBACK
T2: acted on data that never existed
```

### 2. Non-Repeatable Read
Same row read twice in same transaction returns **different values** because another
transaction committed in between.
```
T1: SELECT balance → 100
T2: UPDATE balance = 200, COMMIT
T1: SELECT balance → 200  ← different result
```

### 3. Phantom Read
Same **range query** returns different **row count** because rows were inserted/deleted.
```
T1: SELECT * WHERE age > 25 → 5 rows
T2: INSERT row with age=30, COMMIT
T1: SELECT * WHERE age > 25 → 6 rows  ← phantom
```

### 4. Lost Update
Two transactions read same value, both update — one overwrites the other.
```
T1: SELECT balance → 100
T2: SELECT balance → 100
T1: SET balance = 150, COMMIT
T2: SET balance = 130, COMMIT  ← T1's +50 is lost
```

## The 4 Standard Isolation Levels (SQL-92)

| Level | Dirty Read | Non-Repeatable | Phantom | Lost Update |
|---|---|---|---|---|
| **READ UNCOMMITTED** | ✅ possible | ✅ possible | ✅ possible | ✅ possible |
| **READ COMMITTED** | ❌ prevented | ✅ possible | ✅ possible | ✅ possible |
| **REPEATABLE READ** | ❌ prevented | ❌ prevented | ✅ possible* | ❌ prevented |
| **SERIALIZABLE** | ❌ prevented | ❌ prevented | ❌ prevented | ❌ prevented |

*MySQL InnoDB and PostgreSQL both prevent phantoms at REPEATABLE READ via MVCC/gap locks.

### READ UNCOMMITTED
Lowest isolation, highest throughput. Almost never used. Use case: approximate analytics
(live dashboards showing rough counts).

### READ COMMITTED ← most common default
Only reads committed data. Default in PostgreSQL, Oracle, SQL Server. Each SELECT gets a
fresh snapshot — same query run twice can return different results within the same transaction.

### REPEATABLE READ
MySQL InnoDB default. Re-reading a row gives the same result. MySQL also prevents
phantoms via gap locks (stronger than SQL-92 spec).

### SERIALIZABLE
Highest isolation. Transactions behave as if running serially. Prevents all anomalies.
Significant performance cost — use only on critical paths.

## MVCC (Multi-Version Concurrency Control)

Most modern databases use MVCC instead of pure locking.

```
T1 writes: balance = 200 [version 2]
         : balance = 100 [version 1, kept for old readers]

T3 (started before T1 committed) → reads balance = 100  (old version)
T4 (started after T1 committed)  → reads balance = 200  (new version)
```

**Benefits:**
- Readers never block writers ← key insight
- Writers never block readers
- Each transaction gets a consistent snapshot

**Downsides:**
- Storage overhead (old row versions)
- Vacuum/garbage collection overhead (PostgreSQL `AUTOVACUUM`)
- Long-running transactions prevent cleanup → table bloat

## DB-Specific Behaviors (senior-level knowledge)

### PostgreSQL
- Default: **READ COMMITTED**
- REPEATABLE READ: uses snapshot from **first statement** in transaction
- SERIALIZABLE: uses **SSI (Serializable Snapshot Isolation)** — optimistic, tracks read/write
  dependencies, aborts conflicting transactions. Better throughput than locking.
- READ UNCOMMITTED silently upgraded to READ COMMITTED (Postgres never does dirty reads)
- Phantoms prevented at REPEATABLE READ level

### MySQL InnoDB
- Default: **REPEATABLE READ**
- Uses **gap locks** at REPEATABLE READ to prevent phantoms — but can cause more deadlocks
- READ COMMITTED disables gap locks → better concurrency, but phantoms possible

### SQL Server
- Default: **READ COMMITTED** (lock-based)
- `READ_COMMITTED_SNAPSHOT` (RCSI) gives READ COMMITTED semantics using row versioning
- Snapshot Isolation (SI): transaction-level snapshot, prevents lost updates

## Locking vs MVCC
| Approach | Readers block writers? | Writers block readers? |
|---|---|---|
| Lock-based | Yes | Yes |
| MVCC | No | No |
| PostgreSQL SSI | No | No |

## Practical patterns

### Optimistic Locking (application-level)
```sql
UPDATE accounts
SET balance = 150, version = version + 1
WHERE id = 1 AND version = 5;
-- If 0 rows updated: throw OptimisticLockException, retry
```
Best for: low contention, read-heavy workloads.

### Pessimistic Locking — SELECT FOR UPDATE
```sql
BEGIN;
SELECT balance FROM accounts WHERE id = 1 FOR UPDATE;
-- row is locked; others block until COMMIT
UPDATE accounts SET balance = 150 WHERE id = 1;
COMMIT;
```
Best for: high contention, financial transactions, inventory.

### SKIP LOCKED — distributed job queue
```sql
SELECT id FROM jobs
WHERE status = 'pending'
ORDER BY created_at LIMIT 1
FOR UPDATE SKIP LOCKED;
```
Multiple workers safely pick jobs without blocking each other.

### Deadlock prevention
- Always acquire locks in the **same order** across transactions
- Keep transactions short — reduces lock hold time
- Use NOWAIT or SKIP LOCKED where appropriate
- Catch deadlock exceptions and retry at application level

## Write Skew (distributed anomaly)
Not prevented even by Serializable in some DBs:
```
T1 reads: doctors_on_call = 2 → removes one doctor
T2 reads: doctors_on_call = 2 → removes one doctor
Both commit → doctors_on_call = 0 (invariant violated!)
```
Only PostgreSQL SSI and truly serializable distributed systems prevent write skew.

## Isolation in distributed systems
Standard isolation assumes a single DB node. Distributed DBs make different choices:
| Database | Isolation |
|---|---|
| CockroachDB | Serializable (SSI) by default |
| Google Spanner | Serializable (TrueTime + 2PC) |
| DynamoDB | Eventually consistent; strong consistency optional |
| Cassandra | No transactions; tunable consistency (quorum) |
| MongoDB | Snapshot isolation for multi-document transactions |

## Quick mental model
```
READ UNCOMMITTED → fastest, almost never use
READ COMMITTED   → default for most DBs, good for OLTP
REPEATABLE READ  → MySQL default; re-reads must be consistent
SERIALIZABLE     → financial critical paths; use sparingly
```

**The right level is always a trade-off: higher isolation = fewer anomalies = more lock
contention = lower throughput. Pick the weakest level that preserves your correctness invariants.**

## Interview / system design pointers
- Financial systems → REPEATABLE READ or SERIALIZABLE + SELECT FOR UPDATE
- High-read analytics → READ COMMITTED; possibly READ UNCOMMITTED for approximations
- Job queues → READ COMMITTED + SKIP LOCKED
- Inventory/booking → pessimistic or optimistic locking with retry
- Microservices → can't use DB transactions across services → use [[patterns/saga]] instead
- Long-running transactions → avoid; holds locks / prevents MVCC cleanup
- Mention SSI (PostgreSQL) vs gap locks (MySQL) to show depth

## Related
[[concepts/acid-transactions]] · [[scenarios/concurrency-in-microservices]] ·
[[patterns/saga]] · [[components/aws-aurora]]
