# llm-wikis

A collection of **LLM-maintained knowledge bases** (inspired by Andrej Karpathy's
"LLM wiki" idea). Each subproject is an [Obsidian](https://obsidian.md) vault where a
human curates raw source material and an LLM agent builds and maintains a structured,
interlinked wiki on top of it.

This repo is a **monorepo of independent vaults** — each subdirectory is its own
self-contained wiki with its own agent contract, raw sources, and output.

## Subprojects

| Vault | What it is |
|---|---|
| [`SystemDesign/`](./SystemDesign) | Interview-prep knowledge base. System design & distributed systems at its core, plus applied scenario Q&A and behavioral/leadership interview material. |
| [`DDIA/`](./DDIA) | Companion wiki for *Designing Data-Intensive Applications* by Martin Kleppmann. The human drops chapter notes into `raw/`; the agent builds chapter summaries, concept pages, real-system pages, and the underlying research papers. |
| [`LLMWiki/`](./LLMWiki) | General-purpose knowledge base. Ingests arbitrary sources (articles, clippings, documents) into entities, concepts, source summaries, and synthesis pages. The original Karpathy-style vault. |

## The shared model

Every vault follows the same three-layer pattern:

1. **`raw/`** — immutable source documents. The human owns these; the agent only reads them.
2. **`wiki/`** — the agent's output: cross-linked Markdown pages, an `index.md` catalog,
   an append-only `log.md`, and an `overview.md` synthesis.
3. **The agent contract** — conventions and workflows (INGEST / QUERY / LINT) the agent
   must follow. See "Running with different tools" below for where this file lives.

The agent does all bookkeeping (linking, indexing, logging, consistency); the human
curates sources and asks questions. Read each vault's agent contract for its exact
schema — they differ in directory layout and frontmatter.

## Running with different tools

The agent contract is stored once per vault as **`AGENTS.md`** (the emerging cross-tool
standard) and exposed to each tool under the filename that tool expects, via symlinks:

```
<vault>/AGENTS.md                          # canonical contract (the real file)
<vault>/CLAUDE.md → AGENTS.md              # symlink — read by Claude Code
<vault>/.github/copilot-instructions.md → ../AGENTS.md   # symlink — read by GitHub Copilot
```

Edit `AGENTS.md` and every tool sees the change. To work on a vault, **open that vault
directory as the workspace root** (not the repo root) — instruction-file discovery is
relative to the opened folder.

| Tool | How it picks up the contract |
|---|---|
| **Claude Code** | `cd <vault> && claude` — auto-loads `CLAUDE.md`. |
| **OpenAI Codex** | Open `<vault>/` — auto-loads `AGENTS.md`. |
| **Cursor / Aider** | Open `<vault>/` as the project — both read `AGENTS.md`. |
| **GitHub Copilot** | Open `<vault>/` in VS Code with custom instructions enabled (`github.copilot.chat.codeGeneration.useInstructionFiles`) — reads `.github/copilot-instructions.md`. Copilot treats it as advisory context, so the strict INGEST/QUERY/LINT workflows are honored only partially. |
| **Any other agent** | Point it at `<vault>/AGENTS.md` manually, or symlink the filename it expects to `AGENTS.md`. |

> **Windows note:** these are real Git symlinks (mode `120000`). On Windows checkouts
> they may materialize as plain text files containing the target path unless
> `git config core.symlinks true` is set before cloning.

## Typical workflow

1. Drop a source into the relevant vault's `raw/` directory.
2. Open that vault with your tool of choice and say *"ingest \<file\>"* (or
   *"process this source"*).
3. The agent summarizes it, proposes key takeaways, and — on your direction —
   propagates it across the wiki, updating `index.md`, `overview.md`, and `log.md`.
4. Ask questions any time (*"QUERY"*); ask for a *"lint"* to health-check a vault.
