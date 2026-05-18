---
title: Database Indexes
type: concept
tags: [database, indexes, clustered, query-optimization, concepts]
sources: [miscellaneous]
created: 2026-05-16
updated: 2026-05-16
---

# Database Indexes

Index structures that speed up data retrieval at the cost of write overhead
(see [[sources/miscellaneous-study-guide]]).

## Three types

### Clustered Index
- **Determines the physical order** of rows in the table. Data IS the index.
- Only **one per table** (data can only be physically ordered one way).
- Automatically created on primary key in most RDBMS (MySQL InnoDB, SQL Server).
- Great for **range queries** and ordered retrieval.
```sql
CREATE CLUSTERED INDEX idx_emp_id ON employees(emp_id);
-- Rows stored physically in ascending emp_id order
```

### Non-Clustered Index
- **Separate structure** that stores index keys + pointers to actual data rows.
- Multiple non-clustered indexes can exist per table.
- Useful for lookups on non-primary-key columns.
- Slower for range queries vs clustered (pointer lookup overhead).
```sql
CREATE NONCLUSTERED INDEX idx_salary ON employees(salary);
-- Lookup structure for salary → points to clustered index rows
```

### Composite Index
- Index built on **multiple columns**.
- Column order matters: **leftmost prefix rule** — queries filtering only on the non-first
  column may not use the index efficiently.
- Can be clustered or non-clustered.
```sql
CREATE INDEX idx_location ON users(city, state);
-- WHERE city='Bangalore' AND state='KA' ← uses index ✅
-- WHERE state='KA'                       ← may not use index efficiently ❌
```

## Comparison

| Feature | Clustered | Non-Clustered | Composite |
|---|---|---|---|
| Physical row order | Sets it | Separate structure | Depends on type |
| Count per table | 1 | Many | Many |
| Range queries | Excellent | Slower (pointer hop) | Good if leading columns match |
| Write cost | Reorders data on insert | Updates index separately | Updates per indexed column |

## When to add indexes
- Columns frequently in `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`
- Foreign key columns (prevent full scans on joins)
- High-cardinality columns (many distinct values = more selective)

## When to avoid / remove indexes
- Columns rarely queried
- Small tables (full scan may be faster)
- Heavy write-heavy workloads (each write updates all indexes)
- **Slow DB writes?** — check for unused indexes first. See [[scenarios/slow-db-writes]].

## Leftmost prefix rule (composite)
For `INDEX(city, state)`:
- `WHERE city = 'X'` ← uses index ✅
- `WHERE city = 'X' AND state = 'Y'` ← uses index ✅
- `WHERE state = 'Y'` ← cannot use this index ❌ (not leftmost)

## Related
[[scenarios/slow-db-writes]] · [[patterns/n-plus-1-query]] ·
[[comparisons/sharding-vs-partitioning]]
