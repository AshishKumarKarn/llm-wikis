---
title: Message-Passing Dataflow
type: concept
chapters: [4]
tags: [messaging, message-broker, actors, dataflow]
status: developing
updated: 2026-05-16
---

# Message-Passing Dataflow

## Definition

Asynchronous message passing sits between RPC and databases: like RPC, a message is
delivered to another process with low latency; like a database, it goes via an
intermediary — a **message broker** (message queue / message-oriented middleware) —
that stores it temporarily. *(DDIA Ch 4)*

## Why a broker (vs. direct RPC)

Buffers if the recipient is down/overloaded (reliability); redelivers after a crash
(no lost messages); sender needn't know recipient's IP/port (good for ephemeral cloud
VMs); one message → several recipients; **decouples** sender from recipient.
Typically **one-way / asynchronous** — sender doesn't await a reply (a response, if
any, goes on a separate channel/reply queue).

## Message brokers

Historically commercial (TIBCO, IBM WebSphere); now RabbitMQ, ActiveMQ, NATS,
[[apache-kafka]] (compared in depth → [[ch11-stream-processing]]). Producer sends to
a named **queue/topic**; broker delivers to consumers/subscribers; many producers &
consumers per topic; a consumer can republish to another topic (chaining) or a reply
queue. Brokers don't enforce a data model — any encoding; if backward/forward
compatible, publishers/consumers evolve independently. Preserve unknown fields when
republishing.

## Distributed actor frameworks

Actor model: concurrency via actors (encapsulated state, async messages, one message
at a time — no threads/locks). **Distributed actor frameworks** use the same
message-passing across nodes (transparent encode/decode). Location transparency works
*better* than RPC here because the actor model already assumes messages can be lost.
Still need [[backward-forward-compatibility]] for rolling upgrades. Akka (Java
serialization by default — no compat; swap for Protobuf); Orleans (custom format, no
rolling upgrade by default); Erlang OTP (record schema changes surprisingly hard).

## Related concepts

- [[modes-of-dataflow]] · [[rpc-vs-rest]] · [[backward-forward-compatibility]]
- [[apache-kafka]] · [[ch11-stream-processing]] — full broker treatment

## Sources

DDIA Ch 4 ("Message-Passing Dataflow").
