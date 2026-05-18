---
title: Latency Percentiles (P90, P95, P99)
type: concept
tags: [performance, observability, latency]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Latency Percentiles (P90, P95, P99)

Statistical measures that describe the tail of the latency distribution — more meaningful
than averages for user-facing systems (see [[sources/system-design-study-guide]]).

## What they mean
- **P90:** 90% of requests complete within this time; 10% are slower.
- **P95:** 95% of requests complete within this time; 5% are slower.
- **P99:** 99% of requests complete within this time; 1% are slower.
- **P99.9 (P999):** the "tail" — 1 in 1000 requests is slower than this.

## Why averages lie
A system with avg=10ms may have P99=5000ms. Users hitting the tail experience the
service as broken even if most requests are fine. SLAs are almost always defined on
percentiles, not averages.

## In interviews
When diagnosing performance: measure P95/P99, not just mean. Tools: distributed
tracing (Jaeger, AWS X-Ray), metrics (Prometheus, CloudWatch). Kafka's
`records-lag-max` is similar — it's the worst-case consumer lag, not the average.

## Related
[[components/kafka]] · [[scenarios/multi-tenant-usage-dashboard]]
