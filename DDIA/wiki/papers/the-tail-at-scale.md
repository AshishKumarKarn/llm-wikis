---
title: "The Tail at Scale (Dean & Barroso, 2013)"
type: paper
chapters: [1]
tags: [paper, latency, performance, distributed]
status: stub
updated: 2026-05-16
---

# The Tail at Scale

Jeffrey Dean & Luiz André Barroso, *Communications of the ACM*, 56(2):74–80, Feb
2013. doi:10.1145/2408776.2408794

The source DDIA cites for **tail latency amplification**: when one user request
fans out into many parallel backend calls, the user request can only complete when
the *slowest* backend call returns, so even a small fraction of slow backend calls
makes a large fraction of user-facing requests slow. Motivates measuring and
optimizing high [[response-time-percentiles]] (p99, p999) rather than means.

> `status: stub` — captures the one idea DDIA Ch 1 uses it for. Expand if revisited.

## Related

- [[response-time-percentiles]] — the concept this paper grounds
- [[ch01-reliable-scalable-maintainable]]
