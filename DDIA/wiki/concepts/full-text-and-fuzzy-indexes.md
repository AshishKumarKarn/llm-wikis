---
title: Full-Text & Fuzzy Indexes
type: concept
chapters: [3]
tags: [storage, indexes, search, lucene]
status: developing
updated: 2026-05-16
---

# Full-Text & Fuzzy Indexes

## Definition

The indexes covered so far need *exact* keys (or sorted ranges). **Fuzzy** querying —
similar/misspelled keys, synonyms, grammatical variants, proximity — needs different
techniques. *(DDIA Ch 3)*

## How it works

- A full-text index is a key-value structure: key = a term (word), value = the
  **postings list** (IDs of documents containing it). [[lucene]] keeps the
  term→postings mapping in **SSTable-like sorted files** merged in the background
  (see [[sstables-and-lsm-trees]]).
- **Edit distance** (1 = one letter added/removed/replaced) enables typo-tolerant
  search. Lucene's in-memory index is a **finite-state automaton over key
  characters** (trie-like), transformable into a **Levenshtein automaton** for
  efficient within-edit-distance search.
- Further fuzzy techniques head toward document classification / machine learning
  (information-retrieval territory, mostly out of scope for DDIA).

## Related concepts

- [[sstables-and-lsm-trees]] · [[lucene]] · [[multi-dimensional-indexes]]

## Sources

DDIA Ch 3 ("Full-text search and fuzzy indexes").
