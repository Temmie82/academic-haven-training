# 4. DML — INSERT, UPDATE, DELETE

**DML (Data Manipulation Language)** changes the rows inside tables.

## INSERT

Add new rows.

### Insert one row, all columns

```sql
INSERT INTO products
VALUES (9, 'Webcam', 'Electronics', 85.00, 40);
```

### Insert one row, specific columns

```sql
INSERT INTO products (product_id, product_name, category, price, stock_quantity)
VALUES (10, 'USB-C Hub', 'Electronics', 35.00, 50);
```

### Insert multiple rows at once

```sql
INSERT INTO products (product_id, product_name, category, price, stock_quantity) VALUES
(11, 'Headphones', 'Electronics', 90.00, 25),
(12, 'Stapler',    'Office Supplies', 8.00, 75);
```

### Insert from another query

```sql
INSERT INTO archived_customers (customer_id, first_name, last_name)
SELECT customer_id, first_name, last_name
FROM   customers
WHERE  signup_date < '2025-02-01';
```

## UPDATE

Change values in existing rows. **Always include `WHERE`** unless you really want to update every row.

```sql
UPDATE products
SET    stock_quantity = stock_quantity - 5
WHERE  product_id = 9;
```

### Update multiple columns

```sql
UPDATE customers
SET    city  = 'Bethesda',
       state = 'MD'
WHERE  customer_id = 7;
```

### Update with a calculation

```sql
UPDATE products
SET    price = price * 1.10        -- raise all prices by 10%
WHERE  category = 'Electronics';
```

## DELETE

Remove rows. **Always include `WHERE`** unless you want to empty the table.

```sql
DELETE FROM products
WHERE  product_id = 9;
```

### Delete every row (keeps the table)

```sql
DELETE FROM products;
```

## Safety tip: preview with SELECT first

Before running an `UPDATE` or `DELETE`, run the same `WHERE` clause as a `SELECT`:

```sql
-- Preview
SELECT * FROM products WHERE category = 'Furniture';

-- Then run the destructive statement
DELETE FROM products WHERE category = 'Furniture';
```
