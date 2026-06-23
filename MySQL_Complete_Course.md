# 🗄️ MySQL — Complete Course
**Database:** MySQL | **Level:** Beginner → SDE Interview Ready
**Based on:** LeetCode SQL 50 + Top Interview Questions
**Style:** Concept → Syntax → Examples → Solved Problems → Practice Problems

---

# 📌 Contents

**Phase 1 — MySQL Foundations**
- 1.1 What is SQL? How MySQL Works
- 1.2 Data Types in MySQL
- 1.3 The Anatomy of a SQL Query (Execution Order)
- 1.4 Full MySQL Syntax Reference Sheet

**Phase 2 — SELECT, Filter & Sort**
- 2.1 SELECT Basics
- 2.2 WHERE Clause — All Operators
- 2.3 NULL Handling
- 2.4 ORDER BY, LIMIT, OFFSET
- 2.5 DISTINCT
- Solved LC Problems
- Practice Problems

**Phase 3 — Aggregation & Grouping**
- 3.1 Aggregate Functions (COUNT, SUM, AVG, MAX, MIN)
- 3.2 GROUP BY
- 3.3 HAVING vs WHERE
- 3.4 Combining Aggregates
- Solved LC Problems
- Practice Problems

**Phase 4 — JOINs**
- 4.1 INNER JOIN
- 4.2 LEFT JOIN (LEFT OUTER JOIN)
- 4.3 RIGHT JOIN
- 4.4 FULL OUTER JOIN (workaround in MySQL)
- 4.5 SELF JOIN
- 4.6 CROSS JOIN
- 4.7 Multiple JOINs
- Solved LC Problems
- Practice Problems

**Phase 5 — Subqueries**
- 5.1 Scalar Subquery
- 5.2 Subquery in WHERE (IN, NOT IN, EXISTS)
- 5.3 Correlated Subquery
- 5.4 Subquery in FROM (Derived Table)
- 5.5 Subquery vs JOIN
- Solved LC Problems
- Practice Problems

**Phase 6 — String, Date & Math Functions**
- 6.1 String Functions
- 6.2 Date & Time Functions
- 6.3 Math Functions
- 6.4 CASE WHEN Expression
- 6.5 IFNULL, COALESCE, IF
- Solved LC Problems
- Practice Problems

**Phase 7 — Window Functions**
- 7.1 What are Window Functions?
- 7.2 ROW_NUMBER, RANK, DENSE_RANK
- 7.3 LAG and LEAD
- 7.4 SUM, AVG OVER (Running Totals)
- 7.5 PARTITION BY vs GROUP BY
- 7.6 FIRST_VALUE, LAST_VALUE, NTH_VALUE
- Solved LC Problems
- Practice Problems

**Phase 8 — Advanced Patterns**
- 8.1 UNION and UNION ALL
- 8.2 CTE (Common Table Expressions — WITH clause)
- 8.3 Finding Duplicates
- 8.4 Finding Missing Values / Gaps
- 8.5 Consecutive Rows Pattern
- 8.6 Pivot / Transpose Data
- 8.7 Ranking and Nth Value Patterns
- Solved LC Problems
- Practice Problems

**Master Reference**
- Pattern Decision Table
- All LC SQL Problems by Category

---

# 🟢 Phase 1 — MySQL Foundations

---

## 1.1 What is SQL? How MySQL Works

SQL (Structured Query Language) is used to communicate with relational databases. MySQL stores data in TABLES — structured grids of rows and columns.

```
Table: Employee
┌────┬────────┬────────┬───────────┐
│ id │  name  │ salary │ dept_id   │
├────┼────────┼────────┼───────────┤
│  1 │  Alice │  90000 │     1     │
│  2 │  Bob   │  70000 │     2     │
│  3 │  Carol │  85000 │     1     │
│  4 │  Dave  │  60000 │     3     │
└────┴────────┴────────┴───────────┘

Each ROW = one record
Each COLUMN = one attribute/field
PRIMARY KEY = unique identifier for each row (id here)
FOREIGN KEY = column that references another table's primary key (dept_id)
```

### Relational Database Concepts

```
One-to-Many:  One department has MANY employees
              Department (1) ───< Employee (many)

Many-to-Many: Student can enroll in many courses,
              Course can have many students
              Student >───< Course (via enrollment table)
```

---

## 1.2 Data Types in MySQL

```sql
-- NUMERIC TYPES
INT           -- whole number: -2,147,483,648 to 2,147,483,647
BIGINT        -- larger whole number
DECIMAL(p,s)  -- exact decimal: p=total digits, s=decimal places
              -- DECIMAL(10,2) = up to 8 digits before decimal, 2 after
FLOAT         -- approximate decimal (avoid for money!)
BOOLEAN       -- 0 or 1 (TINYINT(1))

-- STRING TYPES
VARCHAR(n)    -- variable length string, max n chars
CHAR(n)       -- fixed length, always n chars (pads with spaces)
TEXT          -- large text (up to 65KB)
ENUM('A','B') -- one value from a defined list

-- DATE/TIME TYPES
DATE          -- 'YYYY-MM-DD'
DATETIME      -- 'YYYY-MM-DD HH:MM:SS'
TIMESTAMP     -- like DATETIME but auto-updates
TIME          -- 'HH:MM:SS'
YEAR          -- 'YYYY'
```

---

## 1.3 The Anatomy of a SQL Query — Execution Order

This is the MOST IMPORTANT thing to understand. SQL does NOT execute in the order you WRITE it.

```
WRITTEN ORDER:          EXECUTION ORDER:
─────────────────       ────────────────────────────
1. SELECT               1. FROM & JOIN (load tables)
2. FROM                 2. WHERE (filter rows)
3. JOIN                 3. GROUP BY (group rows)
4. WHERE                4. HAVING (filter groups)
5. GROUP BY             5. SELECT (pick columns)
6. HAVING               6. DISTINCT (remove dupes)
7. ORDER BY             7. ORDER BY (sort)
8. LIMIT                8. LIMIT / OFFSET (page)
```

```
WHY THIS MATTERS:
  You CANNOT use a SELECT alias in WHERE:
  ❌  SELECT salary * 1.1 AS new_sal FROM emp WHERE new_sal > 5000
  ✅  SELECT salary * 1.1 AS new_sal FROM emp WHERE salary * 1.1 > 5000

  You CAN use a SELECT alias in ORDER BY:
  ✅  SELECT salary * 1.1 AS new_sal FROM emp ORDER BY new_sal DESC

  You CANNOT use aggregate functions in WHERE — use HAVING:
  ❌  WHERE COUNT(*) > 5
  ✅  HAVING COUNT(*) > 5
```

---

## 1.4 Full MySQL Syntax Reference Sheet

