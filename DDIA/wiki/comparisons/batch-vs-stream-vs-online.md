---
title: "Services vs. Batch vs. Stream Processing"
type: comparison
chapters: [10]
tags: [batch, stream, online, comparison]
status: solid
updated: 2026-05-16
---

# Services vs. Batch vs. Stream Processing

The three types of data-processing systems. *(DDIA Ch 10)*

| | **Services** (online) | **Batch** (offline) | **Stream** (near-real-time) |
|---|---|---|---|
| Trigger | client request; user waits | scheduled (e.g. daily) | event, shortly after it happens |
| Input | per request | **bounded** (known fixed size) | **unbounded** (never-ending) |
| Primary metric | response time | throughput | latency (lower than batch) |
| Completes? | per request | yes — job ends when input consumed | never (always more coming) |
| Examples | web server, REST API, DB | MapReduce, Spark report | Kafka/Flink pipelines (Ch 11) |

## The key axis

**Boundedness of input.** A batch job knows when it has read the *entire* input, so
it eventually completes and its **output is derived from a fixed input** (rerunnable,
deterministic). A stream job's input never ends, so it's never "done". Stream
processing builds on batch ideas — "another way of speeding up batch processing" — so
it's covered next ([[ch11-stream-processing]]).

## Related

- [[mapreduce]] · [[unix-philosophy]] · [[systems-of-record-and-derived-data]] ·
  [[response-time-percentiles]] · [[ch11-stream-processing]]

## Sources

DDIA Ch 10 (intro, Summary).
