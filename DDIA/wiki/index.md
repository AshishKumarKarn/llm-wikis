---
title: Index
type: index
updated: 2026-05-17
---

# DDIA Wiki — Index

The content catalog. Every wiki page is listed here with a one-line summary.
Read this first when answering a query, then drill into the relevant pages.

> Status: **complete** — all 12 chapters ingested; overview.md rebuilt; lint pass done.

## Overview

- [[overview]] — the big-picture synthesis of the whole book; start here for orientation.

## Chapters

- [[ch01-reliable-scalable-maintainable]] — Ch 1: the three pillars (reliability, scalability, maintainability) and the vocabulary for each.
- [[ch02-data-models-and-query-languages]] — Ch 2: relational vs. document vs. graph; declarative queries; history rhymes.
- [[ch03-storage-and-retrieval]] — Ch 3: LSM vs. B-tree; OLTP vs. OLAP; column storage; the index trade-off.
- [[ch04-encoding-and-evolution]] — Ch 4: backward/forward compat; Avro/Protobuf/Thrift; dataflow modes; Part II intro.
- [[ch05-replication]] — Ch 5: single/multi/leaderless replication; lag & consistency models; quorums; conflicts.
- [[ch06-partitioning]] — Ch 6: key-range vs. hash; secondary-index partitioning; rebalancing; routing.
- [[ch07-transactions]] — Ch 7: ACID; isolation levels & race conditions; serializability (serial/2PL/SSI).
- [[ch08-the-trouble-with-distributed-systems]] — Ch 8: partial failure; unreliable networks/clocks; pauses; quorum truth.
- [[ch09-consistency-and-consensus]] — Ch 9: linearizability; causality/ordering; total order broadcast; 2PC; consensus.
- [[ch10-batch-processing]] — Ch 10: Unix philosophy; MapReduce/HDFS; joins; dataflow engines; Pregel.
- [[ch11-stream-processing]] — Ch 11: messaging/log brokers; CDC & event sourcing; stream joins; exactly-once.
- [[ch12-the-future-of-data-systems]] — Ch 12: data integration; unbundling; dataflow; correctness; ethics.

## Concepts

