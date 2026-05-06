# 6. Filtering Rows with WHERE

`WHERE` keeps only the rows that match a condition.

## Comparison operators

| Operator | Meaning |
|---|---|
| `=` | equal to |
| `!=` or `<>` | not equal to |
| `>` | greater than |
| `<` | less than |
| `>=` | greater than or equal to |
| `<=` | less than or equal to |

```sql
SELECT *
FROM   products
WHERE  price > 100;
```

## Combine conditions: AND, OR, NOT

```sql
SELECT *
FROM   customers
WHERE  state = 'MD'
  AND  city  = 'Baltimore';
```

```sql
SELECT *
FROM   orders
WHERE  status = 'Pending'
   OR  status = 'Cancelled';
```

```sql
SELECT *
FROM   products
WHERE  NOT category = 'Electronics';
```

> Use parentheses with mixed `AND`/`OR` to avoid surprises:
>
> ```sql
> WHERE (status = 'Shipped' OR status = 'Pending') AND customer_id = 1
> ```

## BETWEEN — a range (inclusive)

```sql
SELECT product_name, price
FROM   products
WHERE  price BETWEEN 20 AND 300;
```

Equivalent to `price >= 20 AND price <= 300`.

## IN — match any value in a list

```sql
SELECT first_name, last_name, city
FROM   customers
WHERE  city IN ('Baltimore', 'Towson', 'Columbia');
```

`NOT IN` is the opposite:

```sql
SELECT *
FROM   products
WHERE  category NOT IN ('Furniture', 'Office Supplies');
```

## LIKE — pattern matching

Wildcards:

- `%` = any number of characters (including zero)
- `_` = exactly one character

```sql
-- Names starting with 'A'
SELECT * FROM customers WHERE first_name LIKE 'A%';

-- Names ending in 'a'
SELECT * FROM customers WHERE first_name LIKE '%a';

-- Names with 'desk' anywhere (case sensitivity depends on the DB)
SELECT * FROM products  WHERE product_name LIKE '%desk%';

-- Four-letter names
SELECT * FROM customers WHERE first_name LIKE '____';
```

For case-insensitive matching in PostgreSQL use `ILIKE`. SQLite's `LIKE` is case-insensitive for ASCII by default.

## NULL handling

`NULL` means *unknown*. You **cannot** compare it with `=`.

```sql
-- WRONG: returns no rows
SELECT * FROM customers WHERE city = NULL;

-- CORRECT
SELECT * FROM customers WHERE city IS NULL;
SELECT * FROM customers WHERE city IS NOT NULL;
```
