# 1. Introduction to SQL & Relational Databases

## What is SQL?

**SQL** stands for **Structured Query Language**. It is the standard language for talking to **relational databases**.

With SQL you can:

- read data
- filter, sort, and summarize data
- combine data from multiple tables
- add, change, or remove records
- create and modify the structure of the database itself

## What is a relational database?

A relational database stores data in **tables**:

- each **row** is one record (e.g. one customer)
- each **column** is one attribute (e.g. `first_name`, `email`)
- tables are linked together by **keys**

A typical e-commerce database might have:

- `customers`
- `products`
- `orders`
- `order_items`

## The categories of SQL commands

| Category | Stands for | Examples |
|---|---|---|
| **DDL** | Data Definition Language | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** | Data Manipulation Language | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Data Query Language | `SELECT` |
| **DCL** | Data Control Language | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | `BEGIN`, `COMMIT`, `ROLLBACK` |

## A first look at SQL

```sql
SELECT first_name, last_name
FROM   customers
WHERE  state = 'MD'
ORDER BY last_name;
```

Read it as: *"Select the first and last names from the customers table where the state is MD, ordered by last name."*

> SQL is **declarative** — you describe **what** you want, not **how** to get it. The database engine figures out the most efficient plan.
