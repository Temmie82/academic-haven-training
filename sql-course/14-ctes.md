# 14. Common Table Expressions (CTEs)

A **CTE** is a temporary, named result set defined with the `WITH` keyword. It makes long queries easier to read and lets you re-use a result inside the same statement.

## Basic CTE

```sql
WITH customer_totals AS (
    SELECT o.customer_id,
           SUM(oi.quantity * oi.unit_price) AS total_spent
    FROM   orders      o
    JOIN   order_items oi ON o.order_id = oi.order_id
    GROUP BY o.customer_id
)
SELECT c.first_name,
       c.last_name,
       ROUND(ct.total_spent, 2) AS total_spent
FROM   customer_totals ct
JOIN   customers c ON ct.customer_id = c.customer_id
ORDER BY total_spent DESC;
```

## Multiple CTEs

Separate them with commas. Later CTEs can refer to earlier ones.

```sql
WITH order_totals AS (
    SELECT o.order_id, o.customer_id,
           SUM(oi.quantity * oi.unit_price) AS order_total
    FROM   orders o
    JOIN   order_items oi ON o.order_id = oi.order_id
    GROUP BY o.order_id, o.customer_id
),
customer_summary AS (
    SELECT customer_id,
           COUNT(*)         AS num_orders,
           SUM(order_total) AS revenue
    FROM   order_totals
    GROUP BY customer_id
)
SELECT c.first_name || ' ' || c.last_name AS customer_name,
       cs.num_orders,
       ROUND(cs.revenue, 2)               AS revenue
FROM   customer_summary cs
JOIN   customers c ON cs.customer_id = c.customer_id
ORDER BY revenue DESC;
```

## CTE vs subquery

A CTE is essentially a named subquery. Use a CTE when:

- the result is used more than once
- the logic is complex and benefits from a name
- you want a recursive query (see below)

## Recursive CTE

Recursive CTEs walk hierarchies (org charts, folder trees, graphs) or generate sequences.

### Generate numbers 1–10

```sql
WITH RECURSIVE numbers(n) AS (
    SELECT 1
    UNION ALL
    SELECT n + 1
    FROM   numbers
    WHERE  n < 10
)
SELECT n FROM numbers;
```

### Walk an employee hierarchy

```sql
WITH RECURSIVE org_chart AS (
    SELECT employee_id, full_name, manager_id, 1 AS level
    FROM   employees
    WHERE  manager_id IS NULL          -- top of the tree
    UNION ALL
    SELECT e.employee_id, e.full_name, e.manager_id, o.level + 1
    FROM   employees e
    JOIN   org_chart o ON e.manager_id = o.employee_id
)
SELECT * FROM org_chart ORDER BY level, full_name;
```
