### **Window Functions Example with `emp` and `dept` Tables**
---
https://www.tutorialspoint.com/execute_sql_online.php
We will use **two tables**:

1. **`emp` Table** (Employees)
2. **`dept` Table** (Departments)

---

### **Step 1: Create Sample Tables**
#### **`emp` (Employee Table)**
```sql
CREATE TABLE emp (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    salary INT,
    dept_id INT
);
```

#### **`dept` (Department Table)**
```sql
CREATE TABLE dept (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);
```

---

### **Step 2: Insert Sample Data**
```sql
INSERT INTO dept VALUES (10, 'HR'), (20, 'IT'), (30, 'Finance');

INSERT INTO emp VALUES 
(101, 'Alice', 8000, 10),
(102, 'Bob', 7500, 10),
(103, 'Charlie', 9000, 20),
(104, 'David', 7000, 20),
(105, 'Emma', 8500, 30),
(106, 'Frank', 8000, 30);
```

---

### **Step 3: Query with Window Functions**
---
#### **1. Rank Employees by Salary in Each Department (`RANK()`)**
```sql
SELECT e.emp_id, e.emp_name, e.salary, e.dept_id, d.dept_name,
       RANK() OVER (PARTITION BY e.dept_id ORDER BY e.salary DESC) AS dept_salary_rank
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id;
```
✅ **Output:**
| emp_id | emp_name | salary | dept_id | dept_name | dept_salary_rank |
|--------|---------|--------|---------|-----------|------------------|
| 101    | Alice   | 8000   | 10      | HR        | 1                |
| 102    | Bob     | 7500   | 10      | HR        | 2                |
| 103    | Charlie | 9000   | 20      | IT        | 1                |
| 104    | David   | 7000   | 20      | IT        | 2                |
| 105    | Emma    | 8500   | 30      | Finance   | 1                |
| 106    | Frank   | 8000   | 30      | Finance   | 2                |

🎯 **Key Point:** `RANK()` ranks employees **within each department** (`PARTITION BY dept_id`).

---

#### **2. Running Total of Salaries in Each Department (`SUM()`)**
```sql
SELECT e.emp_id, e.emp_name, e.salary, e.dept_id, d.dept_name,
       SUM(e.salary) OVER (PARTITION BY e.dept_id ORDER BY e.salary DESC) AS running_total
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id;
```
✅ **Output:**
| emp_id | emp_name | salary | dept_id | dept_name | running_total |
|--------|---------|--------|---------|-----------|--------------|
| 101    | Alice   | 8000   | 10      | HR        | 8000         |
| 102    | Bob     | 7500   | 10      | HR        | 15500        |
| 103    | Charlie | 9000   | 20      | IT        | 9000         |
| 104    | David   | 7000   | 20      | IT        | 16000        |
| 105    | Emma    | 8500   | 30      | Finance   | 8500         |
| 106    | Frank   | 8000   | 30      | Finance   | 16500        |

🎯 **Key Point:** Running total of salaries **within each department** (`PARTITION BY dept_id`).

---

#### **3. Compare Employee Salary with Previous Salary (`LAG()`)**
```sql
SELECT e.emp_id, e.emp_name, e.salary, e.dept_id, d.dept_name,
       LAG(e.salary, 1, 0) OVER (PARTITION BY e.dept_id ORDER BY e.salary DESC) AS prev_salary
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id;
```
✅ **Output:**
| emp_id | emp_name | salary | dept_id | dept_name | prev_salary |
|--------|---------|--------|---------|-----------|-------------|
| 101    | Alice   | 8000   | 10      | HR        | 0           |
| 102    | Bob     | 7500   | 10      | HR        | 8000        |
| 103    | Charlie | 9000   | 20      | IT        | 0           |
| 104    | David   | 7000   | 20      | IT        | 9000        |
| 105    | Emma    | 8500   | 30      | Finance   | 0           |
| 106    | Frank   | 8000   | 30      | Finance   | 8500        |

🎯 **Key Point:** `LAG()` gets the **previous salary** for each department.

---

#### **4. Find Next Employee’s Salary (`LEAD()`)**
```sql
SELECT e.emp_id, e.emp_name, e.salary, e.dept_id, d.dept_name,
       LEAD(e.salary, 1, 0) OVER (PARTITION BY e.dept_id ORDER BY e.salary DESC) AS next_salary
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id;
```
✅ **Output:**
| emp_id | emp_name | salary | dept_id | dept_name | next_salary |
|--------|---------|--------|---------|-----------|-------------|
| 101    | Alice   | 8000   | 10      | HR        | 7500        |
| 102    | Bob     | 7500   | 10      | HR        | 0           |
| 103    | Charlie | 9000   | 20      | IT        | 7000        |
| 104    | David   | 7000   | 20      | IT        | 0           |
| 105    | Emma    | 8500   | 30      | Finance   | 8000        |
| 106    | Frank   | 8000   | 30      | Finance   | 0           |

🎯 **Key Point:** `LEAD()` gets the **next salary** in the same department.

---

#### **5. Find Percentage of Salary in Each Department (`PERCENT_RANK()`)**
```sql
SELECT e.emp_id, e.emp_name, e.salary, e.dept_id, d.dept_name,
       PERCENT_RANK() OVER (PARTITION BY e.dept_id ORDER BY e.salary DESC) AS salary_percentile
FROM emp e
JOIN dept d ON e.dept_id = d.dept_id;
```
✅ **Output:**
| emp_id | emp_name | salary | dept_id | dept_name | salary_percentile |
|--------|---------|--------|---------|-----------|--------------------|
| 101    | Alice   | 8000   | 10      | HR        | 0.0                |
| 102    | Bob     | 7500   | 10      | HR        | 1.0                |
| 103    | Charlie | 9000   | 20      | IT        | 0.0                |
| 104    | David   | 7000   | 20      | IT        | 1.0                |
| 105    | Emma    | 8500   | 30      | Finance   | 0.0                |
| 106    | Frank   | 8000   | 30      | Finance   | 1.0                |

🎯 **Key Point:** Shows how each employee's salary ranks **within the department**.

---

### **Summary**
- `RANK()`: Employee rank in department
- `SUM()`: Running total salary in department
- `LAG()`: Previous employee salary
- `LEAD()`: Next employee salary
- `PERCENT_RANK()`: Percentile of salary in department
