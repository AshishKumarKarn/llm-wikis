---
title: Skewed Workloads & Relieving Hot Spots
type: concept
chapters: [6]
tags: [partitioning, hot-spot, skew]
status: developing
updated: 2026-05-16
---

# Skewed Workloads & Relieving Hot Spots

## The problem

[[hash-partitioning|Hashing]] reduces hot spots but can't eliminate them: if *all*
reads/writes target **one key**, every request still hits one partition (hash of
identical IDs is identical). Real example: a celebrity with millions of followers
causes a write storm on one key. *(DDIA Ch 6)*

## The (manual) mitigation

Most data systems can't auto-compensate — it's the application's job. Add a random
suffix/prefix to a known hot key (e.g. a 2-digit number → 100 sub-keys spread across
partitions). Costs:

- Reads must query **all** split sub-keys and combine.
- Only worth it for the few hot keys → extra bookkeeping to track which keys are
  split.

Future systems may auto-detect/compensate; for now, reason about the trade-off
per application.

## Related concepts

- [[hash-partitioning]] · [[partitioning-basics]] · [[load-parameters]]
  (the Ch 1 Twitter celebrity-fan-out parallel)

## Sources

DDIA Ch 6 ("Skewed Workloads and Relieving Hot Spots").
