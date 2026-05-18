---
title: Apache ZooKeeper
type: system
chapters: [6, 8, 9]
tags: [system, coordination, consensus, service-discovery]
status: solid
updated: 2026-05-16
---

# Apache ZooKeeper

A coordination service that holds small amounts of cluster metadata reliably. In
[[ch06-partitioning]] it keeps the **authoritative partition→node mapping** for
[[request-routing]]: nodes register, the routing tier / partition-aware clients
subscribe and are notified when a partition moves or a node is added/removed. Used
by HBase, SolrCloud, Kafka, and (via Helix) LinkedIn's Espresso.

Ch 8: its monotonic `zxid` (transaction ID) or node `cversion` serve as
**[[fencing-tokens]]** — guaranteed monotonically increasing, the property fencing
requires.

Ch 9: implements **[[total-order-broadcast]]** via the **Zab** consensus protocol;
modeled on Google **[[chubby-paper|Chubby]]**. Coordination feature set:
**linearizable atomic CAS** (locks/leases — the only part really needing
[[consensus]]), monotonic `zxid`/`cversion` ([[fencing-tokens]]), session/heartbeat
**failure detection** with **ephemeral nodes**, **change notifications**. Runs on
3–5 nodes serving many clients — "outsourced consensus". See
[[coordination-services]].

> `status: solid` — comprehensive across Ch 6/8/9.

## Related

- [[coordination-services]] · [[consensus]] · [[total-order-broadcast]] ·
  [[request-routing]] · [[fencing-tokens]] · [[chubby-paper]]
