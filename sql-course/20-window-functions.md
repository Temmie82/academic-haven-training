# 20. Window Functions

Window functions perform calculations across a set of rows **related to the current row**, without collapsing them into one row per group (the way `GROUP BY` does).

## Anatomy

```sql
function() OVER (
    PARTITION BY column   -- optional: split rows into groups
    ORDER BY     column   -- optional: ordering within the group
    ROWS BETWEEN ...      -- optional: frame
)
```

## Ranking functions

| Function | Behaviour |
|---|---|
| `ROW_NUMBER()` | 1, 2, 3, 4, ... (no ties) |
| `RANK()` | 1, 2, 2, 4, ... (ties skip) |
| `DENSE_RANK()` | 1, 2, 2, 3, ... (no skips) |
| `NTILE(n)` | Splits into `n` buckets |

```sql
SELECT o.order_id,
       o.customer_id,
       SUM(oi.quantity * oi.unit_price)        AS order_total,
       RANK() OVER (
           PARTITION BY o.customer_id
           ORDER BY     SUM(oi.quantity * oi.unit_price) DESC
       )                                        AS order_rank_for_customer
FROM   orders o
JOIN   order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id, o.customer_id
ORDER BY o.customer_id, order_rank_for_customer;
```

## Aggregate functions as windows

`SUM`, `AVG`, `COUNT`, `MIN`, `MAX` can all be windows.

### Running total per customer

```sql
SELECT o.customer_id,
       o.order_id,
       o.order_date,
       SUM(oi.quantity * oi.unit_price) AS order_total,
       SUM(SUM(oi.quantity * oi.unit_price)) OVER (
           PARTITION BY o.customer_id
           ORDER BY     o.order_date
       ) AS running_total
FROM   orders o
JOIN   order_items oi ON o.order_id = oi.order_id
GROUP BY o.customer_id, o.order_id, o.order_date
ORDER BY o.customer_id, o.order_date;
```

### Each row vs the customer's average

```sql
SELECT order_id, customer_id, order_total,
       AVG(order_total) OVER (PARTITION BY customer_id) AS customer_avg
FROM   order_totals;
```

## LAG and LEAD — peek at neighboring rows

```sql
SELECT order_id,
       order_date,
       customer_id,
       LAG(order_date)  OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_order,
       LEAD(order_date) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_order
FROM   orders;
```

## FIRST_VALUE and LAST_VALUE

```sql
SELECT product_name,
       category,
       price,
       FIRST_VALUE(product_name) OVER (
           PARTITION BY category ORDER BY price DESC
       ) AS most_expensive_in_category
FROM   products;
```

## Frames — controlling the window

```sql
-- Moving 3-row average
SELECT order_date,
       order_total,
       AVG(order_total) OVER (
           ORDER BY order_date
           ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) AS moving_avg_3
FROM   order_totals;
```

## Top N per group (classic interview question)

The most expensive product in each category:

```sql
WITH ranked AS (
    SELECT product_name, category, price,
           ROW_NUMBER() OVER (
               PARTITION BY category
               ORDER BY     price DESC
           ) AS rn
    FROM   products
)
SELECT product_name, category, price
FROM   ranked
WHERE  rn = 1;
```
