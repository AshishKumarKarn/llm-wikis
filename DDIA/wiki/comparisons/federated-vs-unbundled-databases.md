---
title: "Federated vs. Unbundled Databases"
type: comparison
chapters: [12]
tags: [data-integration, unbundling, comparison]
status: solid
updated: 2026-05-16
---

# Federated vs. Unbundled Databases

Two ways to compose diverse storage/processing tools into a cohesive system — "two
sides of the same coin". *(DDIA Ch 12)*

| | Federated (polystore) | Unbundled |
|---|---|---|
| Unifies | **reads** | **writes** |
| Mechanism | one query interface over many engines | CDC + event logs synchronizing writes |
| Tradition | relational (high-level query, complex impl) | Unix (small tools, uniform low-level API, composed by a language) |
| Example | PostgreSQL foreign data wrappers | `mysql \| elasticsearch`-style log integration |
| Hard part | mapping one data model to another (manageable) | keeping writes in sync across systems (**the harder problem**) |

## Takeaway

Federated read-only querying maps models — manageable. **Keeping writes in sync is
the harder engineering problem**; the right tool is an asynchronous event log with
idempotent consumers, *not* heterogeneous distributed transactions
([[distributed-transactions-xa]]) — because log-based integration gives **loose
coupling** (fault containment + independent teams). Both compose a reliable, scalable,
maintainable system from diverse parts; neither replaces single databases.

## Related

- [[unbundling-databases]] · [[data-integration]] · [[change-data-capture]] ·
  [[dataflow-applications]]

## Sources

DDIA Ch 12 ("Making unbundling work").
