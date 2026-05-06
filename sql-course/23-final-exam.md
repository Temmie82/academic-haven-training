# 23. Final Exam — The Bookstore Challenge

This exam uses a small, separate bookstore schema. Run the setup, then try each challenge.

## Setup

```sql
DROP TABLE IF EXISTS books;
DROP TABLE IF EXISTS authors;

CREATE TABLE authors (
    a_id INTEGER PRIMARY KEY,
    name TEXT
);

CREATE TABLE books (
    b_id  INTEGER PRIMARY KEY,
    a_id  INTEGER,
    title TEXT,
    price REAL,
    stock INTEGER,
    FOREIGN KEY (a_id) REFERENCES authors(a_id)
);

INSERT INTO authors VALUES
    (1, 'J.K. Rowling'),
    (2, 'George Orwell'),
    (3, 'Aldous Huxley'),
    (4, 'New Author');

INSERT INTO books VALUES
    (1, 1,    'Harry Potter', 25.00, 10),
    (2, 2,    '1984',         15.00,  5),
    (3, 2,    'Animal Farm',  12.00,  0),
    (4, NULL, 'Mystery Book', 10.00,  2);
```

---

## Challenge 1 — The Inventory Report

Show the **author name** and **book title** for every book currently in stock (`stock > 0`).

<details><summary>Solution</summary>

```sql
SELECT a.name, b.title
FROM   books   b
JOIN   authors a ON b.a_id = a.a_id
WHERE  b.stock > 0;
```
</details>

---

## Challenge 2 — The "Lost" Authors

Use a `LEFT JOIN` to find authors who have **no books** listed.

<details><summary>Solution</summary>

```sql
SELECT a.name
FROM   authors a
LEFT JOIN books b ON a.a_id = b.a_id
WHERE  b.b_id IS NULL;
```
</details>

---

## Challenge 3 — Financial Summary

Calculate the **total value** of the entire inventory (`price * stock`) across all books.

<details><summary>Solution</summary>

```sql
SELECT SUM(price * stock) AS total_inventory_value
FROM   books;
```
</details>

---

## Challenge 4 — Books with No Author

List every book whose author is unknown.

<details><summary>Solution</summary>

```sql
SELECT title, price, stock
FROM   books
WHERE  a_id IS NULL;
```
</details>

---

## Challenge 5 — Author Productivity

Show each author and the **number of books** they have written, including authors with zero books. Order by book count descending.

<details><summary>Solution</summary>

```sql
SELECT a.name,
       COUNT(b.b_id) AS book_count
FROM   authors a
LEFT JOIN books b ON a.a_id = b.a_id
GROUP BY a.a_id, a.name
ORDER BY book_count DESC;
```
</details>

---

## Challenge 6 — Price Bands

Classify each book as `Cheap` (< 12), `Standard` (12–20), or `Premium` (> 20).

<details><summary>Solution</summary>

```sql
SELECT title,
       price,
       CASE
           WHEN price < 12  THEN 'Cheap'
           WHEN price <= 20 THEN 'Standard'
           ELSE                  'Premium'
       END AS price_band
FROM   books
ORDER BY price;
```
</details>

---

## Challenge 7 — Top-Earning Author

Which author has the highest **total inventory value** (`price * stock`)?

<details><summary>Solution</summary>

```sql
SELECT a.name,
       SUM(b.price * b.stock) AS inventory_value
FROM   authors a
JOIN   books   b ON a.a_id = b.a_id
GROUP BY a.a_id, a.name
ORDER BY inventory_value DESC
LIMIT 1;
```
</details>

---

## 🎓 Congratulations

You have worked through:

- DDL, DML, and DQL
- filtering, sorting, grouping
- joins (inner, left, right, full, self, cross)
- set operators
- subqueries and CTEs (including recursive)
- window functions
- views, indexes, transactions
- string, numeric, and date functions

Next steps:

- Practice on a real dataset (e.g. the [Chinook](https://github.com/lerocha/chinook-database) or [Sakila](https://dev.mysql.com/doc/sakila/en/) sample DBs).
- Learn the dialect-specific extras for whichever database you use most (PostgreSQL, MySQL, SQL Server, Oracle).
- Pair SQL with a BI tool (Metabase, Tableau, Power BI) or a programming language (Python's `pandas`, R's `dplyr`).
