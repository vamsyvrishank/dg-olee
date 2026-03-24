---
{"dg-publish":true,"permalink":"/personal-vault/interview-prep/sql-interview-prep/"}
---


**Patterns**

###### Filtering and Conditionals 

**Concepts:** WHERE, HAVING, CASE WHEN, BETWEEN, IN, LIKE, IS NULL

- Filter rows by a condition
- Filter after aggregation
- Categorize rows into buckets
- Handle NULL values gracefully
- Pattern match on strings

```sql
-- Basic filter
WHERE salary > 50000 AND department = 'Engineering'

-- Filter after aggregation
HAVING COUNT(*) >= 5

-- Bucket rows
CASE WHEN salary > 100000 THEN 'high'
     WHEN salary > 50000  THEN 'mid'
     ELSE                      'low' END

-- NULL safe filter
WHERE column IS NULL
WHERE column IS NOT NULL
COALESCE(column, 0)

-- String match
WHERE name LIKE 'A%'        -- starts with A
WHERE name LIKE '%son'      -- ends with son
WHERE name LIKE '%an%'      -- contains an
```


###### Aggregations 

**Concepts:** COUNT, SUM, AVG, MAX, MIN, STDDEV, GROUP BY, HAVING

- Count rows per group
- Total/average value per group
- Find groups above a threshold
- Conditional counting
- Pivot data using CASE inside aggregates

```sql

-- Standard aggregation
SELECT department, COUNT(*), SUM(salary), AVG(salary)
FROM   Employee
GROUP BY department
HAVING COUNT(*) >= 5

-- COUNT(*) vs COUNT(col)
COUNT(*)          -- counts all rows including NULLs
COUNT(column)     -- skips NULLs

-- Conditional aggregation (pivot)
SELECT department,
  SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
  SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count,
  AVG(CASE WHEN level = 'senior' THEN salary END) AS avg_senior_sal
FROM Employee
GROUP BY department

-- Percentage of total within group
SELECT department,
  COUNT(*) * 100.0 / SUM(COUNT(*)) OVER () AS pct_of_company
FROM Employee
GROUP BY department
```


###### Joins 

**Concepts:** INNER, LEFT, RIGHT, FULL OUTER, SELF, CROSS, ANTI-JOIN

- Combine data from two tables
- Find records that exist in one table but not another
- Hierarchy/org chart problems
- All combinations of two sets
- Reconciliation between two systems

```sql
-- INNER: only matching rows
SELECT a.*, b.*
FROM TableA a
JOIN TableB b ON a.id = b.id

-- LEFT: keep all of left, NULL where no match
FROM TableA a
LEFT JOIN TableB b ON a.id = b.id

-- ANTI-JOIN: rows in A with no match in B
FROM TableA a
LEFT JOIN TableB b ON a.id = b.id
WHERE b.id IS NULL

-- SELF JOIN: table referencing itself
SELECT e.name AS employee, m.name AS manager
FROM   Employee e
LEFT JOIN Employee m ON e.managerId = m.id

-- SELF JOIN for pairs (avoid duplicates)
FROM Employee a
JOIN Employee b ON a.department = b.department
               AND a.id < b.id

-- CROSS JOIN: every combination
FROM TableA
CROSS JOIN TableB
```


###### Deduplication

**Concepts:** ROW_NUMBER, DISTINCT, GROUP BY + HAVING COUNT > 1

- Find duplicate records
- Keep only the latest record per group
- Remove duplicates keeping one row
- Find users with multiple entries

```sql
-- Find duplicates
SELECT user_id, COUNT(*)
FROM Orders
GROUP BY user_id
HAVING COUNT(*) > 1

-- Show all duplicate rows
SELECT * FROM (
  SELECT *,
    ROW_NUMBER() OVER (
      PARTITION BY user_id
      ORDER BY created_at DESC
    ) AS rn
  FROM Orders
) x WHERE rn > 1

-- Keep latest record per user (dedup)
SELECT * FROM (
  SELECT *,
    ROW_NUMBER() OVER (
      PARTITION BY user_id
      ORDER BY created_at DESC
    ) AS rn
  FROM Orders
) x WHERE rn = 1

-- DISTINCT
SELECT DISTINCT user_id, department FROM Employee
```


######  Window Functions  ( Non Ranking)

**Concepts:** SUM OVER, AVG OVER, COUNT OVER, frame clauses

