---
title: "Design: Follow-Me Voice Routing"
type: scenario
tags: [telephony, voip, redis, concurrency, scenarios]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Design: Follow-Me Voice Routing

**Prompt (Vonage):** Route an incoming call through a sequence (Office → Mobile → Home)
until someone picks up. Every ms of routing delay = "silent gap" for the caller
(see [[sources/system-design-study-guide]], p142).

## Key design decisions

### State management — externalize immediately
Call state written to Redis atomically before touching the first carrier:
`SET call:{id} {json state} EX 90` (TTL > max routing window).
If primary router crashes, a standby reads Redis, calculates elapsed time, bridges or
advances. **Redis `WATCH/MULTI/EXEC` optimistic lock** ensures exactly-one standby wins
the race. See [[components/redis]].

### Concurrency — Virtual Threads (Project Loom) over WebFlux
Each call = a **sequential state machine** (dial leg 1, wait, advance). Virtual threads
allow 50K+ concurrent waiting call threads at near-zero cost — they park in heap, not OS
threads. Blocking code (`carrier.dial().get()`) reads naturally, debugs easily.

WebFlux/Reactive forces a Mono chain with `timeout()` + `flatMap` cascades for inherently
sequential state — harder to reason about for telephony, painful stack traces.
**Rule:** use reactive for stateless fan-out; virtual threads for sequential state with
waiting.

### No-answer handling — dual-clock enforcement
Don't trust the carrier to respect your timeout. Own timer fires → send SIP CANCEL → next
leg's INVITE immediately. Eliminates carrier's contribution to gap latency.

**Silent pre-dial:** dial next leg 1–2s before timeout expires, hold provisional, connect
instantly on timeout → perceived gap = 0ms. Uses two carrier legs briefly.
SIP 183 (Session Progress) = human might still answer; no 183 within 3–4s → start
pre-dial.

## Gap budget in practice
Timer fires (0ms) → CANCEL sent → carrier ACK (50–150ms) → next INVITE → carrier
processes (50–200ms) → ringback (50ms) = **150–400ms** silence without pre-dial; ~0ms with.

## Related
[[components/redis]] · [[concepts/streaming-protocols]] (WebRTC/SIP)
