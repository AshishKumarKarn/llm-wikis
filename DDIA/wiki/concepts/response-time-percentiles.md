---
title: Response Time Percentiles & Tail Latency
type: concept
chapters: [1]
tags: [performance, scalability, latency, slo]
status: solid
updated: 2026-05-16
---

# Response Time Percentiles & Tail Latency

## Definition

Response time is what the **client** sees: service time + network + queueing delays.
(**Latency** ≠ response time: latency is the duration a request is *latent*, awaiting
service.) Because every identical request varies, response time must be treated as a
**distribution**, summarized by **percentiles** (median/p50, p95, p99, p999), not a
mean. *(DDIA Ch 1)*

## Why it matters

- The **mean is a poor "typical" metric** — it doesn't tell you how many users
  experienced a delay.
- The **median (p50)** is the true "typical" wait: half of requests are faster.
- **High percentiles = tail latencies** directly shape the experience of the most
  valuable users. Amazon: the slowest requests often belong to customers with the
  most data (most purchases). A 100 ms response increase → 1% sales drop; a 1 s
  slowdown → 16% satisfaction drop. Amazon targets **p99.9** but judged **p99.99**
  too costly for the benefit (diminishing returns; high percentiles are dominated by
  random events outside your control).

## How it works (the mechanism)

- **SLOs / SLAs** are defined in percentiles (e.g. "up" = p50 < 200 ms and
  p99 < 1 s, available 99.9% of the time).
- **Queueing delay** dominates high percentiles: a server processes few things in
  parallel, so a few slow requests cause **head-of-line blocking**. Measure response
  time **client-side**, and load generators must send independently of responses
  (waiting artificially shortens queues and skews results).
- **Tail latency amplification**: when one user request fans out to many backend
  calls, the request waits for the *slowest* call; even a small fraction of slow
  backend calls makes a large fraction of user requests slow. See [[the-tail-at-scale]].
- **Computing percentiles efficiently**: rolling window; approximation algorithms
  (forward decay, t-digest, HdrHistogram). **Averaging percentiles is mathematically
  meaningless** — aggregate by adding histograms.

## Trade-offs

- Optimizing ever-higher percentiles has sharply diminishing returns vs. cost.
- Exact (sort the window) vs. approximate (t-digest/HdrHistogram) — CPU/memory vs.
  precision.

## Related concepts

- [[scalability]] — performance is half of the scalability question
- [[load-parameters]] — the matching load-side description
- [[the-tail-at-scale]] — the amplification paper

## Sources

DDIA Ch 1 ("Describing Performance"). [[the-tail-at-scale]] (Dean & Barroso, CACM
2013).
