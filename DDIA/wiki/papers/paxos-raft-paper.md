---
title: "Consensus Algorithms: Paxos, Raft, VSR, Zab"
type: paper
chapters: [9]
tags: [paper, distributed, consensus, paxos, raft]
status: stub
updated: 2026-05-16
---

# Consensus Algorithms: Paxos, Raft, VSR, Zab

The family of fault-tolerant [[consensus]] algorithms DDIA Ch 9 references
(implementing [[total-order-broadcast]] via epoch numbering + overlapping quorums):

- **Paxos** — Lamport, "The Part-Time Parliament" (1998) / "Paxos Made Simple"
  (2001); "Paxos Made Live" (Chandra et al. 2007, engineering); **Multi-Paxos**.
- **Raft** — Ongaro & Ousterhout, "In Search of an Understandable Consensus
  Algorithm" (2014); used by etcd. Has unpleasant unreliable-link edge cases.
- **Viewstamped Replication** (Oki & Liskov 1988) · **Zab** (Junqueira et al. 2011;
  used by [[zookeeper]]).

> `status: stub` — DDIA only covers high-level common ideas. Anchor for
> [[consensus]] / [[coordination-services]].

## Related

- [[consensus]] · [[total-order-broadcast]] · [[coordination-services]] ·
  [[zookeeper]]
