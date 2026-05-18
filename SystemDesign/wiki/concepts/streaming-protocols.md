---
title: Streaming Protocols
type: concept
tags: [streaming, video, webrtc, protocols]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# Streaming Protocols

How media is transported from producer to consumer (see [[sources/system-design-study-guide]]).

## Protocol comparison
| Protocol | Transport | Latency | Direction | Best for |
|---|---|---|---|---|
| **HLS** | HTTP (chunks + M3U8 manifest) | Seconds | Server→client | VOD, broad device compat (Netflix) |
| **DASH** | HTTP (chunks + MPD manifest) | Seconds | Server→client | Multi-codec, diverse devices |
| **RTMP** | TCP (persistent connection) | ~1–2s | Client→server | Ingest from creator to server (Twitch, YouTube Live) |
| **RTSP** | TCP/UDP (session control) | Low | Both | CCTV, IP cameras |
| **SRT** | UDP + ARQ + FEC + AES | Low | Both | Remote production over public internet |
| **WebRTC** | UDP (P2P, STUN/TURN, DTLS/SRTP) | Near-zero | P2P | Video conferencing (Google Meet, Zoom) |
| **WHIP/WHEP** | HTTP signaling over WebRTC | Near-zero | Both | Large-scale WebRTC ingest/egress |

## Key distinctions
- HLS/DASH are **adaptive bitrate (ABR)** — auto-switch quality on network change.
- RTMP is the standard **ingest protocol** from encoders to streaming servers; HLS/DASH
  for delivery to viewers.
- WebRTC is **peer-to-peer** (server-less path when possible); STUN handles NAT
  traversal, TURN relays when direct P2P fails; signaling server needed for discovery.
- SRT is the go-to for **reliable low-latency over lossy/public internet**.

## Related
[[scenarios/follow-me-voice-routing]] (WebRTC / SIP concepts)
