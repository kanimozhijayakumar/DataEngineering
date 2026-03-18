You can delete duplicate records in SQL using `ROW_NUMBER()`, `CTE`, or `DELETE` with `DISTINCT`. Here’s an example for different SQL databases.

### **Example Table**
```sql
CREATE TABLE Employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary INT
);

INSERT INTO Employees (name, department, salary) VALUES
('Alice', 'HR', 60000),
('Bob', 'IT', 80000),
('Alice', 'HR', 60000), -- Duplicate
('Charlie', 'Finance', 70000),
('Alice', 'HR', 60000); -- Duplicate
```

---

### **Method 1: Using `ROW_NUMBER()` (Recommended)**
For databases supporting `ROW_NUMBER()` (PostgreSQL, MySQL 8+, SQL Server):

```sql
WITH CTE AS (
    SELECT id, 
           ROW_NUMBER() OVER (PARTITION BY name, department, salary ORDER BY id) AS row_num
    FROM Employees
)
DELETE FROM Employees 
WHERE id IN (SELECT id FROM CTE WHERE row_num > 1);
```

---

### **Method 2: Using `DISTINCT ON` (PostgreSQL)**
```sql
DELETE FROM Employees 
WHERE id NOT IN (
    SELECT MIN(id) 
    FROM Employees 
    GROUP BY name, department, salary
);
```

---

### **Method 3: Using `DELETE` with `JOIN` (MySQL)**
```sql
DELETE e1 FROM Employees e1
JOIN Employees e2 
ON e1.name = e2.name AND e1.department = e2.department AND e1.salary = e2.salary
WHERE e1.id > e2.id;
```

---

### **Method 4: Using `DELETE` with Subquery (SQL Server)**
```sql
DELETE FROM Employees WHERE id NOT IN (
    SELECT MIN(id) FROM Employees GROUP BY name, department, salary
);
```
