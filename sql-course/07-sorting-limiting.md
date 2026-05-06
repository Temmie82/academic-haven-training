# 7. Sorting & Limiting

## ORDER BY

Sort the result set ascending (`ASC`, default) or descending (`DESC`).

```sql
SELECT product_name, price
FROM   products
ORDER BY price ASC;
```

```sql
SELECT product_name, price
FROM   products
ORDER BY price DESC;
```

### Sort by multiple columns

```sql
SELECT state, city, first_name, last_name
FROM   customers
ORDER BY state ASC, city ASC, last_name ASC;
```

### Sort by a column position or alias

```sql
SELECT product_name, price * 1.10 AS price_with_tax
FROM   products
ORDER BY price_with_tax DESC;     -- by alias

SELECT product_name, price
FROM   products
ORDER BY 2 DESC;                  -- by 2nd column
```

### NULLs in sorting

In PostgreSQL/Oracle you can control `NULL` placement:

```sql
SELECT * FROM customers ORDER BY city ASC NULLS LAST;
```

In SQLite, `NULL`s sort first when ascending.

## LIMIT — top N rows

```sql
SELECT *
FROM   products
ORDER BY price DESC
LIMIT 3;
```

## OFFSET — skip rows (pagination)

```sql
-- Page 2 of 5 results per page
SELECT *
FROM   products
ORDER BY product_id
LIMIT 5 OFFSET 5;
```

In SQL Server, the equivalent is:

```sql
SELECT * FROM products
ORDER BY product_id
OFFSET 5 ROWS FETCH NEXT 5 ROWS ONLY;
```
