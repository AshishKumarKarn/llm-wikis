---
title: Overview
type: overview
chapters: [1,2,3,4,5,6,7,8,9,10,11,12]
tags: [synthesis]
status: solid
updated: 2026-05-17
---

# DDIA — Big-Picture Synthesis

The whole-book synthesis of *Designing Data-Intensive Applications* (Kleppmann,
2017). Start here, then drill via [[index]]. All 12 chapters ingested.

## The central question

How to build systems that are **[[reliability|reliable]]**,
**[[scalability|scalable]]**, and **[[maintainability|maintainable]]** ([[ch01-reliable-scalable-maintainable|Ch 1]]) —
and the trade-offs that pulls in. Modern apps are *data-intensive* (bounded by data
volume/complexity/change, not CPU) and assembled from composable building blocks, so
the application developer is also a **data-system designer**. The book ends
([[ch12-the-future-of-data-systems|Ch 12]]) by arguing this composition should be
explicit **dataflow** — and that engineers bear ethical responsibility for what they
build ([[data-ethics]]).

## Part I — Foundations of Data Systems (single machine)

- **[[ch01-reliable-scalable-maintainable|Ch 1]]** — the three pillars; vocabulary
  ([[fault-tolerance|fault vs. failure]], [[response-time-percentiles|tail
  latency]], [[load-parameters|load parameters]] via the Twitter timeline — revisited
  "full circle" in Ch 12).
- **[[ch02-data-models-and-query-languages|Ch 2]]** —
  [[relational-vs-document-model|relational vs. document vs. graph]]; declarative
  beats imperative; the 1970s debate ([[relational-model|CODASYL vs. relational]])
  rhymes.
- **[[ch03-storage-and-retrieval|Ch 3]]** — the [[storage-engine-index-tradeoff|index
  trade-off]]; [[btree-vs-lsm-tree|LSM vs. B-tree]]; [[oltp-vs-olap|OLTP vs. OLAP]] →
  [[column-oriented-storage|column storage]]. The **append-only
  [[log-structured-storage|log]]** introduced here recurs everywhere.
- **[[ch04-encoding-and-evolution|Ch 4]]** — [[backward-forward-compatibility|backward/
  forward compatibility]] enables rolling upgrades ([[maintainability|evolvability]]);
  [[avro|Avro]]/[[thrift-and-protocol-buffers|Protobuf]]; [[modes-of-dataflow|dataflow
  modes]]; sets up [[replication-vs-partitioning|Part II]].

## Part II — Distributed Data

- **[[ch05-replication|Ch 5]]** —
  [[single-vs-multi-vs-leaderless-replication|single/multi/leaderless]];
  [[replication-lag|lag]] → consistency models ([[read-after-write-consistency]],
  [[monotonic-reads]], [[consistent-prefix-reads]]); [[quorum-consistency|quorums]];
  [[happens-before-and-concurrency|concurrency & version vectors]].
- **[[ch06-partitioning|Ch 6]]** —
  [[key-range-vs-hash-partitioning|key-range vs. hash]];
  [[local-vs-global-secondary-indexes|secondary-index partitioning]];
  [[rebalancing-partitions|rebalancing]]; [[request-routing|routing]].
- **[[ch07-transactions|Ch 7]]** — [[acid|ACID]]; the [[isolation-levels|master
  anomaly×level matrix]] ([[read-committed]], [[snapshot-isolation]],
  [[lost-updates]], [[write-skew-and-phantoms]]); [[serializability-implementations|
  serial / 2PL / SSI]].
- **[[ch08-the-trouble-with-distributed-systems|Ch 8]]** — [[partial-failure|partial
  failure]] is the defining trait; [[unreliable-networks|unreliable networks]] &
  [[unreliable-clocks|clocks]]; [[process-pauses|pauses]];
  [[truth-by-majority|quorum truth]] + [[fencing-tokens]]; [[system-models|safety vs.
  liveness]].
