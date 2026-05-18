---
title: "The Google File System (Ghemawat et al., 2003)"
type: paper
chapters: [10]
tags: [paper, batch, distributed-filesystem]
status: stub
updated: 2026-05-16
---

# The Google File System

Sanjay Ghemawat, Howard Gobioff, Shun-Tak Leung, 19th ACM SOSP, October 2003.

The distributed filesystem GFS — reimplemented open-source as **HDFS** (see
[[distributed-filesystem-hdfs]]). Shared-nothing, commodity hardware, replicated
blocks tracked by a central master/NameNode; the storage substrate
[[mapreduce|MapReduce]] reads/writes. *(DDIA Ch 10)*

## Related

- [[distributed-filesystem-hdfs]] · [[mapreduce]] · [[mapreduce-paper]]
