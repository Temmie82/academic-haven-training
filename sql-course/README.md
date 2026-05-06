# SQL — A Complete Beginner-to-Intermediate Course

A merged, reorganized version of the original notebooks (`sql basic.ipynb`, `sql_for_beginners_comprehensive.ipynb`, `keys.ipynb`, `joins.ipynb`, `joins_.ipynb`) rewritten as Markdown lessons with **pure SQL** code snippets.

All examples target **SQLite** syntax but work on most relational databases (PostgreSQL, MySQL, SQL Server) with minor changes.

## Lesson order

1. [Introduction to SQL & Relational Databases](01-introduction.md)
2. [DDL — Creating Tables, Keys & Constraints](02-ddl-tables-keys-constraints.md)
3. [Sample Database (used in all later lessons)](03-sample-database.md)
4. [DML — INSERT, UPDATE, DELETE](04-dml-insert-update-delete.md)
5. [SELECT Basics, Aliases & DISTINCT](05-select-basics.md)
6. [Filtering Rows with WHERE](06-filtering-where.md)
7. [Sorting & Limiting (ORDER BY, LIMIT, OFFSET)](07-sorting-limiting.md)
8. [Calculated Columns & Operators](08-calculated-columns.md)
9. [Aggregate Functions](09-aggregates.md)
10. [GROUP BY and HAVING](10-group-by-having.md)
11. [Joins — INNER, LEFT, RIGHT, FULL, SELF, CROSS](11-joins.md)
12. [Set Operators — UNION, INTERSECT, EXCEPT](12-set-operators.md)
13. [Subqueries & Correlated Subqueries](13-subqueries.md)
14. [Common Table Expressions (CTEs) & Recursive CTEs](14-ctes.md)
15. [Conditional Logic & NULL Handling (CASE, COALESCE, NULLIF)](15-case-null-handling.md)
16. [String, Numeric & Date Functions](16-string-date-functions.md)
17. [Views](17-views.md)
18. [Indexes & Query Performance](18-indexes-performance.md)
19. [Transactions](19-transactions.md)
20. [Window Functions](20-window-functions.md)
21. [SQL Execution Order](21-execution-order.md)
22. [Practice Exercises (with Solutions)](22-practice-exercises.md)
23. [Final Exam — The Bookstore Challenge](23-final-exam.md)

## How to use this course

- Read each lesson in order.
- Copy-paste the SQL into any SQLite client (DB Browser for SQLite, `sqlite3` CLI, DBeaver, etc.).
- After running the setup script in [Lesson 3](03-sample-database.md), every later query will work against the same dataset.
- Try the practice exercises before peeking at solutions.
