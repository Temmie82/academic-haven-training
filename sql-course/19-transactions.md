# 19. Transactions

A **transaction** groups multiple SQL statements so they either **all succeed** or **all fail**. This keeps related changes consistent.

## ACID properties

| Letter | Meaning |
|---|---|
| **A**tomicity | All-or-nothing |
| **C**onsistency | Database moves from one valid state to another |
| **I**solation | Concurrent transactions don't interfere |
| **D**urability | Once committed, changes survive crashes |

## Basic transaction

```sql
BEGIN TRANSACTION;

UPDATE products
SET    stock_quantity = stock_quantity - 1
WHERE  product_id = 1;

UPDATE products
SET    stock_quantity = stock_quantity + 1
WHERE  product_id = 2;

COMMIT;
```

If anything goes wrong, undo the changes:

```sql
BEGIN TRANSACTION;

UPDATE products SET price = price * 1.10;

-- Oops, that updated every row by mistake
ROLLBACK;
```

## Savepoints — partial rollbacks

```sql
BEGIN TRANSACTION;

UPDATE customers SET city = 'Bethesda' WHERE customer_id = 7;

SAVEPOINT before_price_change;

UPDATE products SET price = price * 2;   -- big mistake

ROLLBACK TO SAVEPOINT before_price_change;   -- undo only the price change

COMMIT;   -- still commits the customer update
```

## Real-world example: money transfer

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

-- If both succeed, COMMIT. If either fails, ROLLBACK.
COMMIT;
```

## Isolation levels (concept)

Different databases offer levels like `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`. Higher isolation = more correct, less concurrent. SQLite uses `SERIALIZABLE` by default.