- [[reliability]] — working correctly even when things go wrong; fault classes; conscious cost trade-off.
- [[fault-tolerance]] — fault (component off-spec) vs. failure (system fails user); deliberate fault injection.
- [[scalability]] — not a label but a question: how do we cope as load grows?
- [[load-parameters]] — describing load; the Twitter fan-out-on-write/read/hybrid case study.
- [[response-time-percentiles]] — p50/p95/p99/p999, tail latency, SLOs, why means lie.
- [[shared-nothing-architecture]] — scale up vs. out; elastic vs. manual; stateless easy, stateful hard.
- [[maintainability]] — operability, simplicity (accidental complexity), evolvability.
- [[data-model-layering]] — nested abstractions: app objects → general model → bytes → hardware.
- [[relational-model]] — relations of tuples; the query-optimizer insight that beat CODASYL.
- [[document-model]] — nested self-contained docs; reverted to hierarchical, not CODASYL.
- [[normalization-and-denormalization]] — store meaning once vs. duplicate; many-to-one forces joins.
- [[object-relational-impedance-mismatch]] — why ORMs exist and what they can't hide.
- [[data-locality]] — related data stored together; whole-doc speed vs. large-doc waste.
- [[mapreduce-querying]] — pure map/reduce between declarative & imperative; NoSQL reinvents SQL.
- [[graph-data-models]] — property graph / triple-store / Datalog; Cypher, SPARQL.
- [[storage-engine-index-tradeoff]] — indexes speed reads, slow writes; the world's simplest DB.
- [[log-structured-storage]] — the append-only log: the book's most-reused building block.
- [[hash-index]] — in-memory hash map → file offset (Bitcask); keys must fit in RAM.
- [[sstables-and-lsm-trees]] — sorted segments + memtable + WAL + Bloom filters + compaction.
- [[b-tree]] — fixed pages, in-place overwrite, WAL/redo log, latches, copy-on-write.
- [[write-amplification]] — one logical write → many physical writes; SSD wear.
- [[secondary-indexes]] — clustered vs. covering vs. heap file.
- [[multi-dimensional-indexes]] — concatenated keys; R-trees for geospatial.
- [[full-text-and-fuzzy-indexes]] — Lucene term dictionary; Levenshtein automata.
- [[in-memory-databases]] — durability via log/snapshot; speedup = no disk-encoding overhead.
- [[data-warehousing]] — read-only ETL'd copy; SQL-on-Hadoop ecosystem.
- [[star-and-snowflake-schema]] — fact + dimension tables; dimensional modeling.
- [[column-oriented-storage]] — store columns together; bitmap/RLE compression; vectorized.
- [[materialized-views-and-data-cubes]] — precomputed denormalized aggregates; flexibility cost.
- [[backward-forward-compatibility]] — new-reads-old / old-reads-new; rolling upgrades.
- [[data-encoding-formats]] — language-specific vs. textual vs. binary schema-driven.
- [[thrift-and-protocol-buffers]] — schema + field tags; tag-based evolution.
- [[avro]] — writer's vs. reader's schema; name-based; dynamic schemas.
- [[schema-evolution]] — merits of schemas; per-format add/remove/rename rules.
- [[modes-of-dataflow]] — databases / services / message passing; data outlives code.
- [[message-passing-dataflow]] — message brokers & distributed actors.
- [[replication-vs-partitioning]] — the two ways data is distributed (Part II).
- [[single-leader-replication]] — leader takes writes, streams log to followers; sync/async.
- [[multi-leader-replication]] — many leaders; multi-DC/offline/collab; write conflicts.
- [[leaderless-replication]] — Dynamo-style; any replica writes; read repair/anti-entropy.
- [[failover-and-split-brain]] — promoting a follower; split brain, fencing/STONITH.
- [[replication-log-implementations]] — statement / WAL / logical / trigger-based.
- [[change-data-capture]] — *(stub)* logical log → external systems; expand Ch 11.
- [[replication-lag]] — async staleness; the three anomalies & their guarantees.
- [[eventual-consistency]] — replicas converge "eventually"; quantifying it.
- [[read-after-write-consistency]] — see your own writes.
- [[monotonic-reads]] — never see time go backward.
- [[consistent-prefix-reads]] — causally-ordered writes seen in order.
- [[write-conflict-resolution]] — avoidance, convergence, LWW, custom, CRDTs/OT.
- [[quorum-consistency]] — w + r > n; the many edge cases.
- [[sloppy-quorum-and-hinted-handoff]] — write availability under partition; not a real quorum.
- [[happens-before-and-concurrency]] — defining concurrency; LWW danger; version vectors, siblings.
- [[partitioning-basics]] — sharding; skew & hot spots; partitioning ⟂ replication.
- [[key-range-partitioning]] — sorted ranges; range scans; sequential-key hot spots.
- [[hash-partitioning]] — even load, no range scans; "consistent hashing" trap.
- [[skewed-workloads-and-hot-spots]] — single hot key; app-level key salting.
- [[rebalancing-partitions]] — not hash mod N; fixed/dynamic/proportional; auto vs. manual.
- [[request-routing]] — three approaches; ZooKeeper vs. gossip; service discovery.
- [[parallel-query-execution]] — *(stub)* MPP analytics; expand Ch 10.
- [[acid]] — atomicity/consistency/isolation/durability; C is the app's, not the DB's.
- [[single-vs-multi-object-transactions]] — why multi-object; CAS isn't a transaction.
- [[transaction-aborts-and-retries]] — aborts enable safe retry; the caveats.
- [[read-committed]] — no dirty reads/writes; the common default.
- [[snapshot-isolation]] — consistent snapshot via MVCC; "repeatable read" naming chaos.
- [[multi-version-concurrency-control]] — many object versions side by side.
- [[lost-updates]] — read-modify-write clobber; atomic ops / locks / CAS / detection.
- [[write-skew-and-phantoms]] — premise invalidated by concurrent write; only serializable fixes.
- [[serializability]] — strongest level; prevents all race conditions.
- [[actual-serial-execution]] — one thread; stored procedures; partition to scale.
- [[two-phase-locking]] — pessimistic; predicate/index-range locks; deadlocks.
- [[serializable-snapshot-isolation]] — optimistic; non-blocking; abort on conflict.
- [[partial-failure]] — the defining trait of distributed systems; cloud vs. HPC.
- [[unreliable-networks]] — ambiguous non-responses; timeouts; congestion/queueing.
- [[unreliable-clocks]] — drift/jumps; LWW timestamp danger; confidence intervals.
- [[process-pauses]] — GC/VM freezes; the lease bug; real-time is expensive.
- [[truth-by-majority]] — a node can't trust itself; quorum decides who's dead.
- [[fencing-tokens]] — monotonic token the resource checks to block stale writers.
- [[byzantine-faults]] — lying nodes; usually out of scope; weak forms of lying.
- [[system-models]] — sync/partial/async; crash-stop/recovery/Byzantine; safety vs. liveness.
- [[linearizability]] — single-copy recency illusion; total order; the cost & uses.
- [[cap-theorem]] — "Consistent or Available when Partitioned"; narrow; best avoided.
- [[causal-consistency]] — partial order; strongest model that's partition-tolerant & fast.
- [[lamport-timestamps]] — (counter, node ID) total order consistent with causality.
- [[total-order-broadcast]] — reliable + totally-ordered delivery; ≡ consensus.
- [[two-phase-commit]] — distributed atomic commit; in-doubt/coordinator-failure blocking.
- [[distributed-transactions-xa]] — DB-internal vs. heterogeneous; XA; limitations.
- [[consensus]] — agreement/integrity/validity/termination; FLP; epoch + quorums.
- [[coordination-services]] — ZooKeeper/etcd/Chubby: locks, fencing, membership.
- [[systems-of-record-and-derived-data]] — Part III framing; source of truth vs. derived.
- [[unix-philosophy]] — do one thing well; uniform interface; logic↔wiring; the Hadoop template.
- [[mapreduce]] — mapper→shuffle→reducer; computation near data; transparent fault tolerance.
- [[distributed-filesystem-hdfs]] — shared-nothing, NameNode, replication/erasure coding.
- [[batch-joins]] — reduce-side sort-merge vs. map-side hash; GROUP BY; hot-key skew.
- [[batch-workflow-output]] — immutable index/KV files; human fault tolerance.
- [[dataflow-engines]] — Spark/Tez/Flink; avoid materialization; RDD lineage; declarative APIs.
- [[pregel-graph-processing]] — BSP "think like a vertex"; iterative graph batch.
- [[event-streams-and-messaging]] — events/producers/consumers; brokers; load-balance vs. fan-out.
- [[log-based-message-brokers]] — partitioned append-only log; offsets; replay; circular buffer.
- [[change-data-capture]] — DB→stream; fixes dual writes; snapshots & log compaction.
- [[event-sourcing]] — immutable app-level events; commands vs. events; replay to state.
- [[state-streams-immutability]] — log↔state duality; "the log is the truth"; CQRS; limits.
- [[stream-processing-uses]] — CEP, stream analytics, materialized views, search on streams.
- [[reasoning-about-time-in-streams]] — event vs. processing time; stragglers; window types.
- [[stream-joins]] — stream-stream / stream-table / table-table; slowly changing dimensions.
- [[stream-fault-tolerance]] — exactly-once; microbatch/checkpoint; atomic commit; idempotence.
- [[data-integration]] — derive everything from one ordered source; vs. distributed transactions.
- [[lambda-architecture]] — parallel batch+stream; its problems; unifying the two.
- [[unbundling-databases]] — DB features as composable dataflow components; the "database inside-out".
- [[dataflow-applications]] — app code as derivation; subscribe-don't-poll; write vs. read path.
- [[end-to-end-argument]] — correctness needs end-to-end op IDs; low-level dedup insufficient.
- [[enforcing-constraints-in-dataflow]] — uniqueness via log partitioning; multi-partition w/o atomic commit.
- [[timeliness-vs-integrity]] — integrity ≫ timeliness; coordination-avoiding systems.
- [[auditing-and-integrity]] — "trust, but verify"; designing for auditability; Merkle trees.
- [[data-ethics]] — predictive-analytics bias, surveillance, privacy, "data as pollution".

