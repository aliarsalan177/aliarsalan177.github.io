# Agent Guide — Excel

> The world's most-used analysis tool. Lookups, aggregation, absolute references and pivot tables — done cleanly.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/excel/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Excel. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Lock references deliberately
- ✅ **APPLY:** Use absolute ($A$1) vs relative (A1) references correctly so formulas copy the way you intend (F4 to cycle).
- ⛔ **AVOID:** Copying a formula and having its references shift unexpectedly, silently corrupting a whole column.

### 2. Prefer XLOOKUP / INDEX-MATCH
- ✅ **APPLY:** Use XLOOKUP or INDEX+MATCH for robust, two-way lookups that survive column insertions.
- ⛔ **AVOID:** VLOOKUP with a hard-coded column index — it breaks the moment someone inserts a column.

### 3. Wrap risky formulas
- ✅ **APPLY:** Guard against errors with IFERROR and combine conditions with IF+AND/OR for clarity.
- ⛔ **AVOID:** Leaving #DIV/0!, #N/A, #REF! errors to propagate through dependent cells and dashboards.

### 4. Summarize with pivot tables
- ✅ **APPLY:** Use pivot tables to aggregate and explore large tables without fragile nested formulas.
- ⛔ **AVOID:** Building huge manual SUMIF/COUNTIF webs where a pivot table would be faster and less error-prone.

## Cheat Reference — concepts to remember

- **Lock references deliberately** — Use absolute ($A$1) vs relative (A1) references correctly so formulas copy the way you intend (F4 to cycle).
- **Prefer XLOOKUP / INDEX-MATCH** — Use XLOOKUP or INDEX+MATCH for robust, two-way lookups that survive column insertions.
- **Wrap risky formulas** — Guard against errors with IFERROR and combine conditions with IF+AND/OR for clarity.
- **Summarize with pivot tables** — Use pivot tables to aggregate and explore large tables without fragile nested formulas.

## Interview Questions

#### Q1. Relative vs absolute cell references?
Relative references (A1) shift when a formula is copied; absolute references ($A$1) stay fixed. Mixed ($A1 / A$1) lock only the row or column. F4 cycles through them.

#### Q2. Why prefer XLOOKUP/INDEX-MATCH over VLOOKUP?
VLOOKUP can only look right and depends on a hard-coded column number that breaks when columns move. XLOOKUP and INDEX-MATCH look in any direction and reference columns by identity, so they're more robust.

#### Q3. What is a pivot table for?
Quickly summarizing, grouping, and cross-tabulating large datasets (sums, counts, averages by category) interactively, without writing complex formulas.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
