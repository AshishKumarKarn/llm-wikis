---
title: N+1 Query Problem
type: pattern
tags: [database, performance, orm, anti-pattern]
sources: [system-design]
created: 2026-05-15
updated: 2026-05-15
---

# N+1 Query Problem

An **anti-pattern** where fetching N parent records then querying each child individually
produces N+1 database roundtrips instead of 1 (see [[sources/system-design-study-guide]]).

## The problem
```sql
SELECT * FROM students;           -- 1 query
SELECT * FROM courses WHERE student_id = ?;  -- ×N queries
```
100 students → 101 queries. Scales linearly: 1000 students = 1001 queries. Causes:
excessive DB load, network overhead, slow response times.

## Fixes
1. **JOIN** — fetch parent + children in one query (most efficient):
   ```sql
   SELECT students.*, courses.*
   FROM students LEFT JOIN courses ON students.id = courses.student_id;
   ```
2. **Batch query** — 2 queries total:
   ```sql
   SELECT * FROM courses WHERE student_id IN (1, 2, 3, ...);
   ```
3. **ORM eager loading:** TypeORM `.leftJoinAndSelect()` · Spring Data JPA `@Query`
   with `JOIN FETCH` · Hibernate Second-Level Cache.

## Watch out for Cartesian products
`JOIN FETCH` on multiple one-to-many associations in the same query causes a Cartesian
product (rows multiply). Use separate queries for each association instead.

## Related
[[components/aws-aurora]] · [[scenarios/multi-tenant-usage-dashboard]]
