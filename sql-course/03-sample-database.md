# 3. Sample Database

All later lessons use the same e-commerce schema. Run this script once before practicing the queries.

## Schema

```sql
DROP TABLE IF EXISTS order_items;
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS products;
DROP TABLE IF EXISTS customers;

CREATE TABLE customers (
    customer_id  INTEGER PRIMARY KEY,
    first_name   TEXT NOT NULL,
    last_name    TEXT NOT NULL,
    city         TEXT,
    state        TEXT,
    signup_date  TEXT,
    email        TEXT UNIQUE
);

CREATE TABLE products (
    product_id     INTEGER PRIMARY KEY,
    product_name   TEXT NOT NULL,
    category       TEXT NOT NULL,
    price          REAL NOT NULL,
    stock_quantity INTEGER NOT NULL
);

CREATE TABLE orders (
    order_id    INTEGER PRIMARY KEY,
    customer_id INTEGER NOT NULL,
    order_date  TEXT NOT NULL,
    status      TEXT NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_item_id INTEGER PRIMARY KEY,
    order_id      INTEGER NOT NULL,
    product_id    INTEGER NOT NULL,
    quantity      INTEGER NOT NULL,
    unit_price    REAL NOT NULL,
    FOREIGN KEY (order_id)   REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

## Seed data

```sql
INSERT INTO customers (customer_id, first_name, last_name, city, state, signup_date, email) VALUES
(1, 'Ava',    'Johnson', 'Baltimore',     'MD', '2025-01-10', 'ava@example.com'),
(2, 'Liam',   'Smith',   'Towson',        'MD', '2025-02-14', 'liam@example.com'),
(3, 'Noah',   'Brown',   'Columbia',      'MD', '2025-02-20', 'noah@example.com'),
(4, 'Emma',   'Davis',   'Arlington',     'VA', '2025-03-05', 'emma@example.com'),
(5, 'Olivia', 'Wilson',  'Silver Spring', 'MD', '2025-03-18', 'olivia@example.com'),
(6, 'Sophia', 'Miller',  'Alexandria',    'VA', '2025-04-01', 'sophia@example.com'),
(7, 'Mia',    'Taylor',  'Rockville',     'MD', '2025-05-01', 'mia@example.com');

INSERT INTO products (product_id, product_name, category, price, stock_quantity) VALUES
(1, 'Laptop',        'Electronics',     1200.00,  15),
(2, 'Mouse',         'Electronics',       25.00, 100),
(3, 'Keyboard',      'Electronics',       45.00,  60),
(4, 'Desk Chair',    'Furniture',        180.00,  20),
(5, 'Notebook',      'Office Supplies',    5.00, 200),
(6, 'Pen Set',       'Office Supplies',   12.00, 150),
(7, 'Monitor',       'Electronics',      250.00,  30),
(8, 'Standing Desk', 'Furniture',        400.00,  10);

INSERT INTO orders (order_id, customer_id, order_date, status) VALUES
(1, 1, '2025-04-10', 'Shipped'),
(2, 2, '2025-04-12', 'Pending'),
(3, 1, '2025-04-15', 'Shipped'),
(4, 3, '2025-04-18', 'Cancelled'),
(5, 4, '2025-04-19', 'Shipped'),
(6, 5, '2025-04-22', 'Pending'),
(7, 2, '2025-04-25', 'Shipped'),
(8, 6, '2025-04-27', 'Shipped');

INSERT INTO order_items (order_item_id, order_id, product_id, quantity, unit_price) VALUES
( 1, 1, 1,  1, 1200.00),
( 2, 1, 2,  2,   25.00),
( 3, 2, 5, 10,    5.00),
( 4, 2, 6,  3,   12.00),
( 5, 3, 7,  2,  250.00),
( 6, 3, 3,  1,   45.00),
( 7, 4, 4,  1,  180.00),
( 8, 5, 8,  1,  400.00),
( 9, 5, 2,  1,   25.00),
(10, 6, 5, 20,    5.00),
(11, 7, 1,  1, 1150.00),
(12, 7, 7,  1,  250.00),
(13, 8, 4,  1,  180.00),
(14, 8, 5,  5,    5.00);
```

## Confirm it loaded

```sql
SELECT name FROM sqlite_master WHERE type = 'table' ORDER BY name;
```

> Customer **#7 (Mia Taylor)** has no orders. This makes the `LEFT JOIN` lessons more interesting.
