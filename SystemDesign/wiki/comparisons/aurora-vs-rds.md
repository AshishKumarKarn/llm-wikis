---
title: "Aurora vs RDS vs Aurora Serverless"
type: comparison
tags: [aws, aurora, rds, database]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Aurora vs RDS vs Aurora Serverless

"The one that trips up most SA candidates" (see [[sources/system-design-study-guide]]).

| Feature | RDS | Aurora | Aurora Serverless v2 |
|---|---|---|---|
| Storage | EBS (per instance) | Distributed, shared (auto 10GB→128TB) | Same as Aurora |
| Replication | Manual setup | Built-in, log-based (redo records) | Same as Aurora |
| Replica lag | Can be seconds | Sub-10ms | Same as Aurora |
| Failover time | Slower (minutes) | ~30 seconds | ~30 seconds |
| Performance | Standard MySQL/PG | Up to 5× MySQL, 3× PG | Same as Aurora |
| Cost | Lower | Higher | Pay per ACU, good for spiky loads |
| Scaling | Manual/read replicas | Up to 15 read replicas + auto-scale | Compute scales in <1s |
| Global DR | Cross-region read replica | Global Database (<1s lag, RPO ~1s) | Not for global DB |
| Connection pooling | Not needed at low scale | RDS Proxy for Lambda | **Always need RDS Proxy** |

## When to choose
✅ **Aurora:** high-throughput OLTP, HA required, read-heavy, growing/unpredictable
storage, global apps needing DR <1s RPO.
✅ **Aurora Serverless v2:** spiky/dev workloads, right-sizing is wasteful.
✅ **RDS:** tight budget, stable predictable workload, exact engine minor-version needed.

## The Lambda trap
Serverless v2 scales **compute, not connections**. Lambda fan-out at scale exhausts
Aurora's connection limit. Answer is always **RDS Proxy**
(see [[components/aws-aurora]], [[components/aws-lambda]]).
