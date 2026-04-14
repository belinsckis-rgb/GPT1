---
name: sql-query-assistant
description: Write, explain, debug, and optimize SQL queries for analytics and application workloads. Use when a task involves SELECT/JOIN/CTE/window functions, schema-aware query design, performance tuning (indexes, execution plans), query safety reviews, SQL dialect adaptation (PostgreSQL/MySQL/SQLite/BigQuery/SQL Server), or transforming business questions into reliable SQL.
---

# SQL Query Assistant

## Overview

Use this skill to turn business questions into correct, readable, and efficient SQL. Confirm schema assumptions, choose a dialect-safe strategy, then deliver production-ready queries with clear explanations.

## Workflow

1. Clarify objective and output
   - Restate the metric/question and expected columns.
   - Confirm grain (row-level, daily, customer-level, etc.).
2. Validate schema assumptions
   - List required tables, keys, filters, and time columns.
   - If schema is missing, provide explicit assumptions before writing SQL.
3. Build query incrementally
   - Start with minimal filtered dataset.
   - Add joins with key rationale.
   - Add aggregations/window logic last.
4. Check correctness
   - Validate join cardinality (1:1, 1:N, N:N).
   - Check NULL behavior, duplicates, and date boundaries.
5. Optimize and harden
   - Remove unnecessary columns/CTEs.
   - Suggest indexes/partition filters when relevant.
   - Provide parameterized variants for app usage.

## Output format

When returning SQL, always provide:

- `Goal`: one-sentence intent
- `Assumptions`: schema/dialect assumptions
- `Query`: final SQL block
- `Validation checks`: 2-4 quick checks (row counts, duplicate tests, NULL checks)
- `Optimization notes`: optional but concise

## Safety and quality rules

- Prefer explicit column lists over `SELECT *`.
- Use deterministic ordering when returning top-N results.
- Avoid destructive statements (`DELETE`, `TRUNCATE`, `DROP`) unless the user explicitly requests them.
- For updates/deletes, provide a safe preview query first (e.g., matching `SELECT`).
- Use dialect-appropriate date/time functions.
- Keep aliases clear and consistent.

## Dialect handling

If dialect is unclear, ask once. If no answer is available, default to PostgreSQL syntax and mention what to change for other engines.

For quick adaptation patterns, read `references/sql-dialect-cheatsheet.md`.
