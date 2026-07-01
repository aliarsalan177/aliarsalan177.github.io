# Agent Guide — SQL

> The language of relational data. Master joins, aggregation, indexing and NULL semantics to query correctly and fast.
> Category: Backend & Infra · Source: https://aliarsalan177.github.io/guides/sql/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with SQL. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Always join on a condition
- ✅ **APPLY:** Specify the ON/WHERE relationship for every join; pick the join type (INNER/LEFT) intentionally.
- ⛔ **AVOID:** A missing join condition — it produces a cartesian product that explodes row counts and grinds to a halt.

### 2. NULL is not a value
- ✅ **APPLY:** Test with IS NULL / IS NOT NULL and normalize with COALESCE; know aggregates skip NULLs.
- ⛔ **AVOID:** Comparing with `= NULL` (always false) or forgetting NULLs vanish from counts and equality filters.

### 3. Index for your access patterns
- ✅ **APPLY:** Index columns used in WHERE/JOIN/ORDER BY; read the query plan to confirm they're used.
- ⛔ **AVOID:** Over-indexing (slows writes) or wrapping indexed columns in functions (which disables the index).

### 4. WHERE filters rows, HAVING filters groups
- ✅ **APPLY:** Filter individual rows in WHERE, then filter aggregated groups in HAVING after GROUP BY.
- ⛔ **AVOID:** Putting aggregate conditions (COUNT(*) > 5) in WHERE — aggregates only work in HAVING.

## Cheat Reference — concepts to remember

- **Always join on a condition** — Specify the ON/WHERE relationship for every join; pick the join type (INNER/LEFT) intentionally.
- **NULL is not a value** — Test with IS NULL / IS NOT NULL and normalize with COALESCE; know aggregates skip NULLs.
- **Index for your access patterns** — Index columns used in WHERE/JOIN/ORDER BY; read the query plan to confirm they're used.
- **WHERE filters rows, HAVING filters groups** — Filter individual rows in WHERE, then filter aggregated groups in HAVING after GROUP BY.

## Full Cheat Sheet — every concept

### Query Basics
- SELECT cols FROM table WHERE filter ORDER BY col ASC/DESC LIMIT n.
- Order of evaluation: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT.
- DISTINCT removes duplicate rows.

### Grouping & Aggregates
- GROUP BY collapses rows; aggregates: COUNT, SUM, AVG, MIN, MAX (ignore NULLs).
- WHERE filters rows (no aggregates); HAVING filters groups (aggregates allowed).

### Joins
- INNER JOIN = matches in both; LEFT/RIGHT JOIN = all of one side + matches (NULLs otherwise).
- FULL OUTER JOIN = all rows from both.
- Always specify the ON condition — a missing one causes a cartesian product.

### Structure & Integrity
- Primary key uniquely identifies a row; foreign key links tables.
- Normalization removes redundancy; indexes speed reads (cost writes).
- Transactions are ACID (Atomic, Consistent, Isolated, Durable).

### Gotchas
- NULL: use IS NULL / IS NOT NULL, and COALESCE() to default.
- UNION dedupes; UNION ALL keeps duplicates.
- Subqueries run first and feed the outer query.

### DDL, Views & Expressions
- DDL: CREATE/ALTER/DROP TABLE, CREATE DATABASE; constraints (NOT NULL, UNIQUE, CHECK, DEFAULT).
- CREATE VIEW (saved query) and CREATE INDEX (speed reads); DROP to remove.
- Filters: BETWEEN, IN, LIKE (wildcards % _), DISTINCT; aliases with AS.
- CASE WHEN … THEN … ELSE … END for conditional logic; COALESCE for NULL fallback.
- DML: INSERT, UPDATE, DELETE — always with a WHERE (or you hit every row).

## Interview Questions

#### Q1. INNER JOIN vs LEFT JOIN?
INNER JOIN returns only rows matching in both tables. LEFT JOIN returns all rows from the left table plus matches from the right, filling NULLs where there's no match.

#### Q2. WHERE vs HAVING?
WHERE filters rows before grouping and can't use aggregates; HAVING filters after GROUP BY and is where aggregate conditions (e.g., SUM(x) > 100) belong.

#### Q3. How do indexes speed up queries, and what's the trade-off?
An index is a sorted structure (usually a B-tree) enabling O(log n) lookups instead of full scans on indexed columns. The cost is extra storage and slower writes, since every insert/update maintains the index.

#### Q4. Why is NULL handling tricky?
NULL means 'unknown', so comparisons with it yield unknown (not true) — you must use IS NULL. Aggregates ignore NULLs, and they can silently drop rows from equality filters and inner joins.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
