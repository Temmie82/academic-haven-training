# 18. Indexes & Query Performance

An **index** is a data structure that helps the database find rows quickly without scanning every row.

## When indexes help

- Columns used in `WHERE`
- Columns used in `JOIN ... ON`
- Columns used in `ORDER BY`
- Foreign key columns

## Trade-offs

| Pros | Cons |
|---|---|
| Faster reads / lookups | Slower `INSERT` / `UPDATE` / `DELETE` |
| Faster joins | Uses extra disk space |
| Helps `ORDER BY`/`GROUP BY` | Too many indexes can confuse the planner |

## Create an index

```sql
CREATE INDEX idx_orders_customer_id
    ON orders(customer_id);
```

## Composite (multi-column) index

Order matters — most-selective column first. Good for `WHERE customer_id = ? AND status = ?` queries.

```sql
CREATE INDEX idx_orders_customer_status
    ON orders(customer_id, status);
```

## Unique index

Like the `UNIQUE` constraint, but explicit:

```sql
CREATE UNIQUE INDEX idx_customers_email
    ON customers(email);
```

## Drop an index

```sql
DROP INDEX IF EXISTS idx_orders_customer_id;
```

## Inspect a query plan

```sql
EXPLAIN QUERY PLAN
SELECT *
FROM   orders
WHERE  customer_id = 2;
```

Look for `SEARCH ... USING INDEX` (good) vs `SCAN TABLE` (slow on large tables).

## General performance tips

1. Select only the columns you need.
2. Filter early with `WHERE`.
3. Index columns used in joins/filters.
4. Avoid functions on indexed columns in `WHERE`:
   - `WHERE LOWER(email) = 'a@b.com'` defeats the index. Store/lookup with consistent casing instead.
5. Beware `SELECT ... LIKE '%abc%'` — leading `%` cannot use a normal index.
6. Use `LIMIT` for pagination so the database can stop early.
