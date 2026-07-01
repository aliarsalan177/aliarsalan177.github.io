# Agent Guide — Power BI

> Microsoft's BI platform: connect, model, and visualize data. Master Power Query (ETL), a star-schema model, and DAX.
> Category: AI & Data · Source: https://aliarsalan177.github.io/guides/power-bi/

## Purpose

A field guide for **AI coding agents** (and engineers): the concepts to apply and the
mistakes to avoid when building with Power BI. Load this into your agent's context so it
keeps these concepts in mind and does **not** repeat these mistakes.

## Rules — Apply / Avoid

### 1. Shape data in Power Query first
- ✅ **APPLY:** Do cleaning/typing/shaping in Power Query (M) — the ETL layer — before it hits the model.
- ⛔ **AVOID:** Loading raw, wide, messy tables and patching them with fragile DAX later.

### 2. Model as a star schema
- ✅ **APPLY:** Build fact tables surrounded by dimension tables with single-direction 1-to-many relationships.
- ⛔ **AVOID:** Flat one-big-table models and many-to-many/bidirectional relationships that break filter logic and performance.

### 3. Measures over calculated columns
- ✅ **APPLY:** Use DAX measures (computed at query time, respect filter context) for aggregations; understand row vs filter context.
- ⛔ **AVOID:** Calculated columns for aggregations — they bloat the model and don't respond to slicers like measures do.

### 4. Secure with Row-Level Security
- ✅ **APPLY:** Define RLS roles/filters so users only see their data; test with 'View as role'.
- ⛔ **AVOID:** Relying on hidden visuals/pages for security — the underlying data is still accessible.

### 5. Choose the right refresh/storage mode
- ✅ **APPLY:** Import for speed, DirectQuery for real-time/large sources, Incremental refresh for big historical tables.
- ⛔ **AVOID:** Full refresh of massive datasets every time, or DirectQuery on slow sources for interactive dashboards.

## Cheat Reference — concepts to remember

- **Shape data in Power Query first** — Do cleaning/typing/shaping in Power Query (M) — the ETL layer — before it hits the model.
- **Model as a star schema** — Build fact tables surrounded by dimension tables with single-direction 1-to-many relationships.
- **Measures over calculated columns** — Use DAX measures (computed at query time, respect filter context) for aggregations; understand row vs filter context.
- **Secure with Row-Level Security** — Define RLS roles/filters so users only see their data; test with 'View as role'.
- **Choose the right refresh/storage mode** — Import for speed, DirectQuery for real-time/large sources, Incremental refresh for big historical tables.

## Full Cheat Sheet — every concept

### Components
- Power BI Desktop (author reports), Service (cloud share/collaborate, workspaces), Mobile, Embedded, Report Server (on-prem).
- Views: Report, Data, Model (relationships).

### Power Query (ETL / M)
- Connect to files, databases, cloud & online services; 100s of connectors.
- Transform: filter, merge (JOIN), append (UNION), pivot/unpivot, split, typed columns.
- Reference queries, custom M functions; applied-steps are reproducible.

### Data Modeling
- Star schema: fact + dimension tables; relationships with cardinality & single cross-filter direction.
- Hierarchies, calculated tables, date/calendar table for time intelligence.

### DAX
- Measures (query-time) vs calculated columns (row-time); aggregations SUM/AVERAGE/COUNT.
- CALCULATE (modify filter context), FILTER, ALL, RELATED; iterators SUMX/AVERAGEX.
- Time intelligence: TOTALYTD, SAMEPERIODLASTYEAR, DATEADD.

### Visuals & Sharing
- Charts, tables, cards, maps, slicers; custom visuals from marketplace; drill-through & bookmarks.
- Reports vs dashboards; publish to Service; apps & subscriptions.

### Security & Refresh
- Row-Level Security (roles + DAX filters); on-premises data gateway.
- Refresh: Import / DirectQuery / Incremental; schedule refreshes.

## Interview Questions

#### Q1. Power Query vs DAX — what's each for?
Power Query (M language) is the ETL layer: connect, clean, transform, and shape data before load. DAX (Data Analysis Expressions) computes analytics over the loaded model — measures, calculated columns, and KPIs at query time.

#### Q2. Calculated column vs measure?
A calculated column is computed per row at refresh and stored in the model (row context). A measure is computed at query time and respects the current filter context (slicers, rows/columns) — use measures for aggregations.

#### Q3. What is a star schema and why use it?
Central fact tables (events/metrics) linked to dimension tables (attributes) via one-to-many relationships. It simplifies DAX, keeps filters flowing predictably, and performs far better than a flat table.

#### Q4. Import vs DirectQuery?
Import loads data into Power BI's in-memory engine (fast, but needs refresh). DirectQuery queries the source live (real-time, handles huge data, but slower and source-dependent). Composite models mix both.

#### Q5. What is row context vs filter context in DAX?
Row context is the current row (calculated columns, iterators like SUMX). Filter context is the set of filters applied (slicers, visuals). CALCULATE modifies filter context — the key to advanced DAX.

---

_Curated by Ali Arsalan · https://aliarsalan177.github.io · Generated from the Engineering Guides._
