### **Detailed Examples of Window Functions for Beginners**
---
Window functions allow you to perform calculations across a subset of rows related to the current row without collapsing them like `GROUP BY`. Below are beginner-friendly examples.

---

https://www.tutorialspoint.com/execute_sql_online.php

### **1. RANKING FUNCTIONS**
#### **1.1 `RANK()` - Assigns a Rank with Skips**
Ranks employees by salary in descending order. If two employees have the same salary, they get the same rank, and the next rank is skipped.

```sql
SELECT emp_id, emp_name, salary,
       RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | salary_rank |
|--------|---------|--------|-------------|
| 101    | Alice   | 8000   | 1           |
| 102    | Bob     | 8000   | 1           |
| 103    | Charlie | 7500   | 3           |
| 104    | David   | 7000   | 4           |

🎯 **Key Point:** Rank 2 is skipped since two employees share rank 1.

---

#### **1.2 `DENSE_RANK()` - Assigns a Rank without Skips**
Similar to `RANK()`, but **does not skip** numbers.

```sql
SELECT emp_id, emp_name, salary,
       DENSE_RANK() OVER (ORDER BY salary DESC) AS salary_dense_rank
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | salary_dense_rank |
|--------|---------|--------|-------------------|
| 101    | Alice   | 8000   | 1                 |
| 102    | Bob     | 8000   | 1                 |
| 103    | Charlie | 7500   | 2                 |
| 104    | David   | 7000   | 3                 |

🎯 **Key Point:** No rank is skipped.

---

#### **1.3 `ROW_NUMBER()` - Assigns Unique Numbers**
Assigns a unique row number in order.

```sql
SELECT emp_id, emp_name, salary,
       ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | row_num |
|--------|---------|--------|---------|
| 101    | Alice   | 8000   | 1       |
| 102    | Bob     | 8000   | 2       |
| 103    | Charlie | 7500   | 3       |
| 104    | David   | 7000   | 4       |

🎯 **Key Point:** Unlike `RANK()`, it does not repeat numbers.

---

### **2. AGGREGATE WINDOW FUNCTIONS**
#### **2.1 `SUM()` - Running Total**
Computes cumulative salary.

```sql
SELECT emp_id, emp_name, salary,
       SUM(salary) OVER (ORDER BY emp_id) AS running_total
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | running_total |
|--------|---------|--------|---------------|
| 101    | Alice   | 8000   | 8000          |
| 102    | Bob     | 8000   | 16000         |
| 103    | Charlie | 7500   | 23500         |
| 104    | David   | 7000   | 30500         |

🎯 **Key Point:** Each row includes the sum of all previous rows.

---

#### **2.2 `AVG()` - Rolling Average**
Calculates average salary **up to** the current row.

```sql
SELECT emp_id, emp_name, salary,
       AVG(salary) OVER (ORDER BY emp_id) AS rolling_avg
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | rolling_avg |
|--------|---------|--------|-------------|
| 101    | Alice   | 8000   | 8000.00     |
| 102    | Bob     | 8000   | 8000.00     |
| 103    | Charlie | 7500   | 7833.33     |
| 104    | David   | 7000   | 7625.00     |

🎯 **Key Point:** The average updates as more rows are processed.

---

### **3. VALUE-BASED (ANALYTIC) FUNCTIONS**
#### **3.1 `LAG()` - Previous Row Value**
Gets the salary of the **previous employee**.

```sql
SELECT emp_id, emp_name, salary,
       LAG(salary, 1, 0) OVER (ORDER BY salary DESC) AS prev_salary
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | prev_salary |
|--------|---------|--------|-------------|
| 101    | Alice   | 8000   | 0           |
| 102    | Bob     | 8000   | 8000        |
| 103    | Charlie | 7500   | 8000        |
| 104    | David   | 7000   | 7500        |

🎯 **Key Point:** The first row gets `0` as there is no previous row.

---

#### **3.2 `LEAD()` - Next Row Value**
Gets the salary of the **next employee**.

```sql
SELECT emp_id, emp_name, salary,
       LEAD(salary, 1, 0) OVER (ORDER BY salary DESC) AS next_salary
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | next_salary |
|--------|---------|--------|-------------|
| 101    | Alice   | 8000   | 8000        |
| 102    | Bob     | 8000   | 7500        |
| 103    | Charlie | 7500   | 7000        |
| 104    | David   | 7000   | 0           |

🎯 **Key Point:** The last row gets `0` since there's no next row.

---

### **4. PERCENTILE FUNCTIONS**
#### **4.1 `PERCENT_RANK()` - Relative Rank Between 0 and 1**
Determines how a salary ranks compared to others.

```sql
SELECT emp_id, emp_name, salary,
       PERCENT_RANK() OVER (ORDER BY salary DESC) AS percentile_rank
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | salary | percentile_rank |
|--------|---------|--------|-----------------|
| 101    | Alice   | 8000   | 0.0             |
| 102    | Bob     | 8000   | 0.0             |
| 103    | Charlie | 7500   | 0.5             |
| 104    | David   | 7000   | 1.0             |

🎯 **Key Point:** The highest salary has `0.0`, the lowest has `1.0`.

---

### **5. PARTITIONING DATA**
We can **group results** using `PARTITION BY`.

```sql
SELECT emp_id, emp_name, dept_id, salary,
       RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dept_rank
FROM emp;
```

✅ **Example Output:**
| emp_id | emp_name | dept_id | salary | dept_rank |
|--------|---------|--------|---------|-----------|
| 101    | Alice   | 10     | 8000    | 1         |
| 102    | Bob     | 10     | 7500    | 2         |
| 103    | Charlie | 20     | 9000    | 1         |
| 104    | David   | 20     | 7000    | 2         |

🎯 **Key Point:** Ranks reset within each `dept_id`.

---

## **Conclusion**
- **Ranking:** `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`
- **Aggregates:** `SUM()`, `AVG()`, `COUNT()`
- **Comparison:** `LAG()`, `LEAD()`
- **Percentile:** `PERCENT_RANK()`, `CUME_DIST()`
- **Partitioning:** Groups data using `PARTITION BY`
