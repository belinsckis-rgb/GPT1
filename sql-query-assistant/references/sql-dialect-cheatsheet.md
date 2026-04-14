# SQL Dialect Cheatsheet

Use this file only when dialect conversion is needed.

## PostgreSQL → MySQL

- String aggregation: `STRING_AGG(x, ',')` → `GROUP_CONCAT(x SEPARATOR ',')`
- Date truncation: `DATE_TRUNC('month', ts)` → `DATE_FORMAT(ts, '%Y-%m-01')`
- Upsert: `INSERT ... ON CONFLICT (...) DO UPDATE` → `INSERT ... ON DUPLICATE KEY UPDATE`

## PostgreSQL → BigQuery

- Current timestamp: `NOW()` → `CURRENT_TIMESTAMP()`
- Date truncation: `DATE_TRUNC('month', ts)` → `DATE_TRUNC(DATE(ts), MONTH)`
- Intervals: `ts - INTERVAL '7 day'` → `TIMESTAMP_SUB(ts, INTERVAL 7 DAY)`

## PostgreSQL → SQL Server

- Limit: `LIMIT 10` → `TOP 10` (or `OFFSET ... FETCH`)
- Date add: `ts + INTERVAL '1 day'` → `DATEADD(day, 1, ts)`
- Null fallback: `COALESCE(a, b)` (same, preferred over `ISNULL` for portability)

## Portable query habits

- Keep CTE names semantic (`filtered_orders`, `customer_rollup`).
- Isolate engine-specific expressions in one CTE for easier rewrites.
- Avoid relying on implicit casting.
- Prefer ANSI joins and explicit predicates.
