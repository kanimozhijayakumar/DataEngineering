### **Detailed Explanation of Percentile Functions in SQL**
Percentile functions help determine the relative ranking of a row **within a partition** or **across the entire dataset**. They are useful for **statistical analysis, salary distribution, and performance rankings**.

---

## **1. `PERCENT_RANK()`**
- **Formula**:  
  \[
  PERCENT\_RANK = \frac{\text{rank} - 1}{\text{total rows} - 1}
  \]
- **Values Range**: `0 to 1`
- **Purpose**: Determines the relative percentile of a row in the ordered dataset.

### **Example: Calculate Salary Percentile Rank**
```sql
SELECT 
    emp_id, 
    emp_name, 
    salary,
    PERCENT_RANK() OVER (ORDER BY salary ASC) AS percentile_rank
FROM emp;
```

### **Example Output**
| emp_id | emp_name | salary | percentile_rank |
|--------|---------|--------|----------------|
| 5      | Eve     | 7000   | 0.0000         |
| 4      | David   | 7500   | 0.2500         |
| 3      | Charlie | 8500   | 0.5000         |
| 2      | Bob     | 9000   | 0.7500         |
| 1      | Alice   | 10000  | 1.0000         |

### **Explanation**
- `PERCENT_RANK()` returns the **relative position** of a row in the dataset.
- **Lowest salary (Eve, $7000)** gets `0.0000`, **highest salary (Alice, $10000)** gets `1.0000`.
- **David ($7500) has a percentile rank of 0.25**, meaning **he earns more than 25% of employees**.

---

## **2. `CUME_DIST()` (Cumulative Distribution)**
- **Formula**:  
  \[
  CUME\_DIST = \frac{\text{Number of rows with value ≤ current row}}{\text{Total rows}}
  \]
- **Values Range**: `0 < CUME_DIST ≤ 1`
- **Purpose**: Shows **how many employees have a salary equal to or less than the current employee**.

### **Example: Calculate Cumulative Distribution**
```sql
SELECT 
    emp_id, 
    emp_name, 
    salary,
    CUME_DIST() OVER (ORDER BY salary ASC) AS cumulative_distribution
FROM emp;
```

### **Example Output**
| emp_id | emp_name | salary | cumulative_distribution |
|--------|---------|--------|------------------------|
| 5      | Eve     | 7000   | 0.2000                 |
| 4      | David   | 7500   | 0.4000                 |
| 3      | Charlie | 8500   | 0.6000                 |
| 2      | Bob     | 9000   | 0.8000                 |
| 1      | Alice   | 10000  | 1.0000                 |

### **Explanation**
- **CUME_DIST() represents the percentage of employees with salary ≤ current row.**
- **Alice (10000)** has **1.0000**, meaning **100% of employees earn ≤ 10000**.
- **Charlie (8500)** has **0.6000**, meaning **60% of employees earn ≤ 8500**.
- **Eve (7000)** has **0.2000**, meaning **only 20% of employees earn ≤ 7000**.

---

## **3. Using `NTILE()` for Percentile Buckets**
- `NTILE(n)` divides data into **n equal groups**.
- Each row is assigned to a **percentile bucket**.

### **Example: Divide Employees into 4 Salary Quartiles**
```sql
SELECT 
    emp_id, 
    emp_name, 
    salary,
    NTILE(4) OVER (ORDER BY salary ASC) AS quartile
FROM emp;
```

### **Example Output**
| emp_id | emp_name | salary | quartile |
|--------|---------|--------|---------|
| 5      | Eve     | 7000   | 1       |
| 4      | David   | 7500   | 1       |
| 3      | Charlie | 8500   | 2       |
| 2      | Bob     | 9000   | 3       |
| 1      | Alice   | 10000  | 4       |

### **Explanation**
- **Lowest 25% (Quartile 1)**: `Eve (7000), David (7500)`
- **25% - 50% (Quartile 2)**: `Charlie (8500)`
- **50% - 75% (Quartile 3)**: `Bob (9000)`
- **75% - 100% (Quartile 4)**: `Alice (10000)`

✅ **`NTILE(4)` is useful for breaking data into quartiles, deciles, or any equal-sized percentile groups.**

---

## **Summary: When to Use Each Percentile Function**
| Function | Purpose | Value Range |
|----------|---------|-------------|
| `PERCENT_RANK()` | Shows relative ranking **excluding duplicates** | `0 to 1` |
| `CUME_DIST()` | Shows **cumulative percentage** (fraction ≤ current row) | `> 0 and ≤ 1` |
| `NTILE(n)` | Groups data into **equal-sized buckets** | `1 to n` |

🚀 **Use `PERCENT_RANK()` to determine relative ranking.**  
🚀 **Use `CUME_DIST()` to see percentage of rows ≤ current row.**  
🚀 **Use `NTILE(n)` to create quartiles, deciles, or percentiles.**  
