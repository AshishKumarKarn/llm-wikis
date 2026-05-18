---
title: AWS Lambda
type: component
tags: [aws, lambda, serverless, event-driven]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# AWS Lambda

Serverless, event-driven compute — run code without managing servers, pay per invocation
× execution time × memory (see [[sources/system-design-study-guide]]).

## Key characteristics
**Stateless** (each invocation independent; state in DynamoDB/S3/Redis) · **short-lived**
(max 15 min) · **auto-scales horizontally** (each request = separate env; default ~1000
concurrent, increasable) · **cold start** (first invocation delay; mitigate with
provisioned concurrency or keep-warm pattern).

## Integration patterns
| Pattern | Example |
|---|---|
| Synchronous | API Gateway → Lambda |
| Asynchronous | S3 → Lambda |
| Stream processing | Kinesis / DynamoDB Streams → Lambda |
| Queue-based | SQS → Lambda |

## Lambda + Aurora → always add RDS Proxy
Lambda's stateless concurrency opens a new DB connection per invocation. At scale this
exhausts Aurora's connection limit. **RDS Proxy** pools connections and multiplexes,
handles graceful failover. See [[components/aws-aurora]].

## Lambda vs EC2 vs Containers
Lambda: event-driven, short-lived, auto-scale, per-exec billing, no infra management.
EC2: full control, persistent, per-instance billing, manual scaling. Containers: long-
running, more control than Lambda. See [[components/aws-ec2]].

## Limitations
Max 15 min · cold starts · debugging distributed systems harder · vendor lock-in ·
cost spikes on high-frequency triggers.

## Best practices
Single responsibility · DLQs for failed events · monitor with CloudWatch + X-Ray ·
Lambda Layers for shared code · Step Functions for orchestrating multiple Lambdas ·
EventBridge for routing.
