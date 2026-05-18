# Interview Prep Wiki — Schema & Operating Manual

This vault is an **LLM-maintained interview-preparation knowledge base**. Its core is
system design and distributed systems; it also covers applied scenario Q&A and
behavioral/leadership interview material. You (the LLM agent) own and maintain the `wiki/`
layer. The human curates sources, asks questions, and directs analysis. This file is the
contract. Re-read it at the start of every session and follow it exactly.

## Three layers

1. **`raw/`** — immutable source documents (articles, papers, talk notes, book chapters,
   pasted text, images in `raw/assets/`). You READ these. You NEVER edit or delete them.
   This is the source of truth.
2. **`wiki/`** — your output. Markdown pages you create, update, cross-link, and keep
   consistent. The human reads this; you write all of it.
3. **This schema (`CLAUDE.md`)** — conventions and workflows. Co-evolved with the human.
   Propose changes here when you discover a better workflow; don't change it silently.

## Directory conventions

```
raw/                  immutable sources
  assets/             images referenced by sources
wiki/
  overview.md         top-level synthesis + evolving thesis; the entry point
  sources/            one page per ingested source (the summary + takeaways)
  concepts/           fundamental ideas: CAP, consistency models, latency/throughput,
                      idempotency, backpressure, fault tolerance
  components/         building blocks: load balancer, cache, message queue, CDN,
                      database, API gateway, rate limiter, object store
  patterns/           architectural patterns: sharding, replication, CQRS, event
                      sourcing, circuit breaker, saga, leader election, write-ahead log
  systems/            full case studies: "Design Twitter", "Design a URL shortener",
                      "Design Netflix", "Design a rate limiter"
  scenarios/          applied Q&A and "how would you handle X" prompts: optimize an
                      API endpoint, debug high latency, handle a traffic spike
  behavioral/         leadership / behavioral interview material: STAR stories,
                      competency notes (ownership, conflict, prioritization, influence)
  comparisons/        explicit trade-off pages: SQL vs NoSQL, push vs pull, sync vs
                      async replication, REST vs gRPC
index.md              content catalog — every page, one line each, by category
log.md                append-only chronological record of all operations
```

### File naming
- Kebab-case, descriptive, no dates in filenames: `consistent-hashing.md`, `cap-theorem.md`.
- Source pages: `sources/<slug-of-title>.md`.
- One concept/component/pattern per page. Split when a page exceeds ~400 lines.

### Page frontmatter (required on every wiki page)
```yaml
---
title: Consistent Hashing
type: concept            # source | concept | component | pattern | system | scenario | behavioral | comparison | overview
tags: [hashing, sharding, scalability]
sources: [consistent-hashing-dynamo-paper]   # slugs of sources/ pages backing this
created: 2026-05-15
updated: 2026-05-15
---
```
Frontmatter powers Obsidian Dataview. Keep `sources` accurate — it is the citation trail.

### Linking rules
- Link liberally with `[[wiki-link]]` syntax (Obsidian-native), using the filename
  without extension: `[[consistent-hashing]]`, `[[cap-theorem]]`.
- A link to a page that doesn't exist yet is fine — it's a TODO marker (red in graph view).
- Every claim derived from a source must cite it: `(see [[sources/dynamo-paper]])`.
- Every new page must get at least one inbound link from an existing page, or it's an orphan.

## Operations

### INGEST  — trigger: "ingest <file>" / "process this source" / a new file in `raw/`
1. Read the raw source fully. If it references images in `raw/assets/`, view them too.
2. Discuss 3–6 key takeaways with the human **before** writing. Wait for direction unless
   told to ingest unattended.
3. Create `wiki/sources/<slug>.md`: frontmatter + bibliographic line (title, author,
   origin, date, link if any) + structured summary + key takeaways + open questions.
4. Propagate: create or update every relevant `concepts/`, `components/`, `patterns/`,
   `systems/`, `scenarios/`, `behavioral/`, `comparisons/` page. A meaty source
   typically touches 8–15 pages.
   - When the source contradicts an existing page, do NOT silently overwrite. Add a
     `> ⚠️ Contradiction:` callout citing both sources and flag it in the log.
   - Strengthen existing claims by appending the new source to their `sources:` list.
5. Update `index.md` (add/adjust catalog lines) and `overview.md` (revise thesis if the
   source shifts the big picture).
6. Append a `log.md` entry.
7. Report to the human: what pages were created/updated, contradictions found, suggested
   next sources or questions.

### QUERY — trigger: any question about the domain
1. Read `index.md` first to locate relevant pages; then read those pages (and their
   `sources:` if precision matters).
2. Answer with citations to wiki pages and underlying sources. State confidence and gaps.
3. If the answer is durable/reusable (a comparison, synthesis, derivation, decision),
   **offer to file it back** as a new `comparisons/` or `concepts/` page so the
   exploration compounds. File it on confirmation, then update index + log.
4. Output format follows the question: prose, a comparison table, a Marp deck, a
   matplotlib chart. Default to a markdown wiki page.

### LINT — trigger: "lint" / "health check the wiki"
Scan and report (don't auto-fix structural issues without confirmation):
- Contradictions between pages; stale claims superseded by newer sources.
- Orphan pages (no inbound links); dead `[[links]]` to nonexistent pages.
- Concepts referenced but lacking their own page.
- Missing cross-references between obviously related pages.
- Thin `sources:` trails (claims with no citation).
- Data gaps a web search could fill — propose specific sources/questions.
Produce a prioritized fix list; execute approved fixes; log the pass.

## index.md format
Catalog grouped by category. One line per page:
`- [[concepts/cap-theorem]] — Consistency/Availability/Partition-tolerance trade-off · 2 sources`

## log.md format
Append-only. Every entry starts with a parseable header so
`grep "^## \[" log.md | tail -5` works:
`## [2026-05-15] ingest | Dynamo: Amazon's Highly Available Key-value Store`
Followed by a 2–5 line note: pages touched, contradictions, decisions.

## Working principles
- The human curates and questions; you do all bookkeeping. Touch every affected page in
  one pass — never leave the wiki half-updated.
- Precision over volume. A tight, well-linked page beats a long one. Cite everything.
- Surface disagreement between sources rather than averaging it away.
- Prefer updating an existing page over creating a near-duplicate; merge when you find dupes.
- When unsure how the human wants a workflow handled, ask, then encode the answer here.
