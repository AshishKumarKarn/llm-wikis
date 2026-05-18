# LLM Wiki — Schema & Operating Rules

Read this file at the start of every session. Follow it precisely and completely.

---

## Directory Layout

```
LLMWiki/
├── CLAUDE.md                  ← this file (rules — never modify without user instruction)
├── raw/                       ← source documents (immutable — read only, never edit)
│   └── assets/                ← downloaded images referenced by sources
├── wiki/                      ← everything the LLM writes and maintains
│   ├── index.md               ← master catalog of all wiki pages
│   ├── log.md                 ← append-only chronological activity log
│   ├── overview.md            ← high-level synthesis across all knowledge
│   ├── entities/              ← people, organizations, places, products
│   ├── concepts/              ← ideas, frameworks, theories, methods
│   ├── sources/               ← one summary page per raw source document
│   └── synthesis/             ← filed answers, comparisons, analyses
└── Welcome.md                 ← original Obsidian placeholder (ignore)
```

---

## Page Frontmatter

Every wiki page must begin with YAML frontmatter:

```yaml
---
title: Human-Readable Title
type: entity | concept | source | synthesis | overview
tags: [tag1, tag2, tag3]
sources: 0        # count of raw sources this page draws from
updated: YYYY-MM-DD
---
```

---

## File Naming Conventions

| Type | Pattern | Example |
|------|---------|---------|
| Source | `YYYY-MM-DD-short-title.md` | `2026-05-15-attention-is-all-you-need.md` |
| Entity | `firstname-lastname.md` or `org-name.md` | `geoffrey-hinton.md`, `openai.md` |
| Concept | descriptive noun phrase | `transformer-architecture.md` |
| Synthesis | descriptive phrase | `gpt-vs-gemini-comparison.md` |

Rules: lowercase, hyphens only, no spaces, no special characters.

---

## Cross-Linking Rules

- Use Obsidian wiki-links everywhere: `[[entities/sam-altman]]`, `[[concepts/attention-mechanism]]`
- Any entity or concept with its own page must be linked on every page where it appears.
- Every page ends with a `## See Also` section listing 3–5 related wiki links.
- Orphan pages (no inbound links) are a lint error — always link back.

---

## Operation: Ingest

**Trigger:** User drops a file into `raw/` and says "ingest [filename]" or "process [filename]".

**Steps — execute in order, do not skip:**

1. Read the source file completely.
2. Discuss with the user: What are the 3–5 key takeaways? What is surprising? Does anything contradict existing wiki content?
3. Create `wiki/sources/<YYYY-MM-DD-slug>.md` — structured summary (see Source Page Format below).
4. Add the new source to `wiki/index.md` under the Sources section.
5. For each entity mentioned: update or create `wiki/entities/<slug>.md`.
6. For each concept mentioned: update or create `wiki/concepts/<slug>.md`.
7. Update `wiki/overview.md` — revise the synthesis paragraph if needed, update stats.
8. Append an entry to `wiki/log.md` (see Log Format below).
9. Report to the user: list every file created or modified with one-line description of change.

A single ingest typically touches 5–15 wiki pages. Do not shortcut.

**Source Page Format:**

```markdown
---
title: Source Title
type: source
tags: [tag1, tag2]
sources: 1
updated: YYYY-MM-DD
---

# Source Title

**Author(s):** Name  
**Date:** YYYY-MM-DD  
**Type:** article | paper | book | podcast | video | other  
**Original:** `raw/filename.md`

## Summary

2–4 paragraph summary of the source's core argument and findings.

## Key Takeaways

- Takeaway 1
- Takeaway 2
- Takeaway 3

## Key Entities

- [[entities/name]] — brief role in this source

## Key Concepts

- [[concepts/name]] — brief role in this source

## Conflicts & Contradictions

> Note any claims that conflict with other wiki pages. Format:
> **Conflict with [[sources/other-source]]:** This source says X; the other says Y.

## Quotes

> Notable direct quotes with context.

## See Also

- [[wiki/link-1]]
- [[wiki/link-2]]
```

---

## Operation: Query

**Trigger:** User asks any question.

**Steps:**

1. Read `wiki/index.md` to identify relevant pages.
2. Read those pages in full.
3. Synthesize an answer. Cite wiki pages inline: "According to [[sources/paper-name]]..."
4. After answering, ask: "Should I file this as a synthesis page?" If yes:
   - Create `wiki/synthesis/<slug>.md`
   - Add to index
   - Append to log

**Synthesis Page Format:**

```markdown
---
title: Query Title
type: synthesis
tags: [tag1, tag2]
sources: N
updated: YYYY-MM-DD
---

# Query Title

**Original query:** Exact user question  
**Date:** YYYY-MM-DD

## Answer

Full synthesized answer with wiki citations.

## Sources Used

- [[sources/a]]
- [[sources/b]]

## See Also

- [[concepts/related]]
```

---

## Operation: Lint

**Trigger:** User says "lint", "health check", or "audit the wiki".

**Check for:**

1. Orphan pages — pages with no inbound links from other wiki pages
2. Contradictions — claims on one page that conflict with claims on another
3. Missing concept pages — concepts mentioned on 2+ pages but lacking their own page
4. Stale pages — `updated` date older than 30 days on high-traffic pages
5. Missing cross-references — obviously related pages not linking to each other
6. Data gaps — important questions the wiki can't answer yet
7. Oversized pages — pages exceeding ~400 lines (consider splitting)

Report as a numbered checklist, sorted by severity. Ask user which to act on.

---

## Index Format

`wiki/index.md` must maintain these sections in order:

```markdown
# Wiki Index

**Last updated:** YYYY-MM-DD  
**Stats:** X sources | Y entities | Z concepts | W synthesis pages

## Overview
[[wiki/overview]] — master synthesis of all knowledge

## Sources
| Page | Summary | Date | Tags |
|------|---------|------|------|

## Entities
| Page | Type | Summary |
|------|------|---------|

## Concepts
| Page | Summary | Sources |
|------|---------|---------|

## Synthesis
| Page | Query | Date |
|------|-------|------|
```

Summaries: one line, ≤ 120 characters. Update this file on every ingest and every filed synthesis.

---

## Log Format

```
## [YYYY-MM-DD] operation | Title or description
- Detail line 1
- Detail line 2
```

Operations: `ingest`, `query`, `synthesis`, `lint`, `update`, `init`

Log is **append-only** — never edit past entries. New entries go at the bottom.

---

## Quality Standards

- Every factual claim on a wiki page must be traceable to a source page.
- When sources contradict: note it explicitly using the Conflict format above.
- Keep pages focused. Split any page exceeding ~400 lines.
- Never fabricate citations. If you don't know, say so.
- Prefer updating existing pages over creating new ones for minor additions.

---

## Session Start Protocol

At the start of every session, execute this checklist silently, then greet the user:

1. Read `CLAUDE.md` (this file).
2. Read `wiki/log.md` — last 10 entries to understand recent activity.
3. Read `wiki/index.md` — to know the full wiki state.
4. Greet the user with:
   - Wiki stats: page count by type
   - Last 3 log entries (date + title only)
   - One-line wiki health note (any obvious issues)
   - Prompt: "What would you like to do? (ingest / query / lint)"

---

## Handling Missing or Ambiguous Sources

- If the user references a source not in `raw/`, ask them to drop the file in before ingesting.
- If a source is ambiguous (same name, different content), ask for clarification.
- Never ingest from memory — always read the actual file.

---

## Evolution of This Schema

This schema should evolve as the wiki grows. When the user says "update the schema" or "change the rules", modify this file and append an `update` entry to the log. Always explain what changed and why.