```sql
-- ════════════════════════════════════════════
-- BASIC SELECT
-- ════════════════════════════════════════════
SELECT column1, column2
FROM table_name;

SELECT *              -- all columns (avoid in production)
FROM table_name;

SELECT column1 AS alias_name   -- rename output column
FROM table_name;

SELECT DISTINCT column1        -- unique values only
FROM table_name;

-- ════════════════════════════════════════════
-- WHERE OPERATORS
-- ════════════════════════════════════════════
WHERE salary = 5000           -- equal
WHERE salary != 5000          -- not equal (<> also works)
WHERE salary > 5000           -- greater than
WHERE salary >= 5000          -- greater or equal
WHERE salary BETWEEN 3000 AND 7000  -- inclusive range
WHERE name LIKE 'A%'          -- starts with A
WHERE name LIKE '%son'        -- ends with son
WHERE name LIKE '_a%'         -- second char is 'a'
WHERE dept IN ('IT', 'HR', 'Finance')   -- list match
WHERE dept NOT IN ('IT', 'HR')
WHERE manager_id IS NULL      -- check for NULL
WHERE manager_id IS NOT NULL
WHERE salary > 5000 AND dept = 'IT'
WHERE salary > 5000 OR dept = 'HR'
WHERE NOT (dept = 'IT')

-- ════════════════════════════════════════════
-- ORDERING AND LIMITING
-- ════════════════════════════════════════════
ORDER BY salary DESC          -- descending
ORDER BY salary ASC           -- ascending (default)
ORDER BY dept ASC, salary DESC -- multiple columns
LIMIT 10                      -- first 10 rows
LIMIT 5 OFFSET 10             -- rows 11-15 (skip first 10)
LIMIT 10, 5                   -- shorthand: skip 10, take 5

-- ════════════════════════════════════════════
-- AGGREGATE FUNCTIONS
-- ════════════════════════════════════════════
SELECT COUNT(*)               -- count all rows
SELECT COUNT(salary)          -- count non-NULL salary rows
SELECT COUNT(DISTINCT dept)   -- count unique departments
SELECT SUM(salary)            -- total salary
SELECT AVG(salary)            -- average salary
SELECT MAX(salary)            -- highest salary
SELECT MIN(salary)            -- lowest salary
SELECT GROUP_CONCAT(name)     -- join names as string
SELECT GROUP_CONCAT(name ORDER BY name SEPARATOR ', ')

-- ════════════════════════════════════════════
-- GROUP BY AND HAVING
-- ════════════════════════════════════════════
SELECT dept, COUNT(*), AVG(salary)
FROM Employee
GROUP BY dept
HAVING COUNT(*) > 5;

-- ════════════════════════════════════════════
-- JOINS
-- ════════════════════════════════════════════
-- INNER JOIN: only matching rows from BOTH tables
SELECT e.name, d.dept_name
FROM Employee e
INNER JOIN Department d ON e.dept_id = d.id;

-- LEFT JOIN: all rows from LEFT, matching from RIGHT (NULL if no match)
SELECT e.name, d.dept_name
FROM Employee e
LEFT JOIN Department d ON e.dept_id = d.id;

-- RIGHT JOIN: all rows from RIGHT, matching from LEFT
SELECT e.name, d.dept_name
FROM Employee e
RIGHT JOIN Department d ON e.dept_id = d.id;

-- SELF JOIN: join table with itself
SELECT e1.name AS employee, e2.name AS manager
FROM Employee e1
LEFT JOIN Employee e2 ON e1.manager_id = e2.id;

-- ════════════════════════════════════════════
-- SUBQUERIES
-- ════════════════════════════════════════════
-- In WHERE
WHERE salary > (SELECT AVG(salary) FROM Employee)
WHERE dept_id IN (SELECT id FROM Department WHERE location = 'NY')
WHERE NOT EXISTS (SELECT 1 FROM Orders WHERE Orders.customer_id = Customers.id)

-- As derived table (must alias it)
SELECT avg_dept.dept, avg_dept.avg_sal
FROM (
    SELECT dept, AVG(salary) AS avg_sal
    FROM Employee
    GROUP BY dept
) AS avg_dept
WHERE avg_dept.avg_sal > 70000;

-- ════════════════════════════════════════════
-- CONDITIONAL EXPRESSIONS
-- ════════════════════════════════════════════
CASE
    WHEN salary >= 90000 THEN 'High'
    WHEN salary >= 60000 THEN 'Medium'
    ELSE 'Low'
END AS salary_grade

IF(condition, value_if_true, value_if_false)
IFNULL(expression, value_if_null)
COALESCE(val1, val2, val3)    -- returns first non-NULL value
NULLIF(val1, val2)            -- returns NULL if val1 = val2

-- ════════════════════════════════════════════
-- STRING FUNCTIONS
-- ════════════════════════════════════════════
UPPER(str)                    -- 'hello' → 'HELLO'
LOWER(str)                    -- 'HELLO' → 'hello'
LENGTH(str)                   -- byte length
CHAR_LENGTH(str)              -- character count (use this!)
CONCAT(s1, s2)                -- join strings
CONCAT_WS(sep, s1, s2)       -- join with separator
SUBSTRING(str, pos, len)      -- extract part of string
LEFT(str, n)                  -- first n characters
RIGHT(str, n)                 -- last n characters
TRIM(str)                     -- remove leading/trailing spaces
LTRIM(str), RTRIM(str)
REPLACE(str, from, to)        -- replace all occurrences
INSTR(str, substr)            -- position of substr (1-indexed)
LOCATE(substr, str)           -- same as INSTR but args swapped
REVERSE(str)                  -- reverse string
LPAD(str, len, padstr)       -- left pad to length
RPAD(str, len, padstr)

-- ════════════════════════════════════════════
-- DATE FUNCTIONS
-- ════════════════════════════════════════════
CURDATE()                     -- today's date: '2024-03-15'
NOW()                         -- current datetime
YEAR(date)                    -- extract year
MONTH(date)                   -- extract month (1-12)
DAY(date)                     -- extract day
DAYOFWEEK(date)               -- 1=Sunday, 7=Saturday
DATEDIFF(date1, date2)        -- days between dates
DATE_ADD(date, INTERVAL n DAY) -- add days
DATE_SUB(date, INTERVAL n MONTH)
DATE_FORMAT(date, '%Y-%m')    -- format date as string
LAST_DAY(date)                -- last day of month
WEEKDAY(date)                 -- 0=Monday, 6=Sunday

-- ════════════════════════════════════════════
-- WINDOW FUNCTIONS
-- ════════════════════════════════════════════
ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)
RANK()        OVER (PARTITION BY dept ORDER BY salary DESC)
DENSE_RANK()  OVER (PARTITION BY dept ORDER BY salary DESC)
LAG(salary, 1)  OVER (PARTITION BY dept ORDER BY date)
LEAD(salary, 1) OVER (PARTITION BY dept ORDER BY date)
SUM(salary)   OVER (PARTITION BY dept ORDER BY date ROWS UNBOUNDED PRECEDING)
AVG(salary)   OVER (PARTITION BY dept)
FIRST_VALUE(salary) OVER (PARTITION BY dept ORDER BY salary DESC)
NTILE(4) OVER (ORDER BY salary)

-- ════════════════════════════════════════════
-- CTE (Common Table Expression)
-- ════════════════════════════════════════════
WITH cte_name AS (
    SELECT dept, AVG(salary) AS avg_sal
    FROM Employee
    GROUP BY dept
)
SELECT * FROM cte_name WHERE avg_sal > 70000;

-- Multiple CTEs
WITH
cte1 AS (SELECT ...),
cte2 AS (SELECT ... FROM cte1 ...)
SELECT * FROM cte2;

-- ════════════════════════════════════════════
-- UNION
-- ════════════════════════════════════════════
SELECT name FROM Employee
UNION               -- removes duplicates
SELECT name FROM Manager;

SELECT name FROM Employee
UNION ALL           -- keeps duplicates (faster)
SELECT name FROM Manager;

-- ════════════════════════════════════════════
-- USEFUL MYSQL-SPECIFIC
-- ════════════════════════════════════════════
IFNULL(val, 0)                -- replace NULL with 0
MOD(num, divisor)             -- remainder (like % in Java)
ROUND(num, decimals)          -- round to decimal places
FLOOR(num)                    -- round down
CEIL(num)                     -- round up
ABS(num)                      -- absolute value
POWER(base, exp)              -- base^exp
TRUNCATE(num, decimals)       -- cut digits, no rounding
```

---

# 🔵 Phase 2 — SELECT, Filter & Sort

---

## 2.1 SELECT Basics

```sql
-- Select all columns
SELECT * FROM Employee;

-- Select specific columns
SELECT id, name, salary FROM Employee;

-- Rename columns with aliases
SELECT
    name AS employee_name,
    salary AS annual_salary,
    dept_id AS department
FROM Employee;

-- Compute new columns
SELECT
    name,
    salary,
    salary * 12 AS annual_salary,
    salary * 0.1 AS tax
FROM Employee;

-- Combine text columns
SELECT
    CONCAT(first_name, ' ', last_name) AS full_name,
    email
FROM users;
```

---

## 2.2 WHERE Clause — All Operators

```sql
-- Exact match
SELECT * FROM Employee WHERE dept = 'Engineering';

-- Range: BETWEEN is INCLUSIVE on both ends
SELECT * FROM Employee WHERE salary BETWEEN 50000 AND 80000;
-- Equivalent to: WHERE salary >= 50000 AND salary <= 80000

-- Pattern matching with LIKE
-- % = any number of characters (including zero)
-- _ = exactly ONE character
SELECT * FROM Employee WHERE name LIKE 'A%';      -- starts with A
SELECT * FROM Employee WHERE name LIKE '%son';    -- ends with son
SELECT * FROM Employee WHERE name LIKE '_a%';     -- second char is a
SELECT * FROM Employee WHERE email LIKE '%.com';  -- ends with .com
SELECT * FROM Employee WHERE name LIKE 'J_n';     -- J-n (3 chars)

-- IN list
SELECT * FROM Employee WHERE dept IN ('IT', 'HR', 'Finance');
SELECT * FROM Employee WHERE id IN (1, 5, 10, 15);

-- NOT IN (be careful with NULLs!)
SELECT * FROM Employee WHERE dept NOT IN ('IT', 'HR');

-- Multiple conditions
SELECT * FROM Employee
WHERE dept = 'IT'
  AND salary > 70000
  AND hire_date >= '2020-01-01';

-- OR conditions (use parentheses to control precedence!)
SELECT * FROM Employee
WHERE (dept = 'IT' AND salary > 70000)
   OR (dept = 'HR' AND salary > 60000);
```

---

## 2.3 NULL Handling — Critical!

```sql
-- NULL is NOT a value — it means "unknown" or "missing"
-- NEVER use = or != with NULL!

-- ❌ WRONG:
SELECT * FROM Employee WHERE manager_id = NULL;   -- returns nothing!
SELECT * FROM Employee WHERE manager_id != NULL;  -- returns nothing!

-- ✅ CORRECT:
SELECT * FROM Employee WHERE manager_id IS NULL;
SELECT * FROM Employee WHERE manager_id IS NOT NULL;

-- NULL in expressions
SELECT
    name,
    salary,
    bonus,
    salary + IFNULL(bonus, 0) AS total_comp  -- treat NULL bonus as 0
FROM Employee;

-- referee_id <> 2 needs special care
-- ❌ This misses rows where referee_id IS NULL:
SELECT name FROM Customer WHERE referee_id <> 2;
-- ✅ Correct:
SELECT name FROM Customer WHERE referee_id != 2 OR referee_id IS NULL;

-- COALESCE: returns first non-NULL value
SELECT COALESCE(phone_mobile, phone_home, phone_work, 'No Phone') AS contact
FROM Person;
```

---

## 2.4 ORDER BY, LIMIT, OFFSET

