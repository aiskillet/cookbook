---
name: sql-optimization
description: Make slow queries fast — read the query plan, index for the access pattern, and avoid N+1s and needless scans. Use when a query is slow, designing indexes, or reviewing DB access code.
---

# SQL Optimization

Don't guess — **measure with the query plan**, then fix the specific cause. Most slowness is a missing index, an N+1, or fetching far more than you need.

## When to Activate
- A query or endpoint is slow
- Designing indexes for a table
- Reviewing ORM/DB access code

## Step 1: read the plan
Run `EXPLAIN ANALYZE`. Look for:
- **Seq Scan** on a big table where you filter/join → likely a missing index.
- **Rows estimated ≫ actual** (or vice versa) → stale statistics (`ANALYZE`).
- **Nested loop over many rows** → wrong join strategy, often missing index on the join key.
- The **most expensive node** — optimize that, not the whole query.

## The high-impact fixes
- **Index for the access pattern:** columns in `WHERE`, `JOIN`, `ORDER BY`. Composite index column order = equality columns first, then range/sort.
- **Covering index** — include selected columns so the DB never touches the table (index-only scan).
- **Select only needed columns** — `SELECT *` kills covering indexes and ships junk.
- **Kill N+1** — one query with `JOIN`/`WHERE id IN (...)` instead of a query per row.
- **Paginate with keyset** (`WHERE id > ?`) not `OFFSET` on large tables.
- **Filter before joining/aggregating**; push predicates down.

## Watch out
- Functions on indexed columns (`WHERE lower(email)=...`) disable the index — store normalized or use an expression index.
- Leading wildcard `LIKE '%x'` can't use a b-tree index.
- Over-indexing slows writes — index for real query patterns, not "just in case."

## Checklist
- [ ] Read `EXPLAIN ANALYZE`; found the costly node
- [ ] Index covers WHERE/JOIN/ORDER BY in the right order
- [ ] Selecting only needed columns
- [ ] No N+1; batched instead
- [ ] Keyset pagination for big tables
- [ ] Statistics fresh (`ANALYZE`)