- **[[ch09-consistency-and-consensus|Ch 9]]** — [[linearizability]] (vs.
  [[causal-consistency]]; [[cap-theorem|CAP]]); [[lamport-timestamps|ordering]] →
  [[total-order-broadcast]]; [[two-phase-commit|2PC]]/[[distributed-transactions-xa|
  XA]]; **[[consensus]]**. The profound equivalence: **linearizable CAS ≡ total order
  broadcast ≡ consensus**.

## Part III — Derived Data

- **[[ch10-batch-processing|Ch 10]]** — [[unix-philosophy|Unix philosophy]] →
  [[mapreduce|MapReduce]]/[[distributed-filesystem-hdfs|HDFS]]; [[batch-joins|joins]];
  [[hadoop-vs-mpp-databases|schema-on-read data lake]]; [[dataflow-engines|Spark/Flink]];
  [[pregel-graph-processing|Pregel]]. Bounded input.
- **[[ch11-stream-processing|Ch 11]]** — unbounded input;
  [[log-based-vs-amqp-jms-brokers|log brokers]]; [[change-data-capture|CDC]] &
  [[event-sourcing]]; [[state-streams-immutability|log↔state duality]];
  [[stream-joins]]; [[stream-fault-tolerance|exactly-once]].
- **[[ch12-the-future-of-data-systems|Ch 12]]** — [[data-integration|integration]] via
  log-based derived data; [[unbundling-databases|unbundling the database]] /
  [[dataflow-applications|dataflow apps]]; [[end-to-end-argument|end-to-end
  correctness]]; [[timeliness-vs-integrity|integrity ≫ timeliness]] →
  coordination-avoiding systems; [[auditing-and-integrity|trust but verify]];
  [[data-ethics|ethics]].

## Cross-cutting tensions (the spine of the book)

- **The append-only log** unifies storage ([[log-structured-storage]]), replication
  ([[replication-log-implementations]]), consensus ([[total-order-broadcast]]), and
  stream processing ([[log-based-message-brokers]], [[change-data-capture]]) — the
  book's single most reused idea.
- **Consistency vs. availability/performance** —
  [[linearizability]]/[[serializability]] are strong but slow & partition-intolerant
  ([[cap-theorem]]); [[eventual-consistency]]/[[causal-consistency]] are fast &
  available. [[timeliness-vs-integrity]] reframes it: integrity matters more.
- **Coordinate vs. avoid coordination** — [[consensus]]/[[two-phase-commit|atomic
  commit]] vs. [[enforcing-constraints-in-dataflow|log-partitioned single-writer]] +
  idempotence + [[timeliness-vs-integrity|compensating transactions]].
- **Read-time vs. write-time work** — indexes/[[materialized-views-and-data-cubes|
  materialized views]]/caches shift the [[dataflow-applications|write-path/read-path]]
  boundary (the Twitter timeline, Ch 1 ↔ Ch 12).
- **Immutability** — [[state-streams-immutability|events as the source of truth]],
  [[batch-workflow-output|human fault tolerance]], [[auditing-and-integrity|
  auditability]] — bounded by churn & privacy ([[data-ethics]]).
- **No one-size-fits-all** — every tool encodes a usage pattern; real apps **compose**
  ([[systems-of-record-and-derived-data]], [[unbundling-databases]]).
- **History rhymes** — relational vs. CODASYL ([[relational-model]]), MapReduce vs.
  MPP ([[hadoop-vs-mpp-databases]]), accounting ledgers ([[state-streams-immutability]]).

## How to use this wiki

[[index]] is the catalog (read first for any query). Highest-value reference pages:
[[isolation-levels]] (anomaly matrix), [[btree-vs-lsm-tree]],
[[single-vs-multi-vs-leaderless-replication]], [[cap-theorem]], [[consensus]],
[[change-data-capture]]. Papers are intentionally one-idea stubs; concepts &
comparisons carry the depth.
