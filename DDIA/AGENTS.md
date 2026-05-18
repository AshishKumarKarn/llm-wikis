# DDIA Wiki — Schema & Operating Manual

This repository is an LLM-maintained companion wiki for **_Designing Data-Intensive
Applications_ by Martin Kleppmann (DDIA)**. The human reads the book and drops chapter
notes into `raw/`. You (the LLM) read those notes and build/maintain a structured,
interlinked wiki under `wiki/`.

**Prime directive: thoroughness.** This wiki is the human's go-to place for *any*
query about the book's material — concepts, trade-offs, real systems, the papers
behind the ideas. Depth and correctness beat brevity. When in doubt, write the
fuller explanation, add the cross-reference, flag the contradiction. Never leave a
concept half-documented because it was tedious.

---

## Layers

1. **`raw/`** — Immutable source of truth. The human's chapter notes / pasted text /
   excerpts. You READ these; you NEVER modify them. One file per source is typical
   (e.g. `raw/ch05-replication-notes.md`).
2. **`wiki/`** — You own this entirely. Markdown pages you create and maintain.
3. **`CLAUDE.md`** (this file) — The conventions and workflows. Co-evolves with the
   human. If a workflow proves clumsy, propose an edit here.

---

## Wiki layout

```
wiki/
  index.md            # Content catalog — every page, one-line summary. Read FIRST on any query.
  log.md              # Append-only chronological record of ingests / queries / lints.
  overview.md         # The evolving big-picture synthesis of the whole book so far.
  chapters/           # One page per book chapter. The summary spine.
  concepts/           # Topic pages: replication, consensus, isolation levels, LSM-trees, CAP...
  systems/            # Real systems the book discusses: PostgreSQL, Cassandra, Kafka, Spanner...
  papers/             # The research papers DDIA cites: Dynamo, Spanner, Raft, Bigtable...
  comparisons/        # Head-to-head pages: B-tree vs LSM-tree, leader vs leaderless replication...
```

### The book's structure (use canonical chapter numbers)

- **Part I — Foundations of Data Systems**
  - Ch 1 — Reliable, Scalable, and Maintainable Applications
  - Ch 2 — Data Models and Query Languages
  - Ch 3 — Storage and Retrieval
  - Ch 4 — Encoding and Evolution
- **Part II — Distributed Data**
  - Ch 5 — Replication
  - Ch 6 — Partitioning
  - Ch 7 — Transactions
  - Ch 8 — The Trouble with Distributed Systems
  - Ch 9 — Consistency and Consensus
- **Part III — Derived Data**
  - Ch 10 — Batch Processing
  - Ch 11 — Stream Processing
  - Ch 12 — The Future of Data Systems

Chapter pages: `wiki/chapters/ch05-replication.md`. Concept/system/paper/comparison
pages: kebab-case slug, e.g. `wiki/concepts/quorum-consistency.md`,
`wiki/systems/apache-kafka.md`, `wiki/papers/dynamo.md`,
`wiki/comparisons/btree-vs-lsm-tree.md`.

---

## Page conventions

Every wiki page starts with YAML frontmatter (enables Obsidian Dataview):

```markdown
---
title: Quorum Consistency
type: concept            # chapter | concept | system | paper | comparison | overview
chapters: [5]            # which book chapters touch this (numbers)
tags: [replication, consistency, distributed]
status: stub             # stub | developing | solid   — honesty about depth
updated: 2026-05-16
---
```

Then the body. Conventions:

- **Wikilinks everywhere.** Use `[[quorum-consistency]]` (the slug, no folder, no
  `.md`) to link concepts/systems/papers/chapters. Link liberally — a link to a page
  that doesn't exist yet is a *good* signal (it marks a page worth writing). Obsidian
  resolves these.
- **Cite the chapter.** When a claim comes from the book, note the chapter: *(DDIA
  Ch 5)*. When it comes from a cited paper or your own knowledge, say so explicitly —
  never blur the book's claims with outside knowledge.
- **Trade-offs are first-class.** DDIA is a book about trade-offs. Every concept page
  should have a "Trade-offs" or "When to use / when not" section where relevant.
- **Flag contradictions & nuance.** If a later chapter complicates an earlier claim,
  add a `> [!warning]` callout on both pages and cross-link them.
