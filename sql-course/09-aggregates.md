# 9. Aggregate Functions

Aggregate functions collapse many rows into a single value.

| Function | Description |
|---|---|
| `COUNT(*)` | Number of rows |
| `COUNT(col)` | Number of non-NULL values in `col` |
| `COUNT(DISTINCT col)` | Number of unique non-NULL values |
| `SUM(col)` | Total |
| `AVG(col)` | Average |
| `MIN(col)` | Smallest value |
| `MAX(col)` | Largest value |

## Simple aggregates

```sql
SELECT COUNT(*)   AS total_products,
       AVG(price) AS avg_price,
       MIN(price) AS cheapest,
       MAX(price) AS most_expensive
FROM   products;
```

## Computed totals

Total revenue across every line item:

```sql
SELECT SUM(quantity * unit_price) AS gross_sales
FROM   order_items;
```

## COUNT with DISTINCT

How many different states do our customers come from?

```sql
SELECT COUNT(DISTINCT state) AS unique_states
FROM   customers;
```

## Aggregates ignore NULL

`COUNT(column)` skips `NULL`s, but `COUNT(*)` counts every row.

```sql
SELECT COUNT(*)    AS all_rows,
       COUNT(city) AS rows_with_city
FROM   customers;
```
