To find duplicate records in a table, you can use the `GROUP BY` clause along with `HAVING COUNT(*) > 1`. Below is an example SQL query to find duplicates in a table.

### Example Table: `employees`
```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    email VARCHAR(100)
);

INSERT INTO employees (name, department, email) VALUES
('Alice', 'HR', 'alice@example.com'),
('Bob', 'IT', 'bob@example.com'),
('Alice', 'HR', 'alice@example.com'),
('Charlie', 'Finance', 'charlie@example.com'),
('Bob', 'IT', 'bob@example.com'),
('David', 'IT', 'david@example.com');
```

### Finding Duplicates:
If we consider duplicates as having the same `name`, `department`, and `email`, we can use:

```sql
SELECT name, department, email, COUNT(*)
FROM employees
GROUP BY name, department, email
HAVING COUNT(*) > 1;
```

### Output:
```
 name   | department |       email        | count
--------+-----------+--------------------+-------
 Alice  | HR        | alice@example.com  | 2
 Bob    | IT        | bob@example.com    | 2
```
This query groups records by `name`, `department`, and `email` and then filters those that appear more than once.

---

### **1. Using `DISTINCT` with `JOIN`**
```sql
SELECT e1.*
FROM employees e1
JOIN employees e2
ON e1.name = e2.name
AND e1.department = e2.department
AND e1.email = e2.email
AND e1.id > e2.id;
```
**Explanation:**  
- This self-join finds duplicate rows based on `name`, `department`, and `email`, where the `id` of one row is greater than another (avoiding self-matching).
- It returns only the duplicate rows excluding the first occurrence.

---

### **2. Using `ROW_NUMBER()`**
If your database supports `ROW_NUMBER()`, this approach can help find and rank duplicate records.

```sql
WITH cte AS (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY name, department, email ORDER BY id) AS rn
    FROM employees
)
SELECT * FROM cte WHERE rn > 1;
```
**Explanation:**
- The `PARTITION BY` groups by `name`, `department`, and `email`.
- The `ROW_NUMBER()` assigns a unique rank for each duplicate set.
- We filter for `rn > 1` to retrieve only duplicates.

---

### **3. Using `EXISTS`**
```sql
SELECT * FROM employees e1
WHERE EXISTS (
    SELECT 1 FROM employees e2
    WHERE e1.name = e2.name
    AND e1.department = e2.department
    AND e1.email = e2.email
    AND e1.id > e2.id
);
```
**Explanation:**
- The subquery checks if a record exists with the same `name`, `department`, and `email` but a greater `id`, meaning it's a duplicate.
- This method is efficient for filtering duplicates without aggregation.

---

### **Which One Should You Use?**
| Method            | Best For | Database Support |
|------------------|---------|----------------|
| `GROUP BY HAVING` | Simple duplicate check | All databases |
| `JOIN` | Finding and filtering duplicates | All databases |
| `ROW_NUMBER()` | Identifying and ranking duplicates | PostgreSQL, MySQL 8+, SQL Server, Oracle |
| `EXISTS` | Performance optimization | All databases |
