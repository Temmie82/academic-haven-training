# 15. Conditional Logic & NULL Handling

## CASE — if/else inside SQL

### Searched CASE

```sql
SELECT product_name,
       price,
       CASE
           WHEN price >= 500 THEN 'Premium'
           WHEN price >= 100 THEN 'Mid-range'
           ELSE                    'Budget'
       END AS price_band
FROM   products
ORDER BY price DESC;
```

### Simple CASE (compares one value)

```sql
SELECT order_id,
       status,
       CASE status
           WHEN 'Shipped'   THEN 'Done'
           WHEN 'Pending'   THEN 'In progress'
           WHEN 'Cancelled' THEN 'Closed'
       END AS friendly_status
FROM   orders;
```

### CASE inside an aggregate

Count shipped vs pending orders in one query:

```sql
SELECT
    SUM(CASE WHEN status = 'Shipped'   THEN 1 ELSE 0 END) AS shipped_orders,
    SUM(CASE WHEN status = 'Pending'   THEN 1 ELSE 0 END) AS pending_orders,
    SUM(CASE WHEN status = 'Cancelled' THEN 1 ELSE 0 END) AS cancelled_orders
FROM orders;
```

## COALESCE — first non-NULL value

```sql
SELECT customer_id,
       first_name,
       COALESCE(city, 'Unknown') AS city
FROM   customers;
```

You can chain as many fallbacks as you like:

```sql
SELECT COALESCE(nickname, first_name, 'Friend') AS display_name
FROM   customers;
```

## NULLIF — return NULL when two values are equal

Useful to avoid division by zero:

```sql
SELECT product_name,
       quantity_sold,
       revenue / NULLIF(quantity_sold, 0) AS avg_unit_price
FROM   sales_summary;
```

## IFNULL / ISNULL

Database-specific shortcut for `COALESCE` with two arguments.

```sql
-- SQLite & MySQL
SELECT IFNULL(city, 'Unknown') FROM customers;

-- SQL Server
SELECT ISNULL(city, 'Unknown') FROM customers;
```
