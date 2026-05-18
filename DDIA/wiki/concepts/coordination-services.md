---
title: Coordination & Membership Services (ZooKeeper)
type: concept
chapters: [9]
tags: [distributed, coordination, zookeeper, consensus, service-discovery]
status: solid
updated: 2026-05-16
---

# Coordination & Membership Services (ZooKeeper)

## What they are

ZooKeeper / etcd: not general-purpose databases but **outsourced consensus, failure
detection, and membership**. Hold a *small* amount of data (fits in memory) replicated
via fault-tolerant [[total-order-broadcast]]. Modeled on Google's **Chubby** lock
service. Used indirectly by HBase, YARN, Kafka, Nova, etc. Run on a **fixed small
number of nodes** (3–5) doing majority votes, serving many clients. *(DDIA Ch 9)*

## The useful feature set

- **Linearizable atomic operations** — atomic CAS implements a fault-tolerant
  distributed **lock/lease** (only this really needs [[consensus]]).
- **Total ordering of operations** — monotonic `zxid`/`cversion` serve as
  **[[fencing-tokens]]** (prevent process-pause conflicts).
- **Failure detection** — long-lived sessions + heartbeats; on session timeout, the
  client's **ephemeral nodes** vanish (auto-release locks).
- **Change notifications** — watch for other clients joining/failing without polling.

## What it's good for

- **Allocating work**: leader/primary election; deciding which partition → which
  node, rebalancing on join/fail (atomic ops + ephemeral nodes + notifications;
  Apache Curator helps). Data is **slow-changing** ("node X leads partition 7"), not
  runtime app state.
- **Service discovery** (also Consul): register endpoints. Doesn't actually *need*
  consensus (DNS is fine, stale OK) — but leader election does, and read-only
  caching replicas can serve non-linearizable discovery reads.
- **Membership services**: couple failure detection with consensus → agreement on
  which nodes are live (may be wrong, but *agreed*).

## Related concepts

- [[consensus]] · [[total-order-broadcast]] · [[linearizability]] ·
  [[fencing-tokens]] · [[request-routing]] · [[zookeeper]] · [[chubby-paper]]

## Sources

DDIA Ch 9 ("Membership and Coordination Services").
