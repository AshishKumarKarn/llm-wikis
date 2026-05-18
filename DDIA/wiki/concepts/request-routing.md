---
title: Request Routing & Service Discovery
type: concept
chapters: [6]
tags: [partitioning, routing, service-discovery, zookeeper]
status: developing
updated: 2026-05-16
---

# Request Routing & Service Discovery

## The problem

After partitioning + rebalancing, a client asking "which IP/port serves key foo?"
needs an answer that stays current as partition→node assignments change. An instance
of the general **service discovery** problem (any HA networked software has it).
*(DDIA Ch 6)*

## Three approaches

1. **Client → any node** (round-robin LB); the node serves it or forwards to the
   owner and relays the reply.
2. **Client → routing tier** (partition-aware load balancer) which forwards.
3. **Partition-aware client** connects directly.

In all cases the routing decider must learn assignment changes — and all
participants must **agree**, a hard distributed consensus problem
([[ch09-consistency-and-consensus]]).

## How it's done

- **Coordination service**: [[zookeeper]] holds the authoritative partition→node map;
  nodes register, routing tier/clients subscribe and get notified on change. Used by
  Espresso (via Helix), HBase, SolrCloud, Kafka; MongoDB uses its own config
  server + `mongos` routing tier.
- **Gossip protocol** (Cassandra, Riak): nodes disseminate cluster-state changes
  among themselves; requests to any node are forwarded (approach 1) — more
  complexity in nodes, no external ZooKeeper dependency.
- Couchbase: no auto-rebalance, `moxi` routing tier learns from nodes. Client IPs
  themselves change slowly → DNS suffices.

## Related concepts

- [[rebalancing-partitions]] · [[zookeeper]] ·
  [[ch09-consistency-and-consensus]] (consensus) · [[rpc-vs-rest]] (service
  discovery in modern RPC)

## Sources

DDIA Ch 6 ("Request Routing").
