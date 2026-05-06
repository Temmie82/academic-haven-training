# 10. GROUP BY and HAVING

`GROUP BY` splits rows into groups and applies aggregate functions to each group.

## GROUP BY

Count products per category:

```sql
SELECT category,
       COUNT(*)   AS product_count,
       AVG(price) AS avg_price
FROM   products
GROUP BY category;
```

Count orders per status:

```sql
SELECT status,
       COUNT(*) AS num_orders
FROM   orders
GROUP BY status;
```

## The Golden Rule

> Every column in `SELECT` must either be:
> - listed in `GROUP BY`, **or**
> - wrapped in an aggregate function.

## Group by multiple columns

```sql
SELECT state, city,
       COUNT(*) AS customers_here
FROM   customers
GROUP BY state, city
ORDER BY state, city;
```

## HAVING — filter groups

`WHERE` filters rows **before** grouping.
`HAVING` filters groups **after** aggregation.

```sql
SELECT category,
       COUNT(*)   AS product_count,
       AVG(price) AS avg_price
FROM   products
GROUP BY category
HAVING AVG(price) > 100;
```

## WHERE vs HAVING — both used together

Find categories where the average price of **in-stock** products is above 100:

```sql
SELECT category,
       AVG(price) AS avg_price
FROM   products
WHERE  stock_quantity > 0          -- row-level filter
GROUP BY category
HAVING AVG(price) > 100;           -- group-level filter
```
