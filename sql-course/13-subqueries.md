# 13. Subqueries

A **subquery** is a `SELECT` statement nested inside another query.

## In the WHERE clause (scalar)

Products priced above the average:

```sql
SELECT product_name, price
FROM   products
WHERE  price > (SELECT AVG(price) FROM products)
ORDER BY price DESC;
```

## With IN

Customers who have placed at least one order:

```sql
SELECT *
FROM   customers
WHERE  customer_id IN (SELECT DISTINCT customer_id FROM orders);
```

## With NOT IN

Customers who have **never** placed an order:

```sql
SELECT *
FROM   customers
WHERE  customer_id NOT IN (SELECT customer_id FROM orders);
```

> Be careful: if the subquery can return `NULL`, `NOT IN` may return zero rows. Prefer `NOT EXISTS` in that case.

## EXISTS / NOT EXISTS

Often faster than `IN` for large tables.

```sql
SELECT *
FROM   customers c
WHERE  EXISTS (
    SELECT 1
    FROM   orders o
    WHERE  o.customer_id = c.customer_id
);
```

## Correlated subquery

A subquery that references the outer query. Runs once per outer row.

The most expensive product in each category:

```sql
SELECT p1.category, p1.product_name, p1.price
FROM   products p1
WHERE  p1.price = (
    SELECT MAX(p2.price)
    FROM   products p2
    WHERE  p2.category = p1.category    -- references the outer table
)
ORDER BY p1.category;
```

## Subquery in the FROM clause (derived table)

```sql
SELECT category, AVG(line_total) AS avg_line
FROM (
    SELECT p.category,
           oi.quantity * oi.unit_price AS line_total
    FROM   order_items oi
    JOIN   products    p ON oi.product_id = p.product_id
) AS lines
GROUP BY category;
```

## Subquery in the SELECT clause

```sql
SELECT c.customer_id,
       c.first_name,
       (SELECT COUNT(*) FROM orders o WHERE o.customer_id = c.customer_id) AS order_count
FROM   customers c;
```
