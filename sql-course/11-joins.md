# 11. Joins

Joins combine rows from two or more tables based on a related column.

## Visualizing joins

| Join | Returns |
|---|---|
| `INNER JOIN` | Only rows with matches in **both** tables |
| `LEFT JOIN`  | All rows from the **left** table + matches from the right |
| `RIGHT JOIN` | All rows from the **right** table + matches from the left |
| `FULL OUTER JOIN` | All rows from **both** tables |
| `CROSS JOIN` | Cartesian product (every row × every row) |
| `SELF JOIN`  | A table joined to itself |

## INNER JOIN

```sql
SELECT o.order_id,
       o.order_date,
       o.status,
       c.first_name,
       c.last_name
FROM   orders    AS o
INNER JOIN customers AS c
       ON o.customer_id = c.customer_id
ORDER BY o.order_id;
```

`JOIN` alone is shorthand for `INNER JOIN`.

## Joining three or more tables

```sql
SELECT o.order_id,
       c.first_name || ' ' || c.last_name AS customer_name,
       p.product_name,
       oi.quantity,
       oi.unit_price,
       oi.quantity * oi.unit_price        AS line_total
FROM   order_items AS oi
JOIN   orders      AS o ON oi.order_id   = o.order_id
JOIN   customers   AS c ON o.customer_id = c.customer_id
JOIN   products    AS p ON oi.product_id = p.product_id
ORDER BY o.order_id, p.product_name;
```

## LEFT JOIN

Keeps every row from the left table, even when there is no matching row on the right. Unmatched right-side columns become `NULL`.

```sql
SELECT c.customer_id,
       c.first_name,
       c.last_name,
       o.order_id,
       o.order_date
FROM   customers AS c
LEFT JOIN orders AS o
       ON c.customer_id = o.customer_id
ORDER BY c.customer_id, o.order_id;
```

### Find rows with NO match (anti-join)

Customers who have never placed an order:

```sql
SELECT c.customer_id, c.first_name, c.last_name
FROM   customers AS c
LEFT JOIN orders AS o
       ON c.customer_id = o.customer_id
WHERE  o.order_id IS NULL;
```

## RIGHT JOIN

The mirror of `LEFT JOIN`. Not supported in SQLite; supported in PostgreSQL, MySQL, SQL Server.

```sql
SELECT c.first_name, o.order_id
FROM   orders AS o
RIGHT JOIN customers AS c
       ON o.customer_id = c.customer_id;
```

You can always rewrite a `RIGHT JOIN` as a `LEFT JOIN` by swapping the table order.

## FULL OUTER JOIN

Returns all rows from both sides; unmatched columns become `NULL`.

```sql
SELECT c.customer_id, o.order_id
FROM   customers AS c
FULL OUTER JOIN orders AS o
       ON c.customer_id = o.customer_id;
```

In SQLite (older versions), emulate with `UNION`:

```sql
SELECT c.customer_id, o.order_id
FROM   customers c LEFT JOIN orders o ON c.customer_id = o.customer_id
UNION
SELECT c.customer_id, o.order_id
FROM   customers c RIGHT JOIN orders o ON c.customer_id = o.customer_id;
```

## CROSS JOIN

Every combination of rows. Use carefully — result size = `n × m`.

```sql
SELECT c.first_name, p.product_name
FROM   customers c
CROSS JOIN products p;
```

## SELF JOIN

A table joined to itself. Useful for hierarchies (e.g. employee → manager).

```sql
-- Pair every customer with every other customer in the same city
SELECT a.first_name AS customer_a,
       b.first_name AS customer_b,
       a.city
FROM   customers a
JOIN   customers b
       ON a.city = b.city
      AND a.customer_id < b.customer_id;   -- avoid pairs and duplicates
```

## USING clause (shorthand)

When the join column has the **same name** in both tables:

```sql
SELECT order_id, customer_id
FROM   orders
JOIN   customers USING (customer_id);
```

## NATURAL JOIN

Joins automatically on every column with a matching name. Powerful but risky — avoid in production.

```sql
SELECT * FROM orders NATURAL JOIN customers;
```

## Combining joins, GROUP BY and aggregates

Total spent by each customer:

```sql
SELECT c.customer_id,
       c.first_name || ' ' || c.last_name        AS customer_name,
       ROUND(SUM(oi.quantity * oi.unit_price),2) AS total_spent
FROM   customers   c
JOIN   orders      o  ON c.customer_id = o.customer_id
JOIN   order_items oi ON o.order_id    = oi.order_id
GROUP BY c.customer_id, customer_name
ORDER BY total_spent DESC;
```
