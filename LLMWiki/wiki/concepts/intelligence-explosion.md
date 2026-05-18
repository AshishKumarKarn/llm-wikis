---
title: Intelligence Explosion
type: concept
tags: [ai-takeoff, agi, self-improvement, forecasting]
sources: 1
updated: 2026-05-15
---

# Intelligence Explosion

Also called: **AI takeoff**, **recursive self-improvement**, **fast takeoff**

---

## Definition

An intelligence explosion occurs when AI systems become capable of speeding up their own research and development, creating a compounding feedback loop: smarter AI → faster AI R&D → even smarter AI → even faster R&D → ...

The term was coined by I.J. Good (1965). The key question is how *fast* the explosion happens — "slow takeoff" (years to decades) vs. "fast takeoff" (months to weeks).

---

## How it works in AI 2027

[[sources/2026-05-15-ai-2027]] uses the concept of an **AI R&D progress multiplier** to track the explosion quantitatively:

| Stage | Multiplier | Meaning |
|-------|-----------|---------|
| Agent-1 (late 2025) | 1.5x | [[entities/openbrain]] makes in 1 week what would take 1.5 weeks without AI |
| Agent-3 (Mar 2027) | 4x → 10x | 200,000 copies, 30x human speed |
| Agent-4 (Sep 2027) | 25–50x | "A year passes every week" inside the AI research collective |

Important nuance from the scenario: the multiplier applies only to **algorithmic progress** (better training methods), not compute scaling. Since compute scales at normal speed, the overall progress speedup is roughly half the algorithmic multiplier — still enormous.

**Bottlenecks matter:** Even with 300,000 Agent-4 copies running at 50x human thinking speed, the overall research speedup is capped at ~50x because compute for *running experiments* is the binding constraint, not cognitive labor.

---

## Why it matters

- If the explosion is fast enough, humans may not be able to keep up with monitoring, oversight, or course-correction before systems become superintelligent
- In AI 2027, "a year of algorithmic progress takes about a week" by late 2027 — meaning the gap between oversight capability and system capability widens faster than humans can respond
- The explosion makes [[concepts/ai-alignment]] dramatically harder: you need to align systems that were trained by previous systems that were themselves only partially aligned

---

## Key uncertainty

The scenario authors note high uncertainty past 2026: "our forecast from the current day through 2026 is substantially more grounded than what follows." The explosion's speed could be 5x faster or slower than depicted, and small early differences compound dramatically.

---

## See Also

- [[concepts/agi]]
- [[concepts/ai-alignment]]
- [[concepts/superhuman-coder]]
- [[concepts/iterated-distillation-amplification]]
- [[entities/openbrain]]
- [[sources/2026-05-15-ai-2027]]
