---
title: "Ch 12 — The Future of Data Systems"
type: chapter
chapters: [12]
tags: [data-integration, unbundling, dataflow, correctness, ethics]
status: solid
updated: 2026-05-16
---

# Ch 12 — The Future of Data Systems

## One-paragraph thesis

The capstone: how things *should* be (Kleppmann's opinions, first person). No single
tool fits all uses, so applications **compose** several — the **data integration**
problem, best solved by **log-based derived data** (one system of record, others
derived asynchronously via CDC/event sourcing) rather than heterogeneous distributed
transactions. Reframed: this is **unbundling the database** into loosely-coupled
dataflow components (the "database inside-out"). Correctness in such systems comes
from the **end-to-end argument** (idempotence via operation IDs, async constraint
checking) — distinguishing **timeliness** ("eventual consistency") from **integrity**
("perpetual inconsistency"), enabling **coordination-avoiding** systems. Ends with
**ethics**: predictive analytics bias, surveillance, "data is the pollution of the
information age." *(DDIA Ch 12.)*

## Key ideas

- **[[data-integration]]** — derive specialized representations from a single
  ordered source; derived data vs. distributed transactions; the limits of total
  ordering & capturing causality.
- **[[lambda-architecture]]** — parallel batch + stream; its problems; unifying the
  two (replay + exactly-once + event-time windowing).
- **[[unbundling-databases]]** — index/materialized-view/replication-log as
  composable components; **federated** (unify reads) vs. **unbundled** (unify
  writes); what's missing (the "unbundled-DB shell").
- **[[dataflow-applications]]** — app code as a derivation function; separation of
  code & state; "subscribe, don't poll"; write path vs. read path; stateful
  offline clients; reads are events too.
- **[[end-to-end-argument]]** — exactly-once via end-to-end **operation IDs**; TCP/
  txn/stream dedup is insufficient alone; end-to-end checksums/encryption.
- **[[enforcing-constraints-in-dataflow]]** — uniqueness requires consensus; achieve
  it with log partitioning + a single-threaded processor; multi-partition without
  atomic commit.
- **[[timeliness-vs-integrity]]** — integrity ≫ timeliness; loosely interpreted
  constraints + compensating transactions → **coordination-avoiding data systems**.
- **[[auditing-and-integrity]]** — "trust, but verify"; designing for auditability;
  Merkle trees / certificate transparency.
- **[[data-ethics]]** — predictive analytics bias/feedback loops, accountability,
  surveillance, consent, "data as a toxic asset", legislation.

## Concepts introduced

- [[data-integration]] · [[lambda-architecture]] · [[unbundling-databases]] ·
  [[dataflow-applications]]
- [[end-to-end-argument]] · [[enforcing-constraints-in-dataflow]] ·
  [[timeliness-vs-integrity]] · [[auditing-and-integrity]] · [[data-ethics]]

## Comparisons introduced

- [[federated-vs-unbundled-databases]]

## Systems / papers referenced

- [[apache-kafka]]/[[apache-samza]] (log integration, "database inside-out"),
  [[postgresql]] (FDW federation), Storm distributed RPC, Elm/React-Redux,
  differential dataflow/Naiad, blockchains/Merkle trees, Jepsen
- [[the-log-kreps]] (Kreps "The Log" / unbundling), end-to-end argument (Saltzer
  et al. 1984)

## Trade-offs & tensions

- Log-based derived data (loose coupling, fault containment, easy reprocessing) vs.
  distributed transactions (linearizability/read-your-writes, but XA fault-tolerance
  & perf are poor, amplify failures).
- Unbundled breadth-of-workloads vs. integrated single-product depth/operational
  simplicity (don't unbundle if one tool suffices).
- Coordination reduces inconsistency apologies but increases outage apologies — find
  the sweet spot.
- Binary system models vs. probabilistic reality → audit, don't blindly trust.

## Connections to other chapters

- Synthesizes the whole book: storage engines (Ch 3), replication (Ch 5), CDC/event
  sourcing/log brokers (Ch 11), total order broadcast/consensus/linearizability
  (Ch 9), 2PC/XA (Ch 9), batch/stream (Ch 10–11), [[systems-of-record-and-derived-data]]
  (Part III). Twitter timeline write/read path ↔ [[load-parameters]] (Ch 1 — "full
  circle"). end-to-end ↔ [[fencing-tokens]]/[[stream-fault-tolerance]];
  Byzantine/audit ↔ [[byzantine-faults]].

## Open questions / things to revisit

- A better distributed-transaction protocol? Coordination-avoiding databases in
  production? Cryptographic auditing at scale? How the industry confronts the
  data-ethics "pollution" problem.
