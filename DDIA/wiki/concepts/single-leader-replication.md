---
title: Single-Leader Replication
type: concept
chapters: [5]
tags: [replication, distributed, leader-follower]
status: solid
updated: 2026-05-16
---

# Single-Leader Replication

## Definition

(a.k.a. active/passive, master–slave) One replica is the **leader** (master/primary):
clients send all **writes** to it. The leader writes locally then streams a
**replication log / change stream** to **followers** (read replicas / hot standbys),
which apply writes **in the same order**. Reads can go to leader or any follower
(followers are read-only). *(DDIA Ch 5)*

## Why it matters

The most common approach — built into PostgreSQL (≥9.0), MySQL, Oracle Data Guard,
SQL Server AlwaysOn, MongoDB, RethinkDB, Espresso; also Kafka, RabbitMQ HA queues.
Popular because it's easy to understand and has **no conflict resolution**.

## Synchronous vs. asynchronous

- **Synchronous follower**: leader waits for its ack before reporting success →
  guaranteed up-to-date copy, but if it stalls *all writes block*.
- **Asynchronous**: leader doesn't wait → fast, leader keeps serving even if
  followers lag, but **unreplicated writes are lost** if the leader fails
  unrecoverably (durability not guaranteed even after client ack).
- **Semi-synchronous**: one follower synchronous, rest async; if the sync one
  stalls, an async one is promoted → ≥2 up-to-date nodes. Fully async is widely used
  (esp. many/geo followers) → see [[replication-lag]].

## Operations

- **Setting up a follower**: consistent snapshot (no lock) + its exact log position
  (PostgreSQL log sequence number / MySQL binlog coords) → copy → request changes
  since snapshot → "caught up".
- **Follower failure**: catch-up recovery from its local log.
- **Leader failure**: **[[failover-and-split-brain|failover]]** (promote a follower)
  — fraught.

## Trade-offs

Simplicity & no conflicts vs. a single write bottleneck and failover risk. Read
scaling works only with async followers → [[replication-lag]] anomalies.

## Related concepts

- [[replication-log-implementations]] · [[failover-and-split-brain]] ·
  [[replication-lag]]
- [[multi-leader-replication]] · [[leaderless-replication]] ·
  [[single-vs-multi-vs-leaderless-replication]]

## Sources

DDIA Ch 5 ("Leaders and Followers", "Synchronous Versus Asynchronous Replication").