## Systems

- [[redis]] — *(stub)* datastore used as a queue; the dual of Kafka.
- [[postgresql]] — *(stub)* canonical relational DB; converging with JSON/XML.
- [[mongodb]] — *(stub)* canonical document DB; aggregation pipeline.
- [[neo4j]] — *(stub)* property-graph DB; origin of Cypher.
- [[google-spanner]] — *(stub)* globally-distributed relational DB; locality via interleaving.
- [[cassandra]] — *(stub)* Bigtable column-family; LSM storage; leaderless (expand Ch 5–6).
- [[leveldb-rocksdb]] — *(stub)* embeddable LSM-tree engines; leveled compaction.
- [[lucene]] — *(stub)* full-text engine; SSTable term dictionary; Levenshtein automata.
- [[vertica-cstore]] — *(stub)* columnar warehouse; multiple sort orders.
- [[riak-bitcask]] — *(stub)* Riak KV; Bitcask hash-index; leaderless, CRDTs, dotted version vectors.
- [[voldemort]] — *(stub)* Dynamo-style KV; no anti-entropy; batch-built read-only stores.
- [[apache-spark]] — *(stub)* dataflow engine; RDD lineage; declarative APIs.
- [[apache-kafka]] — log-based broker; partitions/offsets; log compaction; CDC transport.
- [[apache-flink]] — *(stub)* pipelined dataflow/stream engine; checkpointing.
- [[apache-samza]] — *(stub)* Kafka-coupled stream processor; materialized views.
- [[zookeeper]] — coordination service; partition→node routing metadata (consensus → Ch 9).