- **Concept pages are the payload.** Chapter pages summarize and link out; the deep,
  reusable explanation lives in concept pages so it's findable from any angle.

### Chapter page template

```markdown
## One-paragraph thesis
## Key ideas (bulleted, each linking to its concept page)
## Concepts introduced  → [[...]] list
## Systems / papers referenced  → [[...]] list
## Trade-offs & tensions
## Connections to other chapters
## Open questions / things to revisit
```

### Concept page template

```markdown
## Definition (precise, one or two sentences)
## Why it matters / what problem it solves
## How it works (the mechanism, with detail)
## Trade-offs
## Real systems that use it  → [[...]]
## Related concepts  → [[...]]
## Sources: DDIA Ch X; [[paper-slug]] if applicable
```

---

## Operations

### INGEST (human drops a source in `raw/` and says "ingest it")

1. **Read** the raw source fully.
2. **Discuss** key takeaways with the human in chat *before* heavy writing — confirm
   emphasis, surface anything surprising or contradictory. (Skip discussion only if
   the human explicitly says "just file it.")
3. **Write/update the chapter page** in `wiki/chapters/`.
4. **Create or update every concept page** the source touches. This is where
   thoroughness lives — do not skip a concept because its page would be long. Prefer
   updating an existing concept page over duplicating.
5. **Create/update system & paper pages** for anything real-world or research the
   source references. A stub with frontmatter + a "what it is" line + backlinks is
   acceptable for tangential mentions; core systems get full pages.
6. **Update `overview.md`** — fold the new material into the running synthesis.
7. **Update `index.md`** — add/refresh every page touched.
8. **Append to `log.md`** — one entry (format below).
9. A single chapter ingest typically touches 10–20 wiki pages. That is expected and
   correct — err toward more cross-referencing, not less.

### QUERY (human asks a question)

1. Read `wiki/index.md` first to locate relevant pages.
2. Read those pages (and follow wikilinks as needed). Prefer the wiki over
   re-deriving from `raw/`, but consult `raw/` if the wiki is thin on the topic.
3. Answer with **citations to wiki pages and book chapters**. Be thorough — this is
   the go-to reference; a shallow answer is a failure even if technically correct.
4. **File good answers back.** If the answer is a synthesis/comparison/analysis worth
   keeping, write it as a new wiki page (usually under `comparisons/` or `concepts/`),
   update `index.md`, and log it. Ask the human if unsure whether to file.
5. Log the query in `log.md`.

### LINT (human says "lint the wiki" / health check)

Scan for and report (don't silently fix structural issues — propose, then act on
confirmation for big changes; small fixes like adding a missing backlink are fine to
just do):

- Contradictions between pages not yet flagged.
- Stale claims a later chapter superseded.
- Orphan pages (no inbound wikilinks).
- Concepts referenced but with no page yet (broken-looking wikilinks that *should*
  exist).
- Missing cross-references between obviously related pages.
- `status: stub` pages that are now ingestible into something fuller.
- Data/knowledge gaps a targeted web search or the original paper could fill —
  suggest these proactively.

---

## log.md entry format

Append-only. One entry per operation. Consistent prefix so `grep "^## \[" log.md`
works:

```
## [2026-05-16] ingest | Ch 5 Replication
- Pages: chapters/ch05-replication (new), concepts/leader-based-replication (new),
  concepts/quorum-consistency (new), ... 
- Notable: flagged contradiction with [[eventual-consistency]] re: read-your-writes.
- Follow-ups: paper page for [[dynamo]] still a stub; revisit after Ch 9.
```

Operation types: `ingest`, `query`, `lint`, `meta` (schema/structure changes).

---

## Style & honesty rules

- **Never fabricate.** If the human's notes don't cover something and you're filling
  from your own knowledge of DDIA, mark it clearly (e.g. *"(not in the provided
  notes; standard DDIA treatment:)"*). The human needs to trust this wiki absolutely.
- **`status` honestly.** A page you dashed off is `stub` or `developing`, not `solid`.
- **Precision over hedging.** This is a technical book; vague summaries are worthless.
  Name the algorithm, the consistency level, the failure mode.
- **Obsidian is the reader's IDE.** Keep wikilinks valid-by-convention and frontmatter
  clean so graph view and Dataview stay useful.
- **Propose schema changes.** If a recurring need isn't served by this manual, edit
  this file (operation type `meta` in the log) and tell the human.
