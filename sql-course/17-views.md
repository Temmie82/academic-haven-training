# 17. Views

A **view** is a saved `SELECT` query you can use like a table. It does not store data — it stores the query.

## Why use views?

- Hide complex joins and calculations behind a friendly name.
- Provide a stable interface even if underlying tables change.
- Restrict which columns or rows users can see (basic security).

## Create a view

```sql
CREATE VIEW order_summary AS
SELECT o.order_id,
       c.first_name || ' ' || c.last_name        AS customer_name,
       o.order_date,
       o.status,
       ROUND(SUM(oi.quantity * oi.unit_price),2) AS order_total
FROM   orders      o
JOIN   customers   c  ON o.customer_id = c.customer_id
JOIN   order_items oi ON o.order_id    = oi.order_id
GROUP BY o.order_id, customer_name, o.order_date, o.status;
```

## Query a view like a table

```sql
SELECT *
FROM   order_summary
ORDER BY order_total DESC;
```

```sql
SELECT customer_name, SUM(order_total) AS lifetime_value
FROM   order_summary
GROUP BY customer_name;
```

## Replace or drop a view

```sql
CREATE VIEW IF NOT EXISTS order_summary AS ...;   -- SQLite
CREATE OR REPLACE VIEW order_summary AS ...;      -- PostgreSQL / MySQL

DROP VIEW IF EXISTS order_summary;
```

## Updatable views

Some simple views (single table, no aggregation) accept `INSERT`/`UPDATE`/`DELETE`. Most complex views are read-only.

## Materialized views (PostgreSQL, Oracle)

Stores the result on disk. Faster reads, but data is only refreshed when you say so.

```sql
CREATE MATERIALIZED VIEW order_summary_mv AS
SELECT ... ;

REFRESH MATERIALIZED VIEW order_summary_mv;
```

SQLite does not support materialized views directly — you can simulate one with a regular table populated periodically.
