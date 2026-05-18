---
title: The Unix Philosophy
type: concept
chapters: [10]
tags: [batch, unix, composability, design]
status: solid
updated: 2026-05-16
---

# The Unix Philosophy

## Definition

A set of design principles (Doug McIlroy, 1964/78) behind Unix pipes — and the
template for Hadoop/batch processing: *(DDIA Ch 10)*

1. Make each program **do one thing well**; build afresh rather than add features.
2. Expect every program's **output to become another's input**; avoid clutter,
   columnar/binary, interactive input.
3. Build to be **tried early**; throw away clumsy parts and rebuild.
4. Use **tools** over unskilled help, even if you must build & discard them.

(≈ Agile/DevOps, decades early.) `sort` exemplifies "one thing well" — spills to
disk, multi-threaded, beats most stdlib sorts — but is only powerful *composed* with
`uniq` etc. (the log-analysis pipeline; cf. sorting vs. in-memory aggregation, which
reuses [[sstables-and-lsm-trees]] mergesort).

## What enables composition

- **A uniform interface**: in Unix, a **file (descriptor)** = an ordered byte
  sequence — files, pipes, sockets, devices all share it; ASCII text with `\n`
  records by convention. Few non-Unix programs interoperate this well ("Balkanization
  of data"). (URLs/HTTP are the web's analogous uniform interface.)
- **Separation of logic and wiring**: programs use `stdin`/`stdout`; the shell wires
  inputs/outputs (loose coupling, late binding, inversion of control). Limits: can't
  easily do multiple inputs/outputs or pipe to a network connection.
- **Transparency & experimentation**: immutable inputs (rerun freely), inspect at
  any pipeline point, checkpoint to a file. Biggest limit: **single machine** →
  Hadoop.

## Related concepts

- [[mapreduce]] (the distributed embodiment) · [[batch-workflow-output]]
  (same philosophy: immutable in, replaceable out) · [[dataflow-engines]]
  (pipes vs. temp files) · [[batch-vs-stream-vs-online]]

## Sources

DDIA Ch 10 ("Batch Processing with Unix Tools", "The Unix Philosophy").