```sql
-- Sort ascending (default)
SELECT * FROM Employee ORDER BY salary;

-- Sort descending
SELECT * FROM Employee ORDER BY salary DESC;

-- Sort by multiple columns
SELECT * FROM Employee ORDER BY dept ASC, salary DESC;
-- Within each dept, highest salary first

-- Sort with NULLs (NULLs appear first in ASC, last in DESC in MySQL)
SELECT * FROM Employee ORDER BY manager_id ASC;

-- LIMIT: take only n rows
SELECT * FROM Employee ORDER BY salary DESC LIMIT 1;  -- highest paid
SELECT * FROM Employee ORDER BY salary DESC LIMIT 3;  -- top 3

-- OFFSET: skip rows (for pagination)
SELECT * FROM Employee ORDER BY salary DESC LIMIT 5 OFFSET 0;  -- page 1
SELECT * FROM Employee ORDER BY salary DESC LIMIT 5 OFFSET 5;  -- page 2
SELECT * FROM Employee ORDER BY salary DESC LIMIT 5 OFFSET 10; -- page 3

-- Shorthand: LIMIT offset, count
SELECT * FROM Employee ORDER BY salary DESC LIMIT 5, 5;  -- same as above page 2

-- 2nd highest salary (classic interview trick)
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
-- LIMIT 1 OFFSET 1 means: skip 1, take 1 = 2nd row
```

---

## 2.5 DISTINCT

```sql
-- Unique values
SELECT DISTINCT dept FROM Employee;

-- DISTINCT on multiple columns (unique COMBINATION)
SELECT DISTINCT dept, job_title FROM Employee;

-- Count unique values
SELECT COUNT(DISTINCT dept) AS unique_depts FROM Employee;

-- DISTINCT vs GROUP BY (often interchangeable for simple cases)
SELECT DISTINCT dept FROM Employee;
-- is equivalent to:
SELECT dept FROM Employee GROUP BY dept;
```

---

## Solved LC Problems — Phase 2

### LC 595 — Big Countries

Find countries where area ≥ 3,000,000 OR population ≥ 25,000,000.

```sql
-- Table: World (name, continent, area, population, gdp)

SELECT name, population, area
FROM World
WHERE area >= 3000000
   OR population >= 25000000;
```

### LC 1757 — Recyclable and Low Fat Products

```sql
-- Table: Products (product_id, low_fats ENUM('Y','N'), recyclable ENUM('Y','N'))

SELECT product_id
FROM Products
WHERE low_fats = 'Y'
  AND recyclable = 'Y';
```

### LC 584 — Find Customer Referee

Customers NOT referred by customer id=2 (including those with no referee).

```sql
-- Table: Customer (id, name, referee_id)

SELECT name
FROM Customer
WHERE referee_id != 2
   OR referee_id IS NULL;
-- ⚠️ Must include OR IS NULL because != doesn't match NULLs
```

### LC 183 — Customers Who Never Order

```sql
-- Tables: Customers (id, name), Orders (id, customerId)

SELECT name AS Customers
FROM Customers
WHERE id NOT IN (
    SELECT customerId FROM Orders
);

-- Alternative with LEFT JOIN (often faster):
SELECT c.name AS Customers
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customerId
WHERE o.customerId IS NULL;
```

### LC 1148 — Article Views I

Authors who viewed their OWN articles.

```sql
-- Table: Views (article_id, author_id, viewer_id, view_date)

SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY id ASC;
```

### Practice Problems — Phase 2

| # | Problem | Topic |
|---|---------|-------|
| 595 | Big Countries | WHERE with OR |
| 1757 | Recyclable and Low Fat Products | WHERE with AND |
| 584 | Find Customer Referee | NULL handling |
| 183 | Customers Who Never Order | NOT IN / LEFT JOIN |
| 1148 | Article Views I | DISTINCT |
| 1683 | Invalid Tweets | LENGTH filter |
| 196 | Delete Duplicate Emails | DELETE with subquery |
| 197 | Rising Temperature | Self-join date comparison |

---

# 🟡 Phase 3 — Aggregation & Grouping

---

## 3.1 Aggregate Functions

```sql
-- COUNT: count rows
SELECT COUNT(*) FROM Employee;                 -- all rows including NULLs
SELECT COUNT(salary) FROM Employee;            -- rows where salary is NOT NULL
SELECT COUNT(DISTINCT dept) FROM Employee;     -- unique departments

-- SUM
SELECT SUM(salary) AS total_payroll FROM Employee;
SELECT SUM(CASE WHEN dept = 'IT' THEN salary ELSE 0 END) AS it_payroll
FROM Employee;

-- AVG: skips NULL values automatically
SELECT AVG(salary) AS avg_salary FROM Employee;
SELECT ROUND(AVG(salary), 2) AS avg_salary FROM Employee;  -- round to 2 decimals

-- MAX, MIN
SELECT MAX(salary) AS highest, MIN(salary) AS lowest FROM Employee;

-- GROUP_CONCAT: concatenate values into a string
SELECT dept, GROUP_CONCAT(name ORDER BY name SEPARATOR ', ') AS employees
FROM Employee
GROUP BY dept;
```

---

## 3.2 GROUP BY

```sql
-- Count employees per department
SELECT dept, COUNT(*) AS emp_count
FROM Employee
GROUP BY dept;

-- Average salary per department
SELECT dept, ROUND(AVG(salary), 0) AS avg_salary
FROM Employee
GROUP BY dept
ORDER BY avg_salary DESC;

-- Multiple GROUP BY columns
SELECT dept, job_title, COUNT(*) AS count, AVG(salary) AS avg_sal
FROM Employee
GROUP BY dept, job_title;

-- GROUP BY with expressions
SELECT YEAR(hire_date) AS hire_year, COUNT(*) AS hires
FROM Employee
GROUP BY YEAR(hire_date)
ORDER BY hire_year;

-- COUNT of each category
SELECT
    SUM(CASE WHEN status = 'active' THEN 1 ELSE 0 END) AS active_count,
    SUM(CASE WHEN status = 'inactive' THEN 1 ELSE 0 END) AS inactive_count,
    COUNT(*) AS total
FROM Employee;
```

---

## 3.3 HAVING vs WHERE

```sql
-- WHERE filters INDIVIDUAL ROWS before grouping
-- HAVING filters GROUPS after aggregation

-- Departments with more than 5 employees AND average salary > 70000:
SELECT dept, COUNT(*) AS emp_count, AVG(salary) AS avg_sal
FROM Employee
WHERE hire_date >= '2020-01-01'   -- filter rows FIRST
GROUP BY dept
HAVING COUNT(*) > 5               -- then filter groups
   AND AVG(salary) > 70000;

-- ❌ WRONG — can't use aggregate in WHERE:
SELECT dept FROM Employee WHERE COUNT(*) > 5;

-- ✅ CORRECT — use HAVING:
SELECT dept FROM Employee GROUP BY dept HAVING COUNT(*) > 5;
```

---

## 3.4 Combining Aggregates

```sql
-- Multiple metrics per group
SELECT
    dept,
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    ROUND(AVG(salary), 0) AS avg_salary,
    MAX(salary) AS max_salary,
    MIN(salary) AS min_salary
FROM Employee
GROUP BY dept
ORDER BY total_salary DESC;

-- Percentage of total
SELECT
    dept,
    COUNT(*) AS emp_count,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM Employee), 1) AS pct
FROM Employee
GROUP BY dept;
```

---

## Solved LC Problems — Phase 3

### LC 1251 — Average Selling Price

```sql
-- Tables: Prices (product_id, start_date, end_date, price)
--         UnitsSold (product_id, purchase_date, units)

SELECT
    p.product_id,
    ROUND(
        SUM(p.price * u.units) / SUM(u.units),
        2
    ) AS average_price
FROM Prices p
JOIN UnitsSold u
    ON p.product_id = u.product_id
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;
```

### LC 1633 — Percentage of Users Attended a Contest

```sql
-- Tables: Users (user_id, user_name)
--         Register (contest_id, user_id)

SELECT
    contest_id,
    ROUND(COUNT(user_id) * 100 / (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC;
```

### LC 1211 — Queries Quality and Percentage

```sql
-- Table: Queries (query_name, result, position, rating)

SELECT
    query_name,
    ROUND(AVG(rating / position), 2) AS quality,
    ROUND(SUM(IF(rating < 3, 1, 0)) * 100 / COUNT(*), 2) AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name;
```

### LC 1193 — Monthly Transactions I

```sql
-- Table: Transactions (id, country, state, amount, trans_date)

SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(*) AS trans_count,
    SUM(state = 'approved') AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(IF(state = 'approved', amount, 0)) AS approved_total_amount
FROM Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;
```

### LC 550 — Game Play Analysis IV

```sql
-- Table: Activity (player_id, device_id, event_date, games_played)
-- Find fraction of players who logged in again the day AFTER first login

WITH FirstLogin AS (
    SELECT player_id, MIN(event_date) AS first_date
    FROM Activity
    GROUP BY player_id
)
SELECT
    ROUND(
        COUNT(DISTINCT a.player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity),
        2
    ) AS fraction
FROM Activity a
JOIN FirstLogin f ON a.player_id = f.player_id
WHERE DATEDIFF(a.event_date, f.first_date) = 1;
```

### Practice Problems — Phase 3

