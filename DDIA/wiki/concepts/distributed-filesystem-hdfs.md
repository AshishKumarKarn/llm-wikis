---
title: Distributed Filesystem (HDFS)
type: concept
chapters: [10]
tags: [batch, hdfs, storage, distributed]
status: developing
updated: 2026-05-16
---

# Distributed Filesystem (HDFS)

## Definition

MapReduce's "uniform interface" (the role files/pipes play in Unix). HDFS = open-
source reimplementation of the Google File System (GFS). Similar: GlusterFS, QFS;
object stores (S3, Azure Blob, Swift) are comparable (but usually separate storage &
compute, losing locality). *(DDIA Ch 10)*

## How it works

**Shared-nothing** (vs. shared-disk NAS/SAN — no special hardware, just commodity
machines on a conventional network). A daemon per machine exposes its local disks; a
central **NameNode** tracks which blocks are on which machine → one big logical
filesystem over all disks. Scales to tens of thousands of machines, hundreds of PB,
far cheaper than dedicated appliances.

- **Fault tolerance**: blocks replicated to multiple machines, or **erasure coding**
  (Reed–Solomon — lower storage overhead but loses read locality, like RAID over a
  network).
- Enables "dump data indiscriminately, decide how to process later" — the data
  lake / **schema-on-read** model (see [[hadoop-vs-mpp-databases]]).

## Related concepts

- [[mapreduce]] · [[shared-nothing-architecture]] · [[hadoop-vs-mpp-databases]] ·
  [[gfs-paper]] · [[unix-philosophy]]

## Sources

DDIA Ch 10 ("MapReduce and Distributed Filesystems").
