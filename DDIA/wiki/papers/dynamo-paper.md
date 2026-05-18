---
title: "Dynamo: Amazon's Highly Available Key-Value Store (DeCandia et al., 2007)"
type: paper
chapters: [5]
tags: [paper, replication, leaderless, quorum, distributed]
status: developing
updated: 2026-05-16
---

# Dynamo: Amazon's Highly Available Key-Value Store

Giuseppe DeCandia, Deniz Hastorun, Madan Jampani, et al., 21st ACM SOSP, Oct 2007.

The paper that revived **[[leaderless-replication]]**. Source of the techniques DDIA
Ch 5 builds on: tunable **[[quorum-consistency|quorums]]** (n/w/r), **read repair**
and **anti-entropy**, **[[sloppy-quorum-and-hinted-handoff]]**, version vectors for
**[[happens-before-and-concurrency|concurrent-write detection]]**, and conflict
siblings. Inspired open-source "Dynamo-style" stores: [[cassandra]],
[[riak-bitcask|Riak]], [[voldemort]].

> Not the same as AWS **DynamoDB** (a hosted product using single-leader
> replication). Also cited in Ch 1's references for p99.9 tail-latency targets.

## Related

- [[leaderless-replication]] · [[quorum-consistency]] ·
  [[sloppy-quorum-and-hinted-handoff]] · [[happens-before-and-concurrency]]

## Sources

DDIA Ch 5; DeCandia et al. (SOSP 2007).
