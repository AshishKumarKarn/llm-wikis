---
title: "Design: Distributed Number Inventory"
type: scenario
tags: [distributed-transactions, race-conditions, saga, redis, scenarios]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Design: Distributed Number Inventory

**Prompt (Vonage):** Customers search, reserve, and buy virtual phone numbers. Prevent
double-booking when two customers click Buy simultaneously across the world
(see [[sources/system-design-study-guide]], p132).

## Core challenge
Phone numbers = finite, globally distributed, regulated resources with race conditions
during reservation. System must handle millions of concurrent searches while preventing
double-selling and respecting country compliance.

## Key design decisions

### Search — Elasticsearch
Multi-dimensional queries (country, area code, pattern, capabilities, price). ES contains
AVAILABLE numbers only. **ES is an eventually consistent read replica — Postgres owns
the source of truth.**

### Reservation — Redis SET NX PX (the crux)
Two customers can both see the same number as available.
```
SET number:{id} reserved NX PX 600000   -- 10-min TTL
```
`NX` = atomic "set if not exists". Only one wins; loser gets 409 Conflict + alternatives.
After Redis lock set → synchronously write `status=RESERVED` to Postgres (write-behind:
Redis is the fast gate, Postgres is the durable record). See [[components/redis]].

### Purchase — Saga pattern
Steps: billing → compliance (KYC for regulated countries) → carrier provisioning.
**Any step can fail.** Model as a saga with compensating transactions: carrier failure
after billing → automatic refund. Idempotency keys on every step.
See [[patterns/saga]], [[concepts/delivery-semantics]].

### Shard key — country_code
Shard Postgres by country because inventory is naturally geographic, carrier APIs are
regional, compliance rules are country-scoped. No cross-shard joins on hot paths.

### TTL sweep job
Background scheduler releases expired reservations: query `WHERE expires_at < NOW()`,
release Redis lock, reset `status=AVAILABLE`, re-index to ES. Keeps inventory fresh.

### Compliance
Germany/India/Brazil require proof of address or business registration. Compliance
service gates purchase and can place numbers in QUARANTINE. Most candidates miss this.

## Related
[[components/redis]] · [[patterns/saga]] · [[concepts/delivery-semantics]] ·
[[scenarios/high-volume-notification-dispatcher]]
