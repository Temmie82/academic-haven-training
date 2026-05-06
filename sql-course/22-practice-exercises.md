# 22. Practice Exercises

These exercises use the database from [Lesson 3](03-sample-database.md).
Try each one before checking the solution.

---

### 1. Show all products in the `Electronics` category.

<details><summary>Solution</summary>

```sql
SELECT *
FROM   products
WHERE  category = 'Electronics';
```
</details>

---

### 2. Show customers from Maryland (`MD`) ordered by last name.

<details><summary>Solution</summary>

```sql
SELECT *
FROM   customers
WHERE  state = 'MD'
ORDER BY last_name;
```
</details>

---

### 3. Count how many customers live in each state.

<details><summary>Solution</summary>

```sql
SELECT state,
       COUNT(*) AS customer_count
FROM   customers
GROUP BY state;
```
</details>

---

### 4. Show each order with the customer's full name.

<details><summary>Solution</summary>

```sql
SELECT o.order_id,
       o.order_date,
       c.first_name || ' ' || c.last_name AS customer_name
FROM   orders o
JOIN   customers c ON o.customer_id = c.customer_id
ORDER BY o.order_id;
```
</details>

---

### 5. Find the total revenue for each order.

<details><summary>Solution</summary>

```sql
SELECT order_id,
       ROUND(SUM(quantity * unit_price), 2) AS order_total
FROM   order_items
GROUP BY order_id
ORDER BY order_total DESC;
```
</details>

---

### 6. Find the average product price by category.

<details><summary>Solution</summary>

```sql
SELECT category,
       AVG(price) AS avg_price
FROM   products
GROUP BY category;
```
</details>

---

### 7. Show only categories whose average price is greater than 100.

<details><summary>Solution</summary>

```sql
SELECT category,
       AVG(price) AS avg_price
FROM   products
GROUP BY category
HAVING AVG(price) > 100;
```
</details>

---

### 8. Find customers who have never placed an order.

<details><summary>Solution</summary>

```sql
SELECT c.customer_id, c.first_name, c.last_name
FROM   customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE  o.order_id IS NULL;
```
</details>

---

### 9. Show the most expensive product in each category.

<details><summary>Solution (correlated subquery)</summary>

```sql
SELECT p1.category, p1.product_name, p1.price
FROM   products p1
WHERE  p1.price = (
    SELECT MAX(p2.price)
    FROM   products p2
    WHERE  p2.category = p1.category
)
ORDER BY p1.category;
```
</details>

<details><summary>Solution (window function)</summary>

```sql
WITH ranked AS (
    SELECT category, product_name, price,
           ROW_NUMBER() OVER (
               PARTITION BY category
               ORDER BY     price DESC
           ) AS rn
    FROM   products
)
SELECT category, product_name, price
FROM   ranked
WHERE  rn = 1;
```
</details>

---

### 10. Rank each customer's orders from highest total to lowest.

<details><summary>Solution</summary>

```sql
WITH order_totals AS (
    SELECT o.order_id,
           o.customer_id,
           SUM(oi.quantity * oi.unit_price) AS order_total
    FROM   orders o
    JOIN   order_items oi ON o.order_id = oi.order_id
    GROUP BY o.order_id, o.customer_id
)
SELECT customer_id,
       order_id,
       order_total,
       RANK() OVER (
           PARTITION BY customer_id
           ORDER BY     order_total DESC
       ) AS order_rank
FROM   order_totals
ORDER BY customer_id, order_rank;
```
</details>

---

### 11. Which city has the most customers?

<details><summary>Solution</summary>

```sql
SELECT city, COUNT(*) AS num_customers
FROM   customers
GROUP BY city
ORDER BY num_customers DESC
LIMIT 1;
```
</details>

---

### 12. Show each product with its share of total inventory value.

<details><summary>Solution</summary>

```sql
SELECT product_name,
       price * stock_quantity                                   AS stock_value,
       ROUND(
           100.0 * (price * stock_quantity)
           / SUM(price * stock_quantity) OVER (), 2
       )                                                        AS pct_of_total
FROM   products
ORDER BY stock_value DESC;
```
</details>

---

### 13. Find the second most expensive product.

<details><summary>Solution</summary>

```sql
SELECT product_name, price
FROM   products
ORDER BY price DESC
LIMIT 1 OFFSET 1;
```
</details>

---

### 14. Customers who have BOTH a shipped order AND a pending order.

<details><summary>Solution</summary>

```sql
SELECT customer_id FROM orders WHERE status = 'Shipped'
INTERSECT
SELECT customer_id FROM orders WHERE status = 'Pending';
```
</details>

---

### 15. Monthly revenue trend.

<details><summary>Solution</summary>

```sql
SELECT STRFTIME('%Y-%m', o.order_date)        AS month,
       ROUND(SUM(oi.quantity * oi.unit_price), 2) AS revenue
FROM   orders o
JOIN   order_items oi ON o.order_id = oi.order_id
GROUP BY month
ORDER BY month;
```
</details>