## Papers

- [[the-tail-at-scale]] — *(stub)* Dean & Barroso 2013; tail latency amplification.
- [[one-size-fits-all-paper]] — *(stub)* Stonebraker & Çetintemel 2005; specialized engines over one DB.
- [[codd-relational-model]] — *(stub)* Codd 1970; founding relational-model paper.
- [[what-goes-around-comes-around]] — *(stub)* Stonebraker & Hellerstein 2005; the debate rhymes.
- [[mapreduce-paper]] — *(stub)* Dean & Ghemawat 2004; expand at Ch 10.
- [[bigtable-paper]] — *(stub)* Chang et al. 2006; coined SSTable/memtable; column-family.
- [[lsm-tree-paper]] — *(stub)* O'Neil et al. 1996; named the LSM-tree.
- [[dynamo-paper]] — DeCandia et al. 2007; revived leaderless replication.
- [[lamport-clocks-paper]] — *(stub)* Lamport 1978; happens-before, logical clocks.
- [[critique-of-ansi-sql-isolation]] — *(stub)* Berenson et al. 1995; isolation defs are flawed.
- [[byzantine-generals-paper]] — *(stub)* Lamport et al. 1982; consensus with traitors.
- [[flp-impossibility]] — *(stub)* Fischer-Lynch-Paterson 1985; no deterministic async consensus.
- [[paxos-raft-paper]] — *(stub)* Paxos/Raft/VSR/Zab consensus algorithm family.
- [[chubby-paper]] — *(stub)* Burrows 2006; Google lock service; ZooKeeper's model.
- [[mapreduce-paper]] — Dean & Ghemawat 2004; the batch-processing foundation.
- [[gfs-paper]] — *(stub)* Ghemawat et al. 2003; GFS → HDFS.
- [[spark-rdd-paper]] — *(stub)* Zaharia et al. 2012; RDD lineage recomputation.
- [[pregel-paper]] — *(stub)* Malewicz et al. 2010; BSP graph processing.
- [[the-log-kreps]] — *(stub)* Kreps 2013; the log as data-integration abstraction.

## Comparisons

- [[relational-vs-document-model]] — the central Ch 2 head-to-head; depends on relationship structure.
- [[schema-on-read-vs-schema-on-write]] — implicit/dynamic vs. explicit/enforced; the migration example.
- [[declarative-vs-imperative-queries]] — why SQL/CSS beat imperative APIs; the optimizer & parallelism.
- [[btree-vs-lsm-tree]] — the central storage-engine head-to-head; reads vs. writes vs. space.
- [[oltp-vs-olap]] — transaction vs. analytic access patterns; row vs. column storage.
- [[rpc-vs-rest]] — why RPC location transparency is flawed; REST vs. SOAP; modern RPC.
- [[shared-memory-vs-shared-disk-vs-shared-nothing]] — the three distributed-data architectures.
- [[single-vs-multi-vs-leaderless-replication]] — the three replication approaches head-to-head.
- [[key-range-vs-hash-partitioning]] — sorted ranges vs. hashed; the Cassandra hybrid.
- [[local-vs-global-secondary-indexes]] — document vs. term partitioning of secondary indexes.
- [[isolation-levels]] — **master matrix**: which level prevents which race condition.
- [[serializability-implementations]] — serial execution vs. 2PL vs. SSI head-to-head.
- [[synchronous-vs-asynchronous-networks]] — circuit vs. packet switching; bounded vs. unbounded delay.
- [[monotonic-vs-time-of-day-clocks]] — durations vs. timestamps; which to use when.
- [[linearizability-vs-serializability]] — recency-of-one-object vs. transaction isolation.
- [[batch-vs-stream-vs-online]] — services/batch/stream by latency & input-boundedness.
- [[reduce-side-vs-map-side-joins]] — input assumptions, cost, output layout.
- [[hadoop-vs-mpp-databases]] — schema-on-read data lake vs. schema-on-write MPP.
- [[mapreduce-vs-dataflow-engines]] — materialization vs. pipelined recompute.
- [[log-based-vs-amqp-jms-brokers]] — replayable partitioned log vs. per-message queue.
- [[cdc-vs-event-sourcing]] — low-level DB changes vs. app-level intent events.
- [[federated-vs-unbundled-databases]] — unify reads vs. unify writes.