- Running totals
- Rolling averages
- Compare row to group average
- Cumulative percentage
- Moving sum over last N days

```sql
-- Running total
SUM(value) OVER (
  ORDER BY date
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)

-- Rolling 7-day average
AVG(value) OVER (
  ORDER BY date
  ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
)

-- Compare each row to its group average
SELECT name, salary,
  AVG(salary) OVER (PARTITION BY department) AS dept_avg,
  salary - AVG(salary) OVER (PARTITION BY department) AS diff
FROM Employee

-- % of group total
salary * 100.0 / SUM(salary) OVER (PARTITION BY department)

-- % of grand total
salary * 100.0 / SUM(salary) OVER ()

-- Running count
COUNT(*) OVER (PARTITION BY dept ORDER BY hire_date)

-- Frame options
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  -- start to now
ROWS BETWEEN 6 PRECEDING AND CURRENT ROW          -- last 7 rows
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING          -- centered window
ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING  -- whole partition
```

###### LAG / LEAD (Row Comparison)

**Concepts:** LAG, LEAD, offset, default value

- Day-over-day / month-over-month change
- Period-over-period % change
- Detect consecutive events
- Find gaps in a sequence
- Time between events
- Session detection

```sql
-- Previous row value
LAG(price)     OVER (ORDER BY date)
LAG(price, 1)  OVER (ORDER BY date)   -- same thing

-- Next row value
LEAD(price) OVER (ORDER BY date)

-- With default (avoid NULL on first/last row)
LAG(price, 1, 0) OVER (ORDER BY date)

-- Day-over-day change
price - LAG(price) OVER (ORDER BY date) AS daily_change

-- % change
(price - LAG(price) OVER (ORDER BY date))
* 100.0
/ LAG(price) OVER (ORDER BY date) AS pct_change

-- Per group (always PARTITION BY for multiple groups)
LAG(price) OVER (PARTITION BY stock ORDER BY date)

-- Gap detection
DATE_DIFF(date, LAG(date) OVER (ORDER BY date), DAY) AS gap_days

-- Consecutive events
CASE WHEN action = 'fail'
      AND LEAD(action) OVER (ORDER BY date) = 'fail'
     THEN 'consecutive failure' END
```


###### Subqueries & CTEs

**Concepts:** WITH, inline subquery, scalar subquery, EXISTS, IN, correlated subquery

**Problems you'll see:**

- Multi-step logic
- Filter using aggregate from another query
- Check existence in another table
- Reuse intermediate results
- Anything needing more than one SELECT

```sql
-- Scalar subquery
WHERE salary > (SELECT AVG(salary) FROM Employee)

-- CTE (prefer over nested subqueries)
WITH dept_avg AS (
  SELECT department, AVG(salary) AS avg_sal
  FROM Employee GROUP BY department
)
SELECT e.*, d.avg_sal
FROM Employee e
JOIN dept_avg d ON e.department = d.department
WHERE e.salary > d.avg_sal

-- Chained CTEs
WITH step1 AS (...),
     step2 AS (SELECT ... FROM step1),
     step3 AS (SELECT ... FROM step2)
SELECT * FROM step3

-- EXISTS (faster than IN for large tables)
WHERE EXISTS (
  SELECT 1 FROM Orders o
  WHERE o.user_id = u.id
)

-- NOT EXISTS (anti-join alternative)
WHERE NOT EXISTS (
  SELECT 1 FROM Orders o
  WHERE o.user_id = u.id
)

-- IN
WHERE id IN (SELECT user_id FROM Orders)

-- Correlated subquery (references outer query)
WHERE salary > (
  SELECT AVG(salary) FROM Employee e2
  WHERE e2.department = e1.department
)
```


###### Date & Time Problems

**Concepts:** DATE_TRUNC, DATE_DIFF, EXTRACT, BETWEEN, date arithmetic

**Problems you'll see:**

- Group by month/year
- Days between two dates
- Filter last 30/90 days
- Year-over-year comparison
- Find events within a time window

```sql

-- Truncate to month
DATE_TRUNC(date_col, MONTH)

-- Extract part
EXTRACT(YEAR FROM date_col)
EXTRACT(MONTH FROM date_col)
EXTRACT(DOW FROM date_col)    -- day of week

-- Date difference
DATE_DIFF(end_date, start_date, DAY)
DATE_DIFF(end_date, start_date, MONTH)

-- Last 30 days
WHERE date_col >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)

-- Group by month with LAG for MoM change
WITH monthly AS (
  SELECT DATE_TRUNC(date, MONTH) AS month, SUM(revenue) AS rev
  FROM sales GROUP BY 1
)
SELECT month, rev,
  LAG(rev) OVER (ORDER BY month) AS prev_month,
  rev - LAG(rev) OVER (ORDER BY month) AS change
FROM monthly

```

