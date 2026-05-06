# 8. Calculated Columns & Operators

You can compute values directly inside `SELECT`.

## Arithmetic operators

| Operator | Meaning |
|---|---|
| `+` | addition |
| `-` | subtraction |
| `*` | multiplication |
| `/` | division |
| `%` | modulo (remainder) |

```sql
SELECT product_name,
       price,
       price * 0.10 AS tax_estimate,
       price * 1.10 AS price_with_tax
FROM   products;
```

## String concatenation

In standard SQL and SQLite/PostgreSQL: use `||`.

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM   customers;
```

In MySQL: use `CONCAT()`.

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM   customers;
```

## Rounding

```sql
SELECT product_name,
       ROUND(price * 1.075, 2) AS price_with_sales_tax
FROM   products;
```

## A real-world calculated column

Total value of inventory per product:

```sql
SELECT product_name,
       price,
       stock_quantity,
       ROUND(price * stock_quantity, 2) AS stock_value
FROM   products
ORDER BY stock_value DESC;
```
