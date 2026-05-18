# DDIA Wiki — Log

Append-only chronological record. One entry per operation.
Quick tail: `grep "^## \[" log.md | tail -5`

## [2026-05-16] meta | Wiki scaffolded
- Created schema `CLAUDE.md`, directory layout (`raw/`, `wiki/{chapters,concepts,systems,papers,comparisons}`), `index.md`, `log.md`, `overview.md`.
- Subject: *Designing Data-Intensive Applications* (Kleppmann). Sources: human's chapter notes in `raw/`. Priority: thoroughness — go-to reference for any query.
- Next: awaiting first chapter notes to ingest.

## [2026-05-16] ingest | Ch 1 Reliable, Scalable, and Maintainable Applications
- Source: raw/DDIA.md.md lines 628–1642 (whole-book file; ingesting chapter by chapter).
- Pages: chapters/ch01-reliable-scalable-maintainable (new); concepts/reliability, fault-tolerance, scalability, load-parameters, response-time-percentiles, shared-nothing-architecture, maintainability (all new); systems/apache-kafka, redis (new stubs); papers/the-tail-at-scale, one-size-fits-all-paper (new stubs). Index updated.
- Notable: forward-links seeded to Ch 4 (rolling upgrades/evolvability), Ch 6 (rebalancing), Ch 8 (partial failure deepens fault-tolerance), Ch 12 (Twitter hybrid revisited). [[apache-kafka]]/[[redis]] deliberately left as stubs for stream chapters.
- Follow-ups: expand kafka/redis at Ch 10–11; overview.md still stub (rebuilt in task #13).

## [2026-05-16] ingest | Ch 2 Data Models and Query Languages
- Source: raw/DDIA.md.md lines 1643–3547.
- Pages: chapters/ch02-data-models-and-query-languages (new); concepts/data-model-layering, relational-model, document-model, normalization-and-denormalization, object-relational-impedance-mismatch, data-locality, mapreduce-querying, graph-data-models (new); comparisons/relational-vs-document-model, schema-on-read-vs-schema-on-write, declarative-vs-imperative-queries (new — first comparison pages); systems/postgresql, mongodb, neo4j, google-spanner, cassandra (new stubs); papers/codd-relational-model, what-goes-around-comes-around, mapreduce-paper, bigtable-paper (new stubs). Index updated.
- Notable: forward-links seeded — mapreduce→Ch10, schema-on-read/write→Ch4, spanner→Ch8/9 (TrueTime/consensus), cassandra→Ch3/5/6 (LSM/leaderless/partitioning), bigtable→Ch3. Comparison-page pattern established.
- Follow-ups: expand spanner (Ch8/9), cassandra (Ch3/5/6), mapreduce-paper & bigtable-paper at their deep chapters.

## [2026-05-16] ingest | Ch 3 Storage and Retrieval
- Source: raw/DDIA.md.md lines 3548–5288.
- Pages: chapters/ch03-storage-and-retrieval (new); concepts/storage-engine-index-tradeoff, log-structured-storage, hash-index, sstables-and-lsm-trees, b-tree, write-amplification, secondary-indexes, multi-dimensional-indexes, full-text-and-fuzzy-indexes, in-memory-databases, data-warehousing, star-and-snowflake-schema, column-oriented-storage, materialized-views-and-data-cubes (new); comparisons/btree-vs-lsm-tree, oltp-vs-olap (new); systems/leveldb-rocksdb, lucene, vertica-cstore, riak-bitcask (new stubs); papers/lsm-tree-paper (new stub). Index updated.
- Notable: btree-vs-lsm-tree resolves the index.md stub placeholder. Forward-links: append-only log → Ch5 replication logs / Ch11 event log; B-tree range locks & copy-on-write → Ch7; column storage/SQL-on-Hadoop → Ch10; materialized views → Ch11 derived data.
- Follow-ups: deepen bigtable-paper (SSTable origin) — currently shared Ch2/3 stub; cassandra LSM detail noted, full expand at Ch5/6.

## [2026-05-16] ingest | Ch 4 Encoding and Evolution (+ Part II intro)
- Source: raw/DDIA.md.md lines 5289–6868 (incl. Part II "Distributed Data" intro).
- Pages: chapters/ch04-encoding-and-evolution (new); concepts/backward-forward-compatibility, data-encoding-formats, thrift-and-protocol-buffers, avro, schema-evolution, modes-of-dataflow, message-passing-dataflow, replication-vs-partitioning (new); comparisons/rpc-vs-rest, shared-memory-vs-shared-disk-vs-shared-nothing (new). Updated concepts/shared-nothing-architecture (backlinks). Index updated.
- Notable: Part II intro folded into Ch 4 page + 2 dedicated pages. Forward-links: RPC timeout→Ch8, idempotence→Ch11, brokers→Ch11/kafka, replication→Ch5, partitioning→Ch6. schema-on-read/write loop closed (Ch2↔Ch4).
- Follow-ups: kafka full treatment Ch11; deepen replication-vs-partitioning when Ch5/6 land.

## [2026-05-16] ingest | Ch 5 Replication
- Source: raw/DDIA.md.md lines 6870–8915.
- Pages: chapters/ch05-replication (new); concepts/single-leader-replication, multi-leader-replication, leaderless-replication, failover-and-split-brain, replication-log-implementations, change-data-capture (stub), replication-lag, eventual-consistency, read-after-write-consistency, monotonic-reads, consistent-prefix-reads, write-conflict-resolution, quorum-consistency, sloppy-quorum-and-hinted-handoff, happens-before-and-concurrency (new); comparisons/single-vs-multi-vs-leaderless-replication (new); papers/dynamo-paper (new), lamport-clocks-paper (new stub); systems/voldemort (new stub); updated systems/cassandra, riak-bitcask with leaderless detail. Index updated.
- Notable: closed the [[eventual-consistency]] link seeded from Ch1 fault-tolerance. Forward-links: failover/split brain & fencing→Ch9, clocks→Ch8, CDC→Ch11, linearizability vs quorums→Ch9, consistent-prefix→Ch6 partitioning.
- Follow-ups: expand change-data-capture & kafka at Ch11; lamport-clocks-paper at Ch8/9; deepen replication-vs-partitioning at Ch6.

## [2026-05-16] ingest | Ch 6 Partitioning
- Source: raw/DDIA.md.md lines 8916–9802.
- Pages: chapters/ch06-partitioning (new); concepts/partitioning-basics, key-range-partitioning, hash-partitioning, skewed-workloads-and-hot-spots, rebalancing-partitions, request-routing, parallel-query-execution (stub) (new); comparisons/key-range-vs-hash-partitioning, local-vs-global-secondary-indexes (new); systems/zookeeper (new, developing); updated systems/cassandra (Ch6 detail + frontmatter chapters/status). Index updated.
- Notable: parallel-query-execution stub → Ch10; zookeeper consensus role → Ch9; global secondary index consistency → Ch7/9; cascading-failure link to failover-and-split-brain. Cassandra now a multi-chapter developing page.
- Follow-ups: expand zookeeper & parallel-query-execution at Ch9/Ch10.

## [2026-05-16] ingest | Ch 7 Transactions
- Source: raw/DDIA.md.md lines 9803–12089.
- Pages: chapters/ch07-transactions (new); concepts/acid, single-vs-multi-object-transactions, transaction-aborts-and-retries, read-committed, snapshot-isolation, multi-version-concurrency-control, lost-updates, write-skew-and-phantoms, serializability, actual-serial-execution, two-phase-locking, serializable-snapshot-isolation (new); comparisons/isolation-levels (master anomaly×level matrix — key query reference), serializability-implementations (new); papers/critique-of-ansi-sql-isolation (new stub); updated systems/postgresql (MVCC/SSI + frontmatter). Index updated.
- Notable: isolation-levels is the highest-value reference table for queries. Forward-links: distributed txns/2PC/2PL-vs-2PC → Ch9; ACID-C vs CAP-linearizability → Ch9; replicated lost updates/LWW ↔ Ch5 happens-before/write-conflict-resolution; copy-on-write MVCC ↔ Ch3 b-tree.
- Follow-ups: 2PC & distributed serializability at Ch9; idempotence at Ch11.

## [2026-05-16] ingest | Ch 8 The Trouble with Distributed Systems
- Source: raw/DDIA.md.md lines 12090–14248.
- Pages: chapters/ch08-the-trouble-with-distributed-systems (new); concepts/partial-failure, unreliable-networks, unreliable-clocks, process-pauses, truth-by-majority, fencing-tokens, byzantine-faults, system-models (new); comparisons/synchronous-vs-asynchronous-networks, monotonic-vs-time-of-day-clocks (new); papers/byzantine-generals-paper (new stub); updated systems/google-spanner (TrueTime + frontmatter), zookeeper (zxid fencing token). Index updated.
- Notable: LWW clock danger ↔ Ch5 happens-before/write-conflict-resolution closed; lease bug ↔ Ch7 SSI "outdated premise"; quorum/fencing/system-models set up Ch9. Spanner & zookeeper now multi-chapter developing pages.
- Follow-ups: consensus/2PC/total-order-broadcast/linearizability at Ch9; expand zookeeper & spanner there; lamport logical clocks at Ch9.

## [2026-05-17] ingest | Ch 9 Consistency and Consensus (+ Part III intro)
- Source: raw/DDIA.md.md lines 14249–17247 (incl. Part III "Derived Data" intro). Largest chapter; read in 4 passes.
- Pages: chapters/ch09-consistency-and-consensus (new); concepts/linearizability, cap-theorem, causal-consistency, lamport-timestamps, total-order-broadcast, two-phase-commit, distributed-transactions-xa, consensus, coordination-services, systems-of-record-and-derived-data (new); comparisons/linearizability-vs-serializability (new); papers/flp-impossibility, paxos-raft-paper, chubby-paper (new stubs); updated systems/zookeeper (Zab/Chubby, status→solid, chapters [6,8,9]). Index updated.
- Notable: closed all Ch5/7/8 forward-links into Ch9. Key cross-cut: linearizable CAS ≡ total order broadcast ≡ consensus. systems-of-record-and-derived-data frames Part III. Session paused by user (token quota) at end of Ch9; resumed 2026-05-17.
- Follow-ups: Part III practical chapters Ch10–12; overview.md rebuild (task #13).

## [2026-05-17] ingest | Ch 10 Batch Processing
- Source: raw/DDIA.md.md lines 17248–19543.
- Pages: chapters/ch10-batch-processing (new); concepts/unix-philosophy, mapreduce, distributed-filesystem-hdfs, batch-joins, batch-workflow-output, dataflow-engines, pregel-graph-processing (new); comparisons/batch-vs-stream-vs-online, reduce-side-vs-map-side-joins, hadoop-vs-mpp-databases, mapreduce-vs-dataflow-engines (new); papers/gfs-paper, spark-rdd-paper, pregel-paper (new stubs); systems/apache-spark (new stub); expanded papers/mapreduce-paper (stub→developing, Ch10 deep dive) & concepts/parallel-query-execution (Ch6 stub→developing, Ch10 join algos). Index updated.
- Notable: closed Ch6 parallel-query-execution forward-link; mapreduce-querying (Ch2) ↔ mapreduce (Ch10); systems-of-record-and-derived-data (Ch9 Part III intro) anchors batch-workflow-output. Pregel ↔ actor model (Ch4). Bounded→unbounded sets up Ch11.
- Follow-ups: Ch11 stream (Kafka/CDC/exactly-once); Ch12; lucene/voldemort already cross-linked from batch output.

## [2026-05-17] ingest | Ch 11 Stream Processing
- Source: raw/DDIA.md.md lines 19544–21843.
- Pages: chapters/ch11-stream-processing (new); concepts/event-streams-and-messaging, log-based-message-brokers, event-sourcing, state-streams-immutability, stream-processing-uses, reasoning-about-time-in-streams, stream-joins, stream-fault-tolerance (new); expanded concepts/change-data-capture (Ch5 stub→solid); comparisons/log-based-vs-amqp-jms-brokers, cdc-vs-event-sourcing (new); expanded systems/apache-kafka (Ch1 stub→solid); systems/apache-flink, apache-samza (new stubs); papers/the-log-kreps (new stub). Index updated (removed duplicate kafka stub line).
- Notable: closed the Ch5 change-data-capture forward-link (now the hub: replication-log ↔ log brokers ↔ event sourcing ↔ derived data). dual-writes race ↔ Ch5/9; exactly-once ↔ Ch9 distributed-transactions-xa; fencing ↔ Ch8; Twitter timeline ↔ Ch1 load-parameters.
- Follow-ups: Ch12 (unbundling DB, lambda, end-to-end argument, correctness, ethics); overview.md rebuild + lint (task #13).

## [2026-05-17] ingest | Ch 12 The Future of Data Systems
- Source: raw/DDIA.md.md lines 21844–24954 (largest chapter; read in 4 passes; refs skimmed).
- Pages: chapters/ch12-the-future-of-data-systems (new); concepts/data-integration, lambda-architecture, unbundling-databases, dataflow-applications, end-to-end-argument, enforcing-constraints-in-dataflow, timeliness-vs-integrity, auditing-and-integrity, data-ethics (new); comparisons/federated-vs-unbundled-databases (new). Index updated (status→complete, overview promoted).
- Notable: capstone — synthesizes whole book. Closed Ch1 "full circle" (Twitter timeline write/read path ↔ load-parameters). end-to-end ↔ Ch11 stream-fault-tolerance / Ch8 fencing; constraints ↔ Ch9 consensus/TOB; auditing ↔ Ch8 byzantine/system-models. **All 12 chapters now ingested.**
- Follow-ups: task #13 — rebuild overview.md as the book-wide synthesis; lint pass (orphans, broken-looking wikilinks, stale stubs).

## [2026-05-17] lint+meta | overview.md rebuilt; whole-book ingest complete
- Rebuilt overview.md (stub→solid): central question, per-part chapter map, 6 cross-cutting tensions (the append-only log as the unifying idea; consistency vs. availability; coordinate vs. avoid; read/write-path; immutability; no one-size-fits-all; history rhymes), "how to use this wiki". index.md status→complete, updated 2026-05-17, overview promoted.
- Lint results: 184 pages (12 chapters, 113 concepts, 22 comparisons, 16 systems, 18 papers, + index/log/overview). **0 broken wikilinks** (all `[[slug]]`/`[[slug\|alias]]` resolve). **0 orphan pages** (every chapter/concept/comparison/system/paper has ≥1 inbound link). 27 pages remain `status: stub` — all papers (16, intentionally one-idea anchors per CLAUDE.md) and briefly-mentioned systems (11, e.g. neo4j/redis/voldemort/flink); concepts & comparisons carry the depth — no action needed.
- Whole-book ingest of *Designing Data-Intensive Applications* (raw/DDIA.md.md, all 12 chapters + Part II/III intros) is **complete**. Wiki ready as the go-to query reference.

## [2026-05-17] query | "what is batch processing?"
- Read: comparisons/batch-vs-stream-vs-online; answered from ch10-batch-processing + unix-philosophy/mapreduce/dataflow-engines/batch-workflow-output.
- Definitional query fully covered by existing pages — no new page filed.

## [2026-05-17] query | "what is MapReduce and how is it related to Hadoop?"
- Read: concepts/mapreduce, concepts/distributed-filesystem-hdfs; synthesized the Google-papers→Hadoop (GFS→HDFS, MapReduce→Hadoop MapReduce) mapping and the Unix-analogy layering.
- Definitional/relational query fully covered by existing pages (mapreduce, distributed-filesystem-hdfs, hadoop-vs-mpp-databases, ch10) — no new page filed.
