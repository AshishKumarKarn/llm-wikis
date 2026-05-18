---
title: "A Critique of ANSI SQL Isolation Levels (Berenson et al., 1995)"
type: paper
chapters: [7]
tags: [paper, transactions, isolation]
status: stub
updated: 2026-05-16
---

# A Critique of ANSI SQL Isolation Levels

Hal Berenson, Philip A. Bernstein, Jim N. Gray, et al., ACM SIGMOD, May 1995.

DDIA Ch 7's source for the argument that the SQL standard's isolation-level
definitions are **ambiguous, imprecise, and not implementation-independent** — the
root of the "[[snapshot-isolation|repeatable read]] means different things
everywhere" mess. Defines dirty write, read skew, write skew, and snapshot isolation
precisely.

> `status: stub` — anchor for [[isolation-levels]].

## Related

- [[isolation-levels]] · [[snapshot-isolation]] · [[write-skew-and-phantoms]]
