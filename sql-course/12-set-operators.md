# 12. Set Operators — UNION, INTERSECT, EXCEPT

Set operators stack the results of two queries on top of each other. The two queries must return the **same number of columns** with **compatible types**.

## UNION — combine and remove duplicates

```sql
SELECT city FROM customers
UNION
SELECT 'Bethesda';
```

## UNION ALL — combine and keep duplicates (faster)

```sql
SELECT first_name FROM customers WHERE state = 'MD'
UNION ALL
SELECT first_name FROM customers WHERE state = 'VA';
```

## INTERSECT — rows in both queries

```sql
SELECT customer_id FROM orders WHERE status = 'Shipped'
INTERSECT
SELECT customer_id FROM orders WHERE status = 'Pending';
```
*Customers who have both a shipped and a pending order.*

## EXCEPT (or `MINUS` in Oracle) — in the first query but not the second

```sql
SELECT customer_id FROM customers
EXCEPT
SELECT customer_id FROM orders;
```
*Customers who have never placed an order.*

## Tips

- Column names of the result come from the **first** query.
- Use `ORDER BY` only at the very end of the combined query.

```sql
SELECT first_name, 'MD' AS region FROM customers WHERE state = 'MD'
UNION ALL
SELECT first_name, 'VA'           FROM customers WHERE state = 'VA'
ORDER BY region, first_name;
```
