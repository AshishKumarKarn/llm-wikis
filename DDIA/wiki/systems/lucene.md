---
title: Lucene (Elasticsearch / Solr)
type: system
chapters: [3]
tags: [system, search, full-text, lsm-tree]
status: stub
updated: 2026-05-16
---

# Lucene (Elasticsearch / Solr)

Full-text indexing engine behind Elasticsearch and Solr. Stores its term→postings
dictionary in **SSTable-like sorted files** merged in the background (see
[[sstables-and-lsm-trees]]). Its in-memory index is a finite-state automaton over key
characters, transformable into a **Levenshtein automaton** for within-edit-distance
fuzzy search — see [[full-text-and-fuzzy-indexes]].

> `status: stub` — Ch 3. May resurface in derived-data/search-index discussions
> (Part III).

## Related

- [[full-text-and-fuzzy-indexes]] · [[sstables-and-lsm-trees]]
