# 5. SELECT Basics, Aliases & DISTINCT

`SELECT` reads data from one or more tables.

## Basic syntax

```sql
SELECT column1, column2
FROM   table_name;
```

To return **every** column, use `*`:

```sql
SELECT *
FROM   customers;
```

> Avoid `SELECT *` in production code — naming columns makes queries clearer and faster.

## A few specific columns

```sql
SELECT first_name, last_name, city
FROM   customers;
```

## Column aliases (`AS`)

Aliases give a column a friendlier name in the result set.

```sql
SELECT first_name  AS first,
       last_name   AS last,
       signup_date AS joined_on
FROM   customers;
```

The `AS` keyword is optional in most databases:

```sql
SELECT first_name first, last_name last
FROM   customers;
```

If the alias contains spaces, wrap it in double quotes:

```sql
SELECT first_name AS "First Name"
FROM   customers;
```

## Table aliases

Useful when joining multiple tables.

```sql
SELECT c.first_name, c.last_name
FROM   customers AS c;
```

## DISTINCT — remove duplicates

```sql
SELECT DISTINCT state
FROM   customers;
```

`DISTINCT` applies to **the whole row** the SELECT returns:

```sql
-- Unique (state, city) combinations
SELECT DISTINCT state, city
FROM   customers;
```

## SELECT a literal value

You don't even need a table:

```sql
SELECT 'Hello, SQL!' AS greeting,
       1 + 1         AS sum;
```
