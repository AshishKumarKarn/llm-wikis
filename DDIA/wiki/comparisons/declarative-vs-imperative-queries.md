---
title: "Declarative vs. Imperative Query Languages"
type: comparison
chapters: [2]
tags: [query-languages, sql, declarative, comparison]
status: solid
updated: 2026-05-16
---

# Declarative vs. Imperative Query Languages

## The distinction

- **Imperative** — tell the computer *which operations in what order* (loop, mutate
  vars, branch). IMS/CODASYL used imperative query APIs (COBOL iterating records).
- **Declarative** — specify *the pattern of the result* (conditions, sort, group,
  aggregate), not how to obtain it. SQL, relational algebra (σ selection), CSS/XPath.
*(DDIA Ch 2)*

## Why declarative wins (for databases)

1. **Concise & easier** than an imperative API.
2. **Hides engine internals** → the DB can change implementation (storage layout,
   reclaiming disk space by moving records) without breaking queries. SQL guarantees
   no ordering unless asked, giving the optimizer freedom; imperative code might rely
   on incidental order, so the DB can't optimize safely.
3. **Parallelizable** — CPUs scale by adding cores, not clock speed ("the free lunch
   is over"). Imperative code with mandated instruction order is hard to parallelize;
   declarative specifies only the result pattern, so the DB can run it in parallel.

## The web analogy (the memorable argument)

CSS/XSL (declarative) vs. hand-manipulating styles via the DOM API (imperative). The
imperative JS version is longer, harder, and **buggy**: it won't un-set the blue
background when `selected` is removed (CSS does automatically), and adopting a faster
new API forces a rewrite (browser vendors can speed up CSS/XPath without breaking
compatibility). Same lesson as databases.

## Where MapReduce sits

Between the two — see [[mapreduce-querying]]. The moral: a NoSQL system may
*accidentally reinvent SQL* (MongoDB's declarative aggregation pipeline).

## Related

- [[relational-model]] — the optimizer is what makes SQL's declarativeness pay off
- [[mapreduce-querying]] · [[graph-data-models]] (Cypher/SPARQL/Datalog are declarative)
- [[ch02-data-models-and-query-languages]]

## Sources

DDIA Ch 2 ("Query Languages for Data", "Declarative Queries on the Web"). Ref:
Sutter, "The Free Lunch Is Over" (2005).
