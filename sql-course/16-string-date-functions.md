# 16. String, Numeric & Date Functions

## String functions

| Function | What it does |
|---|---|
| `LENGTH(s)` | Number of characters |
| `UPPER(s)` / `LOWER(s)` | Change case |
| `TRIM(s)` | Remove leading/trailing whitespace |
| `SUBSTR(s, start, len)` | Substring |
| `REPLACE(s, find, with)` | Replace text |
| `INSTR(s, find)` | Position of substring (1-based) |
| `s1 \|\| s2` | Concatenate (use `CONCAT()` in MySQL) |

```sql
SELECT first_name,
       last_name,
       first_name || ' ' || last_name AS full_name,
       UPPER(state)                   AS state_upper,
       LENGTH(email)                  AS email_length,
       SUBSTR(email, 1, INSTR(email, '@') - 1) AS email_user
FROM   customers;
```

## Numeric functions

| Function | What it does |
|---|---|
| `ROUND(n, d)` | Round to `d` decimal places |
| `ABS(n)` | Absolute value |
| `CEIL(n)` / `FLOOR(n)` | Round up / down |
| `MOD(a, b)` or `a % b` | Remainder |
| `POWER(a, b)` | a to the power of b |
| `SQRT(n)` | Square root |

```sql
SELECT product_name,
       price,
       ROUND(price * 1.075, 2) AS price_with_tax,
       FLOOR(price)            AS floor_price,
       CEIL(price)             AS ceil_price
FROM   products;
```

## Date & time functions (SQLite)

SQLite stores dates as text in `YYYY-MM-DD` format and provides:

```sql
SELECT
    DATE('now')                          AS today,
    DATETIME('now')                      AS now_ts,
    DATE('now', '+7 day')                AS next_week,
    DATE('now', 'start of month')        AS first_of_month,
    STRFTIME('%Y-%m', order_date)        AS order_month,
    STRFTIME('%w', order_date)           AS day_of_week
FROM orders;
```

### Group orders by month

```sql
SELECT STRFTIME('%Y-%m', order_date) AS order_month,
       COUNT(*)                      AS num_orders,
       SUM(quantity * unit_price)    AS revenue
FROM   orders o
JOIN   order_items oi ON o.order_id = oi.order_id
GROUP BY order_month
ORDER BY order_month;
```

### Equivalents in other databases

| Task | SQLite | PostgreSQL | MySQL | SQL Server |
|---|---|---|---|---|
| Today | `DATE('now')` | `CURRENT_DATE` | `CURDATE()` | `CAST(GETDATE() AS DATE)` |
| Add 7 days | `DATE(d, '+7 day')` | `d + INTERVAL '7 day'` | `DATE_ADD(d, INTERVAL 7 DAY)` | `DATEADD(day, 7, d)` |
| Year-month | `STRFTIME('%Y-%m', d)` | `TO_CHAR(d,'YYYY-MM')` | `DATE_FORMAT(d,'%Y-%m')` | `FORMAT(d,'yyyy-MM')` |
| Diff in days | `JULIANDAY(a) - JULIANDAY(b)` | `a - b` | `DATEDIFF(a, b)` | `DATEDIFF(day, b, a)` |

## CAST — convert types

```sql
SELECT '42' + 1                  AS implicit,
       CAST('42' AS INTEGER) + 1 AS explicit;
```
