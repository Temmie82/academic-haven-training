# 2. DDL — Creating Tables, Keys & Constraints

**DDL (Data Definition Language)** defines the structure of the database.

## CREATE TABLE

```sql
CREATE TABLE departments (
    dept_id   INTEGER PRIMARY KEY,
    dept_name TEXT NOT NULL,
    location  TEXT
);
```

## Common data types

| Type | Description |
|---|---|
| `INTEGER` | Whole numbers |
| `REAL` / `NUMERIC` / `DECIMAL` | Numbers with decimals |
| `TEXT` / `VARCHAR(n)` | Strings |
| `DATE` / `DATETIME` / `TIMESTAMP` | Dates and times |
| `BOOLEAN` | True/false (stored as 0/1 in SQLite) |
| `BLOB` | Binary data |

## Constraints

Constraints protect data quality.

| Constraint | Purpose |
|---|---|
| `PRIMARY KEY` | Uniquely identifies each row; cannot be NULL |
| `FOREIGN KEY` | Links to a primary key in another table |
| `NOT NULL` | Column cannot be empty |
| `UNIQUE` | All values in the column must be different |
| `CHECK` | Custom condition each row must satisfy |
| `DEFAULT` | Value used when none is provided |

### Example with all common constraints

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    full_name   TEXT    NOT NULL,
    email       TEXT    UNIQUE,
    department  TEXT    NOT NULL,
    salary      REAL    CHECK (salary >= 0),
    active      INTEGER DEFAULT 1,
    dept_id     INTEGER,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

## Keys explained

### Primary Key
Uniquely identifies each row in a table. A table has **at most one** primary key.

```sql
CREATE TABLE products (
    product_id   INTEGER PRIMARY KEY,
    product_name TEXT    NOT NULL,
    price        REAL    NOT NULL
);
```

### Composite Primary Key
A primary key made of more than one column.

```sql
CREATE TABLE order_items (
    order_id    INTEGER,
    product_id  INTEGER,
    quantity    INTEGER NOT NULL,
    PRIMARY KEY (order_id, product_id)
);
```

### Foreign Key
Enforces a link to another table's primary key.

```sql
CREATE TABLE sales (
    sale_id    INTEGER PRIMARY KEY,
    user_id    uuid,
    product_id INTEGER,
    FOREIGN KEY (user_id)    REFERENCES auth.users(id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

## ALTER TABLE

Modify an existing table.

```sql
-- Add a column
ALTER TABLE departments ADD COLUMN budget REAL DEFAULT 0;

-- Rename a column (SQLite 3.25+, PostgreSQL, etc.)
ALTER TABLE departments RENAME COLUMN location TO office_city;

-- Rename a table
ALTER TABLE departments RENAME TO company_departments;
```

## DROP TABLE

Permanently deletes the table **and all its data**.

```sql
DROP TABLE company_departments;
```

Use `IF EXISTS` to avoid an error if the table is missing:

```sql
DROP TABLE IF EXISTS company_departments;
```

## TRUNCATE TABLE

Removes all rows but keeps the table structure (faster than `DELETE` on large tables).

```sql
TRUNCATE TABLE employees;   -- not in SQLite; use DELETE FROM employees;
```
