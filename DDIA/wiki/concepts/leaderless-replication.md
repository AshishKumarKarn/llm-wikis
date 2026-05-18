---
title: Leaderless Replication (Dynamo-style)
type: concept
chapters: [5]
tags: [replication, distributed, dynamo, quorum]
status: solid
updated: 2026-05-16
---

# Leaderless Replication (Dynamo-style)

## Definition

No leader: **any replica accepts writes** directly from clients (or via a
non-ordering coordinator). Revived by Amazon's [[dynamo-paper|Dynamo]]; open-source
**Dynamo-style**: [[riak-bitcask|Riak]], [[cassandra]], [[voldemort]]. *(DDIA Ch 5)*

## Writing when a node is down

No failover. Client sends the write to all replicas in parallel; enough acks (e.g. 2
of 3) ⇒ success, the missed replica is ignored. Reads also go to several nodes in
parallel; **version numbers** decide the newest (see
[[happens-before-and-concurrency]]).

## Catching up stale replicas

- **Read repair**: on a parallel read, the client detects a stale response and writes
  the newer value back. Good for frequently-read values only.
- **Anti-entropy**: a background process continuously copies missing data between
  replicas (no ordering, possibly large delay). Not always present (Voldemort lacks
  it) — without it, rarely-read values have reduced durability.

## Quorums

Tunable n/w/r with **w + r > n** → reads and writes overlap on ≥1 up-to-date node.
Full treatment + edge cases: [[quorum-consistency]]. Network partition fallback:
[[sloppy-quorum-and-hinted-handoff]].

## Multi-datacenter

Suited to it (tolerates concurrent writes, network faults, latency). Cassandra/
Voldemort: n spans all DCs, client waits only for a local-DC quorum, cross-DC async.
Riak: n is within one DC, cross-DC async (multi-leader-like).

## Trade-offs

High availability & low latency, tolerates node loss with no failover; but only
**weak consistency** (usually none of read-after-write/monotonic/consistent-prefix),
conflicts on concurrent writes even with strict quorums → app must understand
conflict handling.

## Related concepts

- [[quorum-consistency]] · [[sloppy-quorum-and-hinted-handoff]] ·
  [[happens-before-and-concurrency]] · [[write-conflict-resolution]]
- [[single-vs-multi-vs-leaderless-replication]] · [[dynamo-paper]]

## Sources

DDIA Ch 5 ("Leaderless Replication").
