Window functions in SQL operate on a set of rows related to the current row and return a value for each row. They do **not** collapse rows like aggregate functions; instead, they provide results while preserving individual rows.

https://www.tutorialspoint.com/execute_sql_online.php

### **Types of Window Functions**
---
#### **1. Ranking Functions**
   These functions assign a rank to each row within a partition.
   - `RANK()`: Assigns a rank to each row, skipping ranks for duplicates.
   - `DENSE_RANK()`: Assigns a rank but **does not skip** numbers for duplicates.
   - `ROW_NUMBER()`: Assigns a unique number to each row without gaps.
   - `NTILE(n)`: Divides rows into `n` buckets and assigns a bucket number.

   **Example:**
   ```sql
   SELECT emp_id, emp_name, salary,
          RANK() OVER (ORDER BY salary DESC) AS rank,
          DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank,
          ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
   FROM emp;
   ```

---

#### **2. Aggregate Window Functions**
   These apply aggregate calculations over a defined partition/window.
   - `SUM()`
   - `AVG()`
   - `COUNT()`
   - `MIN()`
   - `MAX()`

   **Example: Running total of salaries**
   ```sql
   SELECT emp_id, emp_name, salary,
          SUM(salary) OVER (ORDER BY emp_id) AS running_total
   FROM emp;
   ```

---

#### **3. Value-Based (Analytic) Functions**
   These help compare rows within the window.
   - `LAG(column, offset, default)`: Fetches the **previous** row’s value.
   - `LEAD(column, offset, default)`: Fetches the **next** row’s value.
   - `FIRST_VALUE(column)`: Gets the first row’s value in the partition.
   - `LAST_VALUE(column)`: Gets the last row’s value in the partition.
   - `NTH_VALUE(column, n)`: Gets the **n-th** row’s value in the partition.

   **Example: Compare salary with the previous row**
   ```sql
   SELECT emp_id, emp_name, salary,
          LAG(salary, 1, 0) OVER (ORDER BY salary DESC) AS prev_salary
   FROM emp;
   ```

---

#### **4. Percentile and Statistical Functions**
   - `PERCENT_RANK()`: Computes relative rank between `0` and `1`.
   - `CUME_DIST()`: Cumulative distribution from `0` to `1`.
   - `NTILE(n)`: Divides data into `n` equal parts.

   **Example:**
   ```sql
   SELECT emp_id, emp_name, salary,
          PERCENT_RANK() OVER (ORDER BY salary DESC) AS percentile
   FROM emp;
   ```

---

### **Understanding `OVER()` Clause**
The `OVER()` clause defines the window of rows for the function.
- `PARTITION BY column`: Divides the data into groups.
- `ORDER BY column`: Specifies the sorting order.
- `ROWS BETWEEN`: Defines the window range.

Example:
```sql
SELECT emp_id, emp_name, dept_id, salary,
       SUM(salary) OVER (PARTITION BY dept_id ORDER BY emp_id ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS rolling_sum
FROM emp;
```
---