###### String Problems

**Concepts:** LIKE, SUBSTRING, CONCAT, UPPER, LOWER, LENGTH, TRIM, REPLACE

**Problems you'll see:**

- Filter by string pattern
- Extract part of a string
- Clean dirty string data
- Combine columns into one string

```sql
-- Pattern matching
WHERE email LIKE '%@gmail.com'
WHERE name LIKE 'A%'
WHERE code LIKE '__-___'         -- _ = exactly one character

-- Extract
SUBSTRING(email, 1, POSITION('@' IN email) - 1) AS username
LEFT(phone, 3)   AS area_code
RIGHT(code, 4)   AS last_four

-- Clean
TRIM(name)
UPPER(name), LOWER(name)
REPLACE(phone, '-', '')

-- Combine
CONCAT(first_name, ' ', last_name)
first_name || ' ' || last_name    -- PostgreSQL
```

###### Set Operations

**Concepts:** UNION, UNION ALL, INTERSECT, EXCEPT

**Problems you'll see:**

- Combine results from two queries
- Find records in both tables
- Find records in one but not the other

```sql
-- UNION: combine, remove duplicates (slow)
SELECT id FROM TableA
UNION
SELECT id FROM TableB

-- UNION ALL: combine, keep duplicates (fast — prefer this)
SELECT id, 'A' AS source FROM TableA
UNION ALL
SELECT id, 'B' AS source FROM TableB

-- INTERSECT: rows in both
SELECT id FROM TableA
INTERSECT
SELECT id FROM TableB

-- EXCEPT: rows in A but not B (like anti-join)
SELECT id FROM TableA
EXCEPT
SELECT id FROM TableB
```

###### Performance & Optimization

**Concepts:** indexes, query plan, early filtering, avoiding full scans

**Problems you'll see:**

- "Why is this query slow?"
- "How would you optimize this?"
- "What's wrong with this query?"

```sql
-- Filter early, reduce rows before joining
WHERE status = 'active'     -- put this before the JOIN processes everything

-- Avoid functions on indexed columns (kills the index)
WHERE YEAR(created_at) = 2024         -- bad
WHERE created_at >= '2024-01-01'      -- good

-- UNION ALL over UNION when duplicates don't matter
-- EXISTS over IN for large subqueries
-- Avoid SELECT * in production
-- Use EXPLAIN to see query execution plan

EXPLAIN SELECT ...    -- shows full table scans, index usage
```

##### Master Cheat Sheet : Problem to Pattern

```
Top N overall                    → ORDER BY + LIMIT
Top N per group                  → ROW_NUMBER + PARTITION BY + WHERE rn <= N
Nth highest value                → ORDER BY DESC LIMIT 1 OFFSET N-1
Rank with ties                   → RANK or DENSE_RANK
Duplicates — find them           → GROUP BY + HAVING COUNT > 1
Duplicates — remove them         → ROW_NUMBER + WHERE rn = 1
Unmatched rows                   → LEFT JOIN + WHERE right IS NULL
Rows in A not in B               → EXCEPT or LEFT JOIN + IS NULL
Running total                    → SUM OVER (UNBOUNDED PRECEDING)
Rolling average                  → AVG OVER (N PRECEDING)
Period change                    → LAG + subtraction
% change                         → (current - LAG) / LAG * 100
Compare row to group avg         → AVG OVER (PARTITION BY group)
% of group total                 → value / SUM OVER (PARTITION BY group)
% of grand total                 → value / SUM OVER ()
Consecutive events               → LEAD to look at next row
Gap in sequence                  → DATE_DIFF with LAG
Multi-step logic                 → CTE chain
Existence check                  → EXISTS
Conditional count                → SUM(CASE WHEN ... THEN 1 ELSE 0 END)
Pivot rows to columns            → SUM(CASE WHEN type = X THEN val END)
Hierarchy / org chart            → SELF JOIN
Quartiles / percentiles          → NTILE(4)
Above average filter             → WHERE val > (SELECT AVG(val) FROM t)
```