| # | Problem | Topic |
|---|---------|-------|
| 620 | Not Boring Movies | WHERE + ORDER BY |
| 1251 | Average Selling Price | JOIN + AVG |
| 1633 | Percentage of Users Attended Contest | COUNT + subquery |
| 1211 | Queries Quality and Percentage | AVG, conditional sum |
| 1193 | Monthly Transactions I | GROUP BY date |
| 1174 | Immediate Food Delivery II | GROUP BY + conditional |
| 550 | Game Play Analysis IV | MIN + date diff |
| 1045 | Customers Who Bought All Products | COUNT DISTINCT compare |

---

# 🟠 Phase 4 — JOINs

---

## 4.1 INNER JOIN

Returns ONLY rows where there is a match in BOTH tables.

```
Table A:           Table B:
id | name          id | dept_name
1  | Alice          1 | Engineering
2  | Bob            3 | HR
3  | Carol

INNER JOIN on dept_id = id:
name  | dept_name
Alice | Engineering
Carol | HR
(Bob has dept_id=2, no match → EXCLUDED)
```

```sql
SELECT e.name, d.dept_name, e.salary
FROM Employee e
INNER JOIN Department d ON e.dept_id = d.id;

-- JOIN on multiple conditions
SELECT *
FROM Orders o
JOIN Products p ON o.product_id = p.id AND p.category = 'Electronics';

-- JOIN with WHERE (WHERE filters AFTER join)
SELECT e.name, d.dept_name
FROM Employee e
JOIN Department d ON e.dept_id = d.id
WHERE e.salary > 70000;
```

---

## 4.2 LEFT JOIN

Returns ALL rows from LEFT table + matching rows from RIGHT.
Non-matching right side = NULL.

```
LEFT JOIN:
name  | dept_name
Alice | Engineering
Bob   | NULL         ← Bob kept, no dept match
Carol | HR
```

```sql
SELECT e.name, d.dept_name
FROM Employee e
LEFT JOIN Department d ON e.dept_id = d.id;

-- Find employees WITHOUT a department:
SELECT e.name
FROM Employee e
LEFT JOIN Department d ON e.dept_id = d.id
WHERE d.id IS NULL;    -- NULL on right side = no match

-- Find customers who NEVER ordered:
SELECT c.name
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customer_id
WHERE o.id IS NULL;    -- key trick for "not in" using LEFT JOIN
```

---

## 4.3 RIGHT JOIN

Returns ALL rows from RIGHT table + matching from LEFT.
Use LEFT JOIN instead (just swap table order) — cleaner.

```sql
-- These are equivalent:
SELECT * FROM A RIGHT JOIN B ON A.id = B.a_id;
SELECT * FROM B LEFT JOIN A ON A.id = B.a_id;  -- prefer this
```

---

## 4.4 FULL OUTER JOIN (MySQL Workaround)

MySQL doesn't support FULL OUTER JOIN directly. Simulate with UNION:

```sql
-- Full outer join = left join UNION right join
SELECT e.name, d.dept_name
FROM Employee e
LEFT JOIN Department d ON e.dept_id = d.id

UNION

SELECT e.name, d.dept_name
FROM Employee e
RIGHT JOIN Department d ON e.dept_id = d.id;
```

---

## 4.5 SELF JOIN

Join a table with ITSELF. Always use different aliases!

```sql
-- Find employees and their managers (manager is also in Employee table)
SELECT
    e.name AS employee,
    m.name AS manager
FROM Employee e
LEFT JOIN Employee m ON e.manager_id = m.id;

-- Find employees who earn MORE than their manager:
SELECT e.name AS Employee
FROM Employee e
JOIN Employee m ON e.manager_id = m.id
WHERE e.salary > m.salary;

-- Find pairs of employees in the same department:
SELECT e1.name, e2.name, e1.dept
FROM Employee e1
JOIN Employee e2
    ON e1.dept = e2.dept
    AND e1.id < e2.id;  -- e1.id < e2.id prevents (A,B) and (B,A) duplicates
```

---

## 4.6 Multiple JOINs

```sql
-- Three-table join
SELECT
    e.name AS employee,
    d.dept_name,
    p.project_name
FROM Employee e
JOIN Department d ON e.dept_id = d.id
JOIN ProjectAssignment pa ON e.id = pa.employee_id
JOIN Project p ON pa.project_id = p.id
WHERE d.location = 'New York';

-- Mix of JOIN types
SELECT
    e.name,
    d.dept_name,
    m.name AS manager_name
FROM Employee e
LEFT JOIN Department d ON e.dept_id = d.id   -- keep employees without dept
JOIN Employee m ON e.manager_id = m.id;       -- only employees with manager
```

---

## Solved LC Problems — Phase 4

### LC 175 — Combine Two Tables

```sql
-- Tables: Person (personId, firstName, lastName)
--         Address (addressId, personId, city, state)
-- Get all persons regardless of whether they have an address

SELECT
    p.firstName,
    p.lastName,
    a.city,
    a.state
FROM Person p
LEFT JOIN Address a ON p.personId = a.personId;
-- LEFT JOIN keeps all persons even without address (NULL city/state)
```

### LC 181 — Employees Earning More Than Their Manager

```sql
-- Table: Employee (id, name, salary, managerId)

SELECT e.name AS Employee
FROM Employee e
JOIN Employee m ON e.managerId = m.id
WHERE e.salary > m.salary;
-- Self join: compare employee's salary with their manager's salary
```

### LC 197 — Rising Temperature

```sql
-- Table: Weather (id, recordDate, temperature)
-- Find days where temperature was higher than the day before

SELECT w1.id
FROM Weather w1
JOIN Weather w2
    ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;
-- Self join: compare today (w1) with yesterday (w2)
```

### LC 1661 — Average Time of Process per Machine

```sql
-- Table: Activity (machine_id, process_id, activity_type ('start'/'end'), timestamp)

SELECT
    a.machine_id,
    ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a
JOIN Activity b
    ON a.machine_id = b.machine_id
    AND a.process_id = b.process_id
    AND a.activity_type = 'start'
    AND b.activity_type = 'end'
GROUP BY a.machine_id;
```

### LC 577 — Employee Bonus

```sql
-- Tables: Employee (empId, name, supervisor, salary)
--         Bonus (empId, bonus)

SELECT e.name, b.bonus
FROM Employee e
LEFT JOIN Bonus b ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;
-- LEFT JOIN keeps employees with no bonus record (NULL)
```

### LC 1280 — Students and Examinations

```sql
-- Tables: Students (student_id, student_name)
--         Subjects (subject_name)
--         Examinations (student_id, subject_name)

SELECT
    s.student_id,
    s.student_name,
    sub.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e
    ON s.student_id = e.student_id
    AND sub.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
-- CROSS JOIN creates all student-subject pairs, then count actual exams
```

### Practice Problems — Phase 4

| # | Problem | Topic |
|---|---------|-------|
| 175 | Combine Two Tables | LEFT JOIN |
| 181 | Employees Earning More Than Manager | Self JOIN |
| 183 | Customers Who Never Order | LEFT JOIN + IS NULL |
| 197 | Rising Temperature | Self JOIN + DATEDIFF |
| 1661 | Average Time of Process per Machine | Self JOIN + AVG |
| 577 | Employee Bonus | LEFT JOIN + OR IS NULL |
| 1280 | Students and Examinations | CROSS JOIN + LEFT JOIN |
| 570 | Managers with at Least 5 Direct Reports | GROUP BY + HAVING |
| 1934 | Confirmation Rate | LEFT JOIN + AVG conditional |

---

# 🔴 Phase 5 — Subqueries

---

## 5.1 Scalar Subquery

A subquery that returns EXACTLY ONE value.

```sql
-- Get employees earning above average
SELECT name, salary
FROM Employee
WHERE salary > (SELECT AVG(salary) FROM Employee);
-- AVG returns one value: ok as scalar subquery

-- Use scalar subquery in SELECT column
SELECT
    name,
    salary,
    (SELECT AVG(salary) FROM Employee) AS company_avg,
    salary - (SELECT AVG(salary) FROM Employee) AS above_avg
FROM Employee;

-- 2nd highest salary (classic problem)
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

---

## 5.2 Subquery in WHERE — IN, NOT IN, EXISTS

```sql
-- IN: check if value is in a list returned by subquery
SELECT name FROM Customer
WHERE id IN (SELECT customerId FROM Orders WHERE amount > 1000);

-- NOT IN: ⚠️ careful with NULLs!
-- If subquery returns any NULL, NOT IN returns nothing!
SELECT name FROM Customer
WHERE id NOT IN (
    SELECT customerId FROM Orders WHERE customerId IS NOT NULL  -- ← add IS NOT NULL!
);

-- EXISTS: true if subquery returns ANY rows (more efficient than IN for large tables)
SELECT name FROM Customer c
WHERE EXISTS (
    SELECT 1 FROM Orders o WHERE o.customerId = c.id  -- ← note: references outer table
);

-- NOT EXISTS
SELECT name FROM Customer c
WHERE NOT EXISTS (
    SELECT 1 FROM Orders o WHERE o.customerId = c.id
);
```

---

## 5.3 Correlated Subquery

A subquery that references the OUTER query. Runs ONCE PER ROW of the outer query.

```sql
-- Employees earning more than the AVERAGE OF THEIR OWN DEPARTMENT
SELECT name, dept, salary
FROM Employee e1
WHERE salary > (
    SELECT AVG(salary)
    FROM Employee e2
    WHERE e2.dept = e1.dept  -- ← references outer query's dept
);

