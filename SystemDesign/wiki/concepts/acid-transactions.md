---
title: ACID Transactions
type: concept
tags: [acid, transactions, database, consistency, concepts]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# ACID Transactions

The four properties that guarantee reliable database transactions
(see [[sources/miscellaneous-study-guide]]).

## The 4 Properties

| Property | Guarantee | Example |
|---|---|---|
| **Atomicity** | All-or-nothing — either all steps complete or none | Transfer $100: debit AND credit, or neither |
| **Consistency** | DB moves from one valid state to another; rules/constraints remain valid | Foreign keys, unique constraints always satisfied |
| **Isolation** | Concurrent transactions don't interfere | Two users updating same account get correct balance |
| **Durability** | Committed data survives crashes | Transaction logs, disk persistence, replication |

## ATM withdrawal example
```
1. Deduct $100 from account
2. Record transaction
3. COMMIT

Atomicity:  both steps succeed, or neither (rollback on failure)
Consistency: account can't go below 0 (constraint)
Isolation:  another ATM withdrawal doesn't interfere
Durability: power cut after COMMIT → data is still saved
```

## Why ACID matters
- Prevents silent failures: money can't be deducted without being credited.
- Guarantees reliability in banking, e-commerce, healthcare.
- Supports safe concurrency.
- Enables crash recovery via transaction logs.

## ACID vs BASE
| | ACID | BASE |
|---|---|---|
| Used by | RDBMS (PostgreSQL, MySQL, Oracle) | NoSQL (MongoDB, Cassandra) |
| Model | Strong consistency | Eventually consistent |
| Trade-off | Reliability | Scalability |

BASE = **B**asically **A**vailable, **S**oft state, **E**ventually consistent.

## Local vs distributed transactions
Local ACID transactions work within a single service and database.
They **cannot span** multiple services or databases.
For cross-service transactions → use [[patterns/saga]] (compensating transactions) instead
of 2PC (two-phase commit).

## Isolation is a spectrum
ACID isolation is not binary — see [[concepts/db-isolation-levels]] for the 4 levels
(READ UNCOMMITTED → SERIALIZABLE) and their trade-offs.

## Related
[[concepts/db-isolation-levels]] · [[patterns/saga]] · [[scenarios/concurrency-in-microservices]]
