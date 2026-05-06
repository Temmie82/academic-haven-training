# 21. SQL Execution Order

You **write** a SQL query in this order:

```
SELECT ... FROM ... JOIN ... ON ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT ...
```

But the database **executes** it in a different logical order:

| Step | Clause | Purpose |
|---:|---|---|
| 1 | `FROM`     | Pick the source tables |
| 2 | `JOIN`     | Combine them |
| 3 | `WHERE`    | Filter rows |
| 4 | `GROUP BY` | Group remaining rows |
| 5 | `HAVING`   | Filter groups |
| 6 | `SELECT`   | Choose / compute columns |
| 7 | `DISTINCT` | Remove duplicates |
| 8 | `ORDER BY` | Sort results |
| 9 | `LIMIT`    | Truncate to top N |

## Why it matters

### 1. You can't use a SELECT alias in WHERE

```sql
-- ERROR in most databases
SELECT price * 1.10 AS price_with_tax
FROM   products
WHERE  price_with_tax > 100;

-- Correct: repeat the expression, or use a subquery / CTE
SELECT price * 1.10 AS price_with_tax
FROM   products
WHERE  price * 1.10 > 100;
```

`WHERE` runs **before** `SELECT`, so the alias doesn't exist yet.

### 2. You CAN use a SELECT alias in ORDER BY

```sql
SELECT product_name, price * 1.10 AS price_with_tax
FROM   products
ORDER BY price_with_tax DESC;     -- works, ORDER BY runs after SELECT
```

### 3. WHERE filters rows; HAVING filters groups

```sql
SELECT category, AVG(price) AS avg_price
FROM   products
WHERE  stock_quantity > 0       -- before GROUP BY
GROUP BY category
HAVING AVG(price) > 100;        -- after GROUP BY
```

Keep this mental model and most "why doesn't this work?" puzzles disappear.