-- Find the latest order for each customer
SELECT *
FROM Orders o1
WHERE order_date = (
    SELECT MAX(order_date)
    FROM Orders o2
    WHERE o2.customer_id = o1.customer_id  -- ← correlated
);
```

---

## 5.4 Subquery in FROM (Derived Table)

```sql
-- Must give derived table an alias!
SELECT dept, avg_salary
FROM (
    SELECT dept, AVG(salary) AS avg_salary
    FROM Employee
    GROUP BY dept
) AS dept_averages      -- ← alias required
WHERE avg_salary > 70000;

-- Rank salaries without window function (older MySQL)
SELECT s1.salary, COUNT(DISTINCT s2.salary) AS rank_num
FROM Salaries s1
JOIN Salaries s2 ON s2.salary >= s1.salary
GROUP BY s1.salary
ORDER BY s1.salary DESC;
```

---

## 5.5 Subquery vs JOIN Comparison

```sql
-- Find customers who HAVE orders:

-- WITH subquery (IN):
SELECT name FROM Customer WHERE id IN (SELECT customerId FROM Orders);

-- WITH JOIN:
SELECT DISTINCT c.name
FROM Customer c
JOIN Orders o ON c.id = o.customerId;

-- Find customers who DON'T have orders:

-- WITH subquery (NOT IN):
SELECT name FROM Customer
WHERE id NOT IN (SELECT customerId FROM Orders WHERE customerId IS NOT NULL);

-- WITH LEFT JOIN (preferred, handles NULLs automatically):
SELECT c.name
FROM Customer c
LEFT JOIN Orders o ON c.id = o.customerId
WHERE o.customerId IS NULL;
```

---

## Solved LC Problems — Phase 5

### LC 176 — Second Highest Salary

```sql
-- Return NULL if there is no second highest salary

SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1   -- skip highest, take next
) AS SecondHighestSalary;

-- Wrap in outer SELECT to return NULL when no result exists
```

### LC 178 — Rank Scores

```sql
-- Table: Scores (id, score)
-- Rank scores with no gaps (DENSE_RANK)

SELECT
    score,
    DENSE_RANK() OVER (ORDER BY score DESC) AS `rank`
FROM Scores
ORDER BY score DESC;

-- Without window functions (older approach):
SELECT
    s1.score,
    COUNT(DISTINCT s2.score) AS `rank`
FROM Scores s1
JOIN Scores s2 ON s2.score >= s1.score
GROUP BY s1.id, s1.score
ORDER BY s1.score DESC;
```

### LC 185 — Department Top Three Salaries

```sql
-- Tables: Employee (id, name, salary, departmentId)
--         Department (id, name)

SELECT
    d.name AS Department,
    e.name AS Employee,
    e.salary AS Salary
FROM Employee e
JOIN Department d ON e.departmentId = d.id
WHERE (
    SELECT COUNT(DISTINCT e2.salary)
    FROM Employee e2
    WHERE e2.departmentId = e.departmentId
      AND e2.salary > e.salary
) < 3;  -- at most 2 salaries higher = top 3

-- OR using window functions (cleaner):
SELECT Department, Employee, Salary
FROM (
    SELECT
        d.name AS Department,
        e.name AS Employee,
        e.salary AS Salary,
        DENSE_RANK() OVER (
            PARTITION BY e.departmentId
            ORDER BY e.salary DESC
        ) AS rnk
    FROM Employee e
    JOIN Department d ON e.departmentId = d.id
) ranked
WHERE rnk <= 3;
```

### LC 180 — Consecutive Numbers

```sql
-- Table: Logs (id, num)
-- Find numbers that appear AT LEAST 3 times consecutively

SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l2.id = l1.id + 1
JOIN Logs l3 ON l3.id = l1.id + 2
WHERE l1.num = l2.num
  AND l2.num = l3.num;
-- Join consecutive rows by id and check same value
```

### Practice Problems — Phase 5

| # | Problem | Topic |
|---|---------|-------|
| 176 | Second Highest Salary | LIMIT OFFSET / subquery |
| 178 | Rank Scores | DENSE_RANK / subquery rank |
| 180 | Consecutive Numbers | Self-join consecutive rows |
| 185 | Department Top Three Salaries | Subquery / DENSE_RANK |
| 184 | Department Highest Salary | Subquery / GROUP BY |
| 626 | Exchange Seats | CASE WHEN + subquery |
| 1164 | Product Price at a Given Date | Subquery + date |

---

# 🟤 Phase 6 — String, Date & Math Functions

---

## 6.1 String Functions

```sql
-- LENGTH vs CHAR_LENGTH
SELECT LENGTH('hello');      -- 5 (bytes)
SELECT CHAR_LENGTH('hello'); -- 5 (characters) — use this!
SELECT CHAR_LENGTH('héllo'); -- 5 (CHAR_LENGTH counts chars, not bytes)

-- UPPER / LOWER
SELECT UPPER('hello world');  -- HELLO WORLD
SELECT LOWER('HELLO WORLD');  -- hello world

-- CONCAT
SELECT CONCAT('Hello', ' ', 'World');           -- Hello World
SELECT CONCAT_WS(' ', 'John', 'Doe');           -- John Doe (with separator)
SELECT CONCAT_WS(', ', 'New York', 'NY');       -- New York, NY

-- SUBSTRING
SELECT SUBSTRING('Hello World', 1, 5);  -- Hello (1-indexed!)
SELECT SUBSTRING('Hello World', 7);     -- World (from pos 7 to end)
SELECT LEFT('Hello World', 5);          -- Hello
SELECT RIGHT('Hello World', 5);         -- World

-- REPLACE
SELECT REPLACE('Hello World', 'World', 'MySQL'); -- Hello MySQL
SELECT REPLACE(phone, '-', '');                   -- remove dashes

-- TRIM
SELECT TRIM('  Hello  ');              -- 'Hello'
SELECT TRIM(LEADING '0' FROM '00123'); -- '123' (remove leading zeros)

-- LIKE pattern generation
SELECT name FROM Employee WHERE name LIKE CONCAT('%', 'son', '%');

-- Find position of substring
SELECT LOCATE('o', 'Hello World');      -- 5 (first 'o')
SELECT LOCATE('o', 'Hello World', 6);   -- 8 (search from pos 6)

-- Practical examples
SELECT
    UPPER(LEFT(name, 1)) || LOWER(SUBSTRING(name, 2)) AS title_case  -- Not MySQL
FROM Employee;

-- MySQL title case (capitalize first letter):
SELECT CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS title_case
FROM Employee;
```

---

## 6.2 Date & Time Functions

```sql
-- Get current date/time
SELECT CURDATE();              -- 2024-03-15
SELECT NOW();                  -- 2024-03-15 14:30:00
SELECT YEAR(NOW());            -- 2024

-- Extract parts
SELECT YEAR('2024-03-15');     -- 2024
SELECT MONTH('2024-03-15');    -- 3
SELECT DAY('2024-03-15');      -- 15
SELECT DAYOFWEEK('2024-03-15');-- 6 (1=Sun, 7=Sat)
SELECT WEEKDAY('2024-03-15');  -- 4 (0=Mon, 6=Sun)

-- Date arithmetic
SELECT DATEDIFF('2024-12-31', '2024-01-01');  -- 365 (days between)
SELECT DATE_ADD('2024-01-01', INTERVAL 30 DAY);  -- 2024-01-31
SELECT DATE_SUB('2024-03-15', INTERVAL 1 MONTH); -- 2024-02-15
SELECT DATE_ADD(hire_date, INTERVAL 90 DAY) AS probation_end FROM Employee;

-- Format dates
SELECT DATE_FORMAT(NOW(), '%Y-%m');       -- '2024-03'
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d');    -- '2024-03-15'
SELECT DATE_FORMAT(NOW(), '%M %d, %Y');   -- 'March 15, 2024'

-- First day of month
SELECT DATE_FORMAT(order_date, '%Y-%m-01') AS month_start FROM Orders;
-- Or: DATE_SUB(order_date, INTERVAL DAY(order_date)-1 DAY)

-- Practical date filtering
SELECT * FROM Orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-03-31';

SELECT * FROM Orders
WHERE YEAR(order_date) = 2024 AND MONTH(order_date) = 3;
```

---

## 6.3 Math Functions

```sql
SELECT ROUND(3.14159, 2);    -- 3.14
SELECT ROUND(2.5);           -- 3
SELECT FLOOR(3.7);           -- 3
SELECT CEIL(3.2);            -- 4
SELECT ABS(-5);              -- 5
SELECT MOD(10, 3);           -- 1 (remainder)
SELECT POWER(2, 10);         -- 1024
SELECT SQRT(16);             -- 4
SELECT TRUNCATE(3.14159, 2); -- 3.14 (no rounding, just cuts)

-- Practical
SELECT ROUND(AVG(salary), 2) AS avg_salary FROM Employee;
SELECT FLOOR(salary / 10000) * 10000 AS salary_bucket FROM Employee;
```

---

## 6.4 CASE WHEN Expression

```sql
-- Simple CASE (like switch)
SELECT
    name,
    CASE dept
        WHEN 'IT' THEN 'Technology'
        WHEN 'HR' THEN 'Human Resources'
        WHEN 'Finance' THEN 'Finance'
        ELSE 'Other'
    END AS department_name
FROM Employee;

-- Searched CASE (like if-else, more flexible)
SELECT
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'Executive'
        WHEN salary >= 80000  THEN 'Senior'
        WHEN salary >= 60000  THEN 'Mid-level'
        ELSE 'Junior'
    END AS level
FROM Employee;

-- CASE in aggregate (count by condition)
SELECT
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count,
    COUNT(*) AS total
FROM Employee;

-- CASE in ORDER BY (custom sort order)
SELECT name, status
FROM Tasks
ORDER BY
    CASE status
        WHEN 'urgent' THEN 1
        WHEN 'high' THEN 2
        WHEN 'medium' THEN 3
        ELSE 4
    END;
```

---

## 6.5 IFNULL, COALESCE, IF

```sql
-- IFNULL: replace NULL with a value
SELECT IFNULL(bonus, 0) AS bonus FROM Employee;
SELECT IFNULL(manager_name, 'No Manager') FROM Employee;

-- COALESCE: first non-NULL from list
SELECT COALESCE(bonus, commission, base_salary) AS compensation FROM Employee;

-- IF: simple ternary
SELECT IF(salary > 70000, 'High', 'Normal') AS pay_grade FROM Employee;

-- Combine with aggregation
SELECT
    COUNT(*) AS total,
    SUM(IF(salary > 70000, 1, 0)) AS high_earners,
    AVG(IF(dept = 'IT', salary, NULL)) AS it_avg_salary  -- NULL ignored by AVG
FROM Employee;
```

---

## Solved LC Problems — Phase 6

### LC 1527 — Patients With a Condition

```sql
-- Table: Patients (patient_id, patient_name, conditions)
-- Find patients with Type I Diabetes (condition starts with 'DIAB1')

SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions LIKE 'DIAB1%'      -- starts with DIAB1
   OR conditions LIKE '% DIAB1%';   -- word DIAB1 in middle
-- Need to handle 'DIAB1...' at start OR after a space
```

### LC 626 — Exchange Seats

```sql
-- Table: Seat (id, student)
-- Swap seats for adjacent pairs. Last seat stays if total is odd.

SELECT
    CASE
        WHEN id % 2 = 1 AND id < (SELECT COUNT(*) FROM Seat)
            THEN id + 1
        WHEN id % 2 = 0
            THEN id - 1
        ELSE id
    END AS id,
    student
FROM Seat
ORDER BY id;
```

### LC 1667 — Fix Names in a Table

```sql
-- Table: Users (user_id, name)
-- Fix: first letter uppercase, rest lowercase

SELECT
    user_id,
    CONCAT(
        UPPER(LEFT(name, 1)),
        LOWER(SUBSTRING(name, 2))
    ) AS name
FROM Users
ORDER BY user_id;
```

### Practice Problems — Phase 6

| # | Problem | Topic |
|---|---------|-------|
| 1527 | Patients With Condition | LIKE pattern |
| 1667 | Fix Names in Table | String functions |
| 1484 | Group Sold Products By Date | GROUP_CONCAT |
| 1965 | Employees With Missing Information | String + UNION |
| 626 | Exchange Seats | CASE WHEN + MOD |
| 1158 | Market Analysis I | Date functions |
| 1393 | Capital Gain/Loss | Conditional SUM |

---

# ⚫ Phase 7 — Window Functions

---

## 7.1 What are Window Functions?

Window functions perform calculations across a SET OF RELATED ROWS without collapsing rows (unlike GROUP BY).

```
GROUP BY collapses:          Window functions DON'T collapse:
dept | avg_salary            name  | dept | salary | avg_by_dept
─────────────────            ─────────────────────────────────────
IT   | 85000                 Alice | IT   | 90000  | 85000
HR   | 65000                 Bob   | IT   | 80000  | 85000
                             Carol | HR   | 65000  | 65000
                             Dave  | HR   | 65000  | 65000
```

### Syntax Template

```sql
function_name()
OVER (
    PARTITION BY column1, column2  -- optional: divide rows into groups
    ORDER BY column3 DESC          -- optional: define order within partition
    ROWS/RANGE frame_clause        -- optional: define which rows to include
)
```

---

## 7.2 ROW_NUMBER, RANK, DENSE_RANK

```sql
-- Example data: salaries [100, 90, 90, 80]

-- ROW_NUMBER: unique sequential number, no ties
SELECT name, salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
-- Result: 1, 2, 3, 4 (even for ties)

-- RANK: gaps in sequence for ties
SELECT name, salary,
    RANK() OVER (ORDER BY salary DESC) AS rnk
-- Result: 1, 2, 2, 4 (90,90 both get rank 2, next is 4, not 3)

-- DENSE_RANK: no gaps for ties
SELECT name, salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rnk
-- Result: 1, 2, 2, 3 (no gap after tie)

-- PARTITION BY: rank WITHIN each department
SELECT
    name,
    dept,
    salary,
    DENSE_RANK() OVER (
        PARTITION BY dept          -- restart ranking for each department
        ORDER BY salary DESC
    ) AS dept_rank
FROM Employee;
```

### When to use which?

```
ROW_NUMBER:  need unique numbering regardless of ties (pagination, dedup)
RANK:        competition-style ranking (2nd place might not exist if 2 firsts)
DENSE_RANK:  ranking without gaps (most common for "top N per group" problems)
```

---

## 7.3 LAG and LEAD

Access values from PREVIOUS or NEXT rows without a self-join.

```sql
-- LAG: previous row's value
SELECT
    name,
    salary,
    LAG(salary, 1) OVER (ORDER BY hire_date) AS prev_salary,
    salary - LAG(salary, 1, 0) OVER (ORDER BY hire_date) AS salary_change
FROM Employee;
-- LAG(column, offset, default) — default used when no previous row

-- LEAD: next row's value
SELECT
    name,
    salary,
    LEAD(salary, 1) OVER (ORDER BY hire_date) AS next_salary
FROM Employee;

-- LAG within partitions (e.g., previous month's sales per category)
SELECT
    category,
    sale_month,
    revenue,
    LAG(revenue) OVER (
        PARTITION BY category
        ORDER BY sale_month
    ) AS prev_month_revenue
FROM MonthlySales;

-- Calculate month-over-month growth
SELECT
    category,
    sale_month,
    revenue,
    ROUND(
        (revenue - LAG(revenue) OVER (PARTITION BY category ORDER BY sale_month))
        * 100.0
        / LAG(revenue) OVER (PARTITION BY category ORDER BY sale_month),
        2
    ) AS pct_growth
FROM MonthlySales;
```

---

## 7.4 Running Totals — SUM/AVG OVER

```sql
-- Running total (cumulative sum)
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM Orders;

-- Running average
SELECT
    order_date,
    amount,
    AVG(amount) OVER (ORDER BY order_date ROWS UNBOUNDED PRECEDING) AS running_avg
FROM Orders;

-- Rolling 3-day sum (current + 2 previous)
SELECT
    order_date,
    amount,
    SUM(amount) OVER (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS rolling_3day_sum
FROM Orders;

-- Cumulative sum reset per partition
SELECT
    customer_id,
    order_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS customer_running_total
FROM Orders;
```

### Frame Clauses Reference

```sql
ROWS UNBOUNDED PRECEDING   -- from start to current row
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  -- same, explicit
ROWS BETWEEN 2 PRECEDING AND CURRENT ROW          -- rolling 3 rows
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING          -- centered 3-row window
ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING  -- current to end
```

---

## 7.5 PARTITION BY vs GROUP BY

```sql
-- GROUP BY: collapses to one row per group, can't see individual rows
SELECT dept, AVG(salary) FROM Employee GROUP BY dept;

-- PARTITION BY: keeps all rows, adds aggregate as extra column
SELECT
    name, dept, salary,
    AVG(salary) OVER (PARTITION BY dept) AS dept_avg,
    salary - AVG(salary) OVER (PARTITION BY dept) AS diff_from_dept_avg
FROM Employee;

-- PARTITION BY without ORDER BY = whole partition as window
-- (AVG of entire dept, same for all rows in dept)

-- PARTITION BY with ORDER BY = running aggregate
SELECT
    customer_id,
    order_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY customer_id
        ORDER BY order_date
    ) AS running_total_per_customer
FROM Orders;
```

---

## 7.6 FIRST_VALUE, LAST_VALUE, NTH_VALUE, NTILE

```sql
-- FIRST_VALUE: first value in window
SELECT
    name, dept, salary,
    FIRST_VALUE(name) OVER (
        PARTITION BY dept
        ORDER BY salary DESC
    ) AS highest_paid_in_dept
FROM Employee;

-- NTILE: divide rows into N buckets (quartiles, percentiles)
SELECT
    name, salary,
    NTILE(4) OVER (ORDER BY salary) AS quartile
FROM Employee;
-- quartile 1 = bottom 25%, quartile 4 = top 25%
```

---

## Solved LC Problems — Phase 7

### LC 185 — Department Top Three Salaries (Window version)

```sql
SELECT Department, Employee, Salary
FROM (
    SELECT
        d.name AS Department,
        e.name AS Employee,
        e.salary AS Salary,
        DENSE_RANK() OVER (
            PARTITION BY e.departmentId
            ORDER BY e.salary DESC
        ) AS rnk
    FROM Employee e
    JOIN Department d ON e.departmentId = d.id
) t
WHERE rnk <= 3;
```

### LC 1321 — Restaurant Growth

```sql
-- Table: Customer (customer_id, name, visited_on, amount)
-- 7-day rolling average starting from day 7

SELECT
    visited_on,
    SUM(amount) OVER (
        ORDER BY visited_on
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS amount,
    ROUND(
        AVG(amount) OVER (
            ORDER BY visited_on
            ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
        ),
        2
    ) AS average_amount
FROM (
    SELECT visited_on, SUM(amount) AS amount
    FROM Customer
    GROUP BY visited_on
) daily
ORDER BY visited_on
LIMIT 9999 OFFSET 6;  -- Skip first 6 days (need 7 days for rolling window)
```

### LC 585 — Investments in 2016

```sql
-- Find sum of TIV_2016 for policies that:
-- 1. Have same TIV_2015 as another policy
-- 2. Are NOT in the same city as any other policy

SELECT ROUND(SUM(TIV_2016), 2) AS TIV_2016
FROM Insurance
WHERE TIV_2015 IN (
    SELECT TIV_2015 FROM Insurance GROUP BY TIV_2015 HAVING COUNT(*) > 1
)
AND (LAT, LON) IN (
    SELECT LAT, LON FROM Insurance GROUP BY LAT, LON HAVING COUNT(*) = 1
);
```

### LC 1341 — Movie Rating

```sql
-- Tables: Movies (movie_id, title)
--         Users (user_id, name)
--         MovieRating (movie_id, user_id, rating, created_at)

-- Find: user with most ratings (alphabetically if tie)
-- AND: movie with highest avg rating in Feb 2020 (alphabetically if tie)

(
    SELECT u.name AS results
    FROM MovieRating mr
    JOIN Users u ON mr.user_id = u.user_id
    GROUP BY mr.user_id
    ORDER BY COUNT(*) DESC, u.name ASC
    LIMIT 1
)
UNION ALL
(
    SELECT m.title AS results
    FROM MovieRating mr
    JOIN Movies m ON mr.movie_id = m.movie_id
    WHERE DATE_FORMAT(mr.created_at, '%Y-%m') = '2020-02'
    GROUP BY mr.movie_id
    ORDER BY AVG(mr.rating) DESC, m.title ASC
    LIMIT 1
);
```

### Practice Problems — Phase 7

| # | Problem | Topic |
|---|---------|-------|
| 185 | Department Top Three Salaries | DENSE_RANK + PARTITION |
| 1321 | Restaurant Growth | Rolling SUM/AVG |
| 1341 | Movie Rating | UNION ALL + ORDER |
| 585 | Investments in 2016 | Subquery + GROUP BY |
| 184 | Department Highest Salary | RANK / MAX subquery |
| 1907 | Count Salary Categories | CASE WHEN + UNION |
| 1978 | Employees Whose Manager Left | Subquery |

---

# 🔵 Phase 8 — Advanced Patterns

---

## 8.1 UNION and UNION ALL

```sql
-- UNION: combine results, remove duplicates
SELECT name, 'Employee' AS type FROM Employee
UNION
SELECT name, 'Manager' AS type FROM Managers;

-- UNION ALL: combine results, KEEP duplicates (faster!)
SELECT name FROM Employee
UNION ALL
SELECT name FROM Managers;

-- Rules for UNION:
-- 1. Same number of columns in each SELECT
-- 2. Column data types must be compatible
-- 3. Column names taken from FIRST SELECT

-- Full outer join simulation:
SELECT e.name, d.dept_name
FROM Employee e LEFT JOIN Department d ON e.dept_id = d.id
UNION
SELECT e.name, d.dept_name
FROM Employee e RIGHT JOIN Department d ON e.dept_id = d.id;

-- Combining different reports:
SELECT 'Revenue' AS metric, SUM(amount) AS value FROM Orders
UNION ALL
SELECT 'Expenses', SUM(cost) FROM Costs
UNION ALL
SELECT 'Net Profit', SUM(amount) - (SELECT SUM(cost) FROM Costs) FROM Orders;
```

---

## 8.2 CTE (Common Table Expressions)

```sql
-- Basic CTE: like a named subquery, improves readability
WITH dept_avg AS (
    SELECT dept_id, AVG(salary) AS avg_salary
    FROM Employee
    GROUP BY dept_id
)
SELECT e.name, e.salary, da.avg_salary
FROM Employee e
JOIN dept_avg da ON e.dept_id = da.dept_id
WHERE e.salary > da.avg_salary;

-- Multiple CTEs (can reference each other)
WITH
high_value_orders AS (
    SELECT customer_id, SUM(amount) AS total
    FROM Orders
    WHERE year(order_date) = 2023
    GROUP BY customer_id
    HAVING total > 10000
),
customer_info AS (
    SELECT c.id, c.name, hvo.total
    FROM Customers c
    JOIN high_value_orders hvo ON c.id = hvo.customer_id
)
SELECT * FROM customer_info
ORDER BY total DESC;

-- CTE vs Subquery: prefer CTE for readability
-- Subquery (harder to read):
SELECT name FROM Employee WHERE dept_id IN (
    SELECT id FROM Department WHERE (
        SELECT COUNT(*) FROM Employee WHERE dept_id = Department.id
    ) > 10
);

-- CTE (much cleaner):
WITH large_depts AS (
    SELECT dept_id
    FROM Employee
    GROUP BY dept_id
    HAVING COUNT(*) > 10
)
SELECT e.name FROM Employee e
JOIN large_depts ld ON e.dept_id = ld.dept_id;
```

---

## 8.3 Finding Duplicates

```sql
-- Find duplicate email addresses:
SELECT email, COUNT(*) AS count
FROM Users
GROUP BY email
HAVING COUNT(*) > 1;

-- Find all rows with duplicate emails:
SELECT *
FROM Users
WHERE email IN (
    SELECT email FROM Users GROUP BY email HAVING COUNT(*) > 1
);

-- Delete duplicates, keep lowest id:
DELETE FROM Users
WHERE id NOT IN (
    SELECT min_id FROM (
        SELECT MIN(id) AS min_id
        FROM Users
        GROUP BY email
    ) AS keep_ids
);

-- Or using ROW_NUMBER:
DELETE FROM Users
WHERE id IN (
    SELECT id FROM (
        SELECT id,
               ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
        FROM Users
    ) t
    WHERE rn > 1  -- delete all but the first occurrence
);
```

---

## 8.4 Finding Missing Values / Gaps

```sql
-- Find missing IDs in a sequence (gap finding)
SELECT t1.id + 1 AS missing_start
FROM Numbers t1
LEFT JOIN Numbers t2 ON t2.id = t1.id + 1
WHERE t2.id IS NULL
  AND t1.id < (SELECT MAX(id) FROM Numbers);

-- Find employees not in any project (complement pattern)
SELECT e.id, e.name
FROM Employee e
LEFT JOIN ProjectAssignment pa ON e.id = pa.employee_id
WHERE pa.employee_id IS NULL;

-- Find dates with no orders (using a date range table)
WITH RECURSIVE date_range AS (
    SELECT '2024-01-01' AS dt
    UNION ALL
    SELECT DATE_ADD(dt, INTERVAL 1 DAY)
    FROM date_range
    WHERE dt < '2024-12-31'
)
SELECT dr.dt AS missing_date
FROM date_range dr
LEFT JOIN Orders o ON dr.dt = o.order_date
WHERE o.order_date IS NULL;
```

---

## 8.5 Consecutive Rows Pattern

```sql
-- Find consecutive days where a user was active
-- (Using ROW_NUMBER to detect breaks in sequence)

WITH ranked AS (
    SELECT
        user_id,
        login_date,
        ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY login_date) AS rn,
        DATE_SUB(login_date, INTERVAL ROW_NUMBER()
            OVER (PARTITION BY user_id ORDER BY login_date) DAY
        ) AS grp_date  -- same grp_date = consecutive days
    FROM UserLogins
)
SELECT
    user_id,
    MIN(login_date) AS streak_start,
    MAX(login_date) AS streak_end,
    COUNT(*) AS streak_length
FROM ranked
GROUP BY user_id, grp_date
HAVING COUNT(*) >= 3  -- at least 3 consecutive days
ORDER BY user_id, streak_start;

-- Self-join approach for consecutive numbers (3 rows):
SELECT DISTINCT l1.Num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l2.id = l1.id + 1 AND l2.Num = l1.Num
JOIN Logs l3 ON l3.id = l1.id + 2 AND l3.Num = l1.Num;
```

---

## 8.6 Pivot / Transpose Data

```sql
-- Transform rows into columns (pivot)
-- Original: (department, month, sales)
-- Desired: (department, Jan, Feb, Mar)

SELECT
    department,
    SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS Jan,
    SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS Feb,
    SUM(CASE WHEN month = 'Mar' THEN sales ELSE 0 END) AS Mar
FROM MonthlySales
GROUP BY department;

-- LC 1179 style (Reformat Department Table):
SELECT id,
    SUM(CASE WHEN month = 'Jan' THEN revenue END) AS Jan_Revenue,
    SUM(CASE WHEN month = 'Feb' THEN revenue END) AS Feb_Revenue,
    SUM(CASE WHEN month = 'Mar' THEN revenue END) AS Mar_Revenue
FROM Department
GROUP BY id
ORDER BY id;
```

---

## 8.7 Ranking and Nth Value Patterns

```sql
-- Nth highest salary (works for any N)
SELECT DISTINCT salary AS NthHighestSalary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;  -- OFFSET is 0-based, N-1 skips N-1 rows

-- Top N per group (most important pattern for interviews!)
SELECT dept, name, salary
FROM (
    SELECT
        dept, name, salary,
        DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS rnk
    FROM Employee
) ranked
WHERE rnk <= 3;  -- change 3 to any N

-- Median salary per department
SELECT dept, AVG(salary) AS median
FROM (
    SELECT
        dept, salary,
        ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary) AS rn,
        COUNT(*) OVER (PARTITION BY dept) AS cnt
    FROM Employee
) t
WHERE rn IN (FLOOR((cnt + 1) / 2), CEIL((cnt + 1) / 2))
GROUP BY dept;

-- Percentile rank
SELECT
    name, salary,
    ROUND(
        PERCENT_RANK() OVER (ORDER BY salary) * 100,
        1
    ) AS percentile
FROM Employee;
```

---

## Solved LC Problems — Phase 8

### LC 177 — Nth Highest Salary

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    DECLARE offset_val INT;
    SET offset_val = N - 1;
    RETURN (
        SELECT DISTINCT salary
        FROM Employee
        ORDER BY salary DESC
        LIMIT 1 OFFSET offset_val
    );
END;
```

### LC 601 — Human Traffic of Stadium

```sql
-- Find consecutive 3+ days with people >= 100

WITH valid_days AS (
    SELECT id, visit_date, people
    FROM Stadium
    WHERE people >= 100
),
grouped AS (
    SELECT *,
        id - ROW_NUMBER() OVER (ORDER BY id) AS grp
    FROM valid_days
)
SELECT id, visit_date, people
FROM grouped
WHERE grp IN (
    SELECT grp FROM grouped GROUP BY grp HAVING COUNT(*) >= 3
)
ORDER BY visit_date;
```

### LC 1179 — Reformat Department Table

```sql
-- Table: Department (id, revenue, month)
-- Pivot: rows of months become columns

SELECT
    id,
    SUM(IF(month='Jan', revenue, NULL)) AS Jan_Revenue,
    SUM(IF(month='Feb', revenue, NULL)) AS Feb_Revenue,
    SUM(IF(month='Mar', revenue, NULL)) AS Mar_Revenue,
    SUM(IF(month='Apr', revenue, NULL)) AS Apr_Revenue,
    SUM(IF(month='May', revenue, NULL)) AS May_Revenue,
    SUM(IF(month='Jun', revenue, NULL)) AS Jun_Revenue,
    SUM(IF(month='Jul', revenue, NULL)) AS Jul_Revenue,
    SUM(IF(month='Aug', revenue, NULL)) AS Aug_Revenue,
    SUM(IF(month='Sep', revenue, NULL)) AS Sep_Revenue,
    SUM(IF(month='Oct', revenue, NULL)) AS Oct_Revenue,
    SUM(IF(month='Nov', revenue, NULL)) AS Nov_Revenue,
    SUM(IF(month='Dec', revenue, NULL)) AS Dec_Revenue
FROM Department
GROUP BY id
ORDER BY id;
```

### LC 1667 — Fix Names in a Table

```sql
SELECT
    user_id,
    CONCAT(UPPER(LEFT(name,1)), LOWER(SUBSTR(name,2))) AS name
FROM Users
ORDER BY user_id;
```

### Practice Problems — Phase 8

| # | Problem | Topic |
|---|---------|-------|
| 177 | Nth Highest Salary | LIMIT OFFSET |
| 601 | Human Traffic of Stadium | Consecutive rows |
| 1179 | Reformat Department Table | PIVOT with CASE/IF |
| 196 | Delete Duplicate Emails | DELETE with subquery |
| 180 | Consecutive Numbers | Self JOIN consecutive |
| 1907 | Count Salary Categories | CASE WHEN + UNION |
| 1097 | Game Play Analysis V | Complex aggregation |

---

# 📋 Master Pattern Decision Table

```
PROBLEM SIGNAL                              → SQL PATTERN
──────────────────────────────────────────────────────────────────
"Find all X" simple filter                  → SELECT ... WHERE
"Find X with condition on Y"                → JOIN or subquery
"Customers with NO orders"                  → LEFT JOIN + IS NULL
                                              OR NOT IN (...)
                                              OR NOT EXISTS (...)
"Compare employee to their manager"         → SELF JOIN
"Average/sum/count per group"               → GROUP BY + aggregate
"Filter groups by aggregate"                → GROUP BY + HAVING
"Rank items within a group"                 → DENSE_RANK() OVER (PARTITION BY)
"Top N per group"                           → DENSE_RANK + WHERE rnk <= N
"Running total / cumulative sum"            → SUM() OVER (ORDER BY)
"Compare current row to previous"           → LAG() OVER (ORDER BY)
"Compare current row to next"               → LEAD() OVER (ORDER BY)
"Nth highest value"                         → ORDER BY DESC LIMIT 1 OFFSET N-1
"Second highest salary"                     → MAX() WHERE < (MAX())
                                              OR LIMIT 1 OFFSET 1
"Consecutive rows / streaks"                → ROW_NUMBER gap method
                                              OR self-join on id+1
"Duplicate rows"                            → GROUP BY HAVING COUNT > 1
"Missing from list"                         → LEFT JOIN + IS NULL
"Same day comparison (temperature)"         → SELF JOIN on DATEDIFF = 1
"Ratio / percentage"                        → COUNT(*) * 100 / total
"Pivot rows to columns"                     → SUM(CASE WHEN month='Jan' THEN...)
"NULL replace default"                      → IFNULL() or COALESCE()
"Conditional aggregation"                   → SUM(IF(condition, val, 0))
"Multiple result sets combined"             → UNION / UNION ALL
"Complex multi-step query"                  → CTE (WITH clause)
"All combinations of two tables"            → CROSS JOIN
"Date range / month grouping"               → DATE_FORMAT or YEAR() + MONTH()
```

---

# 📚 All LC SQL Problems by Category

### Easy — Start Here

| # | Problem | Key Concept |
|---|---------|------------|
| 595 | Big Countries | WHERE with OR |
| 1757 | Recyclable and Low Fat Products | WHERE AND |
| 584 | Find Customer Referee | IS NULL |
| 183 | Customers Who Never Order | LEFT JOIN / NOT IN |
| 1148 | Article Views I | DISTINCT |
| 1683 | Invalid Tweets | CHAR_LENGTH |
| 620 | Not Boring Movies | WHERE + ORDER BY |
| 1068 | Product Sales Analysis I | LEFT JOIN |
| 1075 | Project Employees I | JOIN + AVG |
| 1280 | Students and Examinations | CROSS JOIN + LEFT JOIN |
| 577 | Employee Bonus | LEFT JOIN + IS NULL |
| 1661 | Average Time of Process per Machine | Self JOIN |
| 197 | Rising Temperature | Self JOIN + DATEDIFF |
| 1251 | Average Selling Price | JOIN + AVG |
| 1633 | Percentage of Users Attended Contest | COUNT + % |

### Medium — Core Patterns

| # | Problem | Key Concept |
|---|---------|------------|
| 176 | Second Highest Salary | LIMIT OFFSET |
| 180 | Consecutive Numbers | Self JOIN |
| 184 | Department Highest Salary | GROUP BY + JOIN |
| 185 | Department Top 3 Salaries | DENSE_RANK |
| 178 | Rank Scores | DENSE_RANK |
| 626 | Exchange Seats | CASE WHEN + MOD |
| 1393 | Capital Gain/Loss | Conditional SUM |
| 1907 | Count Salary Categories | CASE WHEN + UNION |
| 1321 | Restaurant Growth | Rolling SUM OVER |
| 1341 | Movie Rating | UNION ALL |
| 1174 | Immediate Food Delivery II | MIN + conditional |
| 1193 | Monthly Transactions I | DATE_FORMAT + GROUP BY |
| 1934 | Confirmation Rate | LEFT JOIN + AVG |
| 550 | Game Play Analysis IV | MIN date + DATEDIFF |
| 570 | Managers with 5+ Direct Reports | GROUP BY HAVING |
| 1179 | Reformat Department Table | PIVOT |
| 1978 | Employees Whose Manager Left | Subquery |

### Hard — Advanced

| # | Problem | Key Concept |
|---|---------|------------|
| 185 | Department Top 3 Salaries | Window functions |
| 601 | Human Traffic of Stadium | Consecutive + CTE |
| 585 | Investments in 2016 | Complex subqueries |
| 262 | Trips and Users | Multi-condition JOIN |
| 177 | Nth Highest Salary | Dynamic LIMIT |
| 1097 | Game Play Analysis V | Complex aggregation |

---

*Updated: 2026-06-18 | MySQL | LeetCode SQL 50 + Top Interview*
*All patterns: SELECT · WHERE · JOIN · Aggregate · Window Functions · Subqueries · CTE · Advanced*
