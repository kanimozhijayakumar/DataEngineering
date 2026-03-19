**e-commerce example tables** with sample data to help beginners understand different **Window Functions** in SQL. The tables include `orders`, `customers`, and `products`, which will be useful for applying **ROW_NUMBER(), RANK(), DENSE_RANK(), NTILE(), LAG(), LEAD(), FIRST_VALUE(), LAST_VALUE(), and SUM() OVER()** functions.

---
https://www.tutorialspoint.com/execute_sql_online.php

### **Step 1: Create Tables**
```sql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    customer_name VARCHAR(100),
    country VARCHAR(50)
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2)
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    product_id INT REFERENCES products(product_id),
    order_date DATE,
    quantity INT
);
```

---

### **Step 2: Insert Sample Data**
```sql
INSERT INTO customers (customer_name, country) VALUES
('Alice', 'USA'),
('Bob', 'Canada'),
('Charlie', 'UK'),
('David', 'India'),
('Eve', 'USA');

INSERT INTO products (product_name, category, price) VALUES
('Laptop', 'Electronics', 1200.00),
('Smartphone', 'Electronics', 800.00),
('Tablet', 'Electronics', 500.00),
('Headphones', 'Accessories', 150.00),
('Smartwatch', 'Accessories', 200.00);

INSERT INTO orders (customer_id, product_id, order_date, quantity) VALUES
(1, 1, '2024-02-01', 1),
(2, 2, '2024-02-02', 2),
(3, 3, '2024-02-03', 1),
(4, 4, '2024-02-04', 3),
(5, 5, '2024-02-05', 1),
(1, 2, '2024-02-06', 2),
(2, 3, '2024-02-07', 1),
(3, 4, '2024-02-08', 1),
(4, 5, '2024-02-09', 2),
(5, 1, '2024-02-10', 1);
```

---

### **Step 3: Apply Window Functions**

#### **1. ROW_NUMBER() – Assigns a unique row number per partition**
```sql
SELECT order_id, customer_id, order_date,
       ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS row_num
FROM orders;
```

#### **2. RANK() – Assigns ranks with gaps**
```sql
SELECT order_id, customer_id, order_date,
       RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS rank_num
FROM orders;
```

#### **3. DENSE_RANK() – Assigns ranks without gaps**
```sql
SELECT order_id, customer_id, order_date,
       DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS dense_rank_num
FROM orders;
```

#### **4. NTILE(4) – Divides data into 4 equal groups**
```sql
SELECT order_id, customer_id, order_date,
       NTILE(4) OVER (ORDER BY order_date) AS quartile
FROM orders;
```

#### **5. LAG() – Get previous order quantity**
```sql
SELECT order_id, customer_id, order_date, quantity,
       LAG(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_quantity
FROM orders;
```

#### **6. LEAD() – Get next order quantity**
```sql
SELECT order_id, customer_id, order_date, quantity,
       LEAD(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_quantity
FROM orders;
```

#### **7. FIRST_VALUE() – Get the first product ordered by each customer**
```sql
SELECT order_id, customer_id, order_date, product_id,
       FIRST_VALUE(product_id) OVER (PARTITION BY customer_id ORDER BY order_date) AS first_product
FROM orders;
```

#### **8. LAST_VALUE() – Get the last product ordered by each customer**
```sql
SELECT order_id, customer_id, order_date, product_id,
       LAST_VALUE(product_id) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_product
FROM orders;
```

#### **9. SUM() OVER() – Running total of quantity ordered**
```sql
SELECT order_id, customer_id, order_date, quantity,
       SUM(quantity) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM orders;
```

---

Here are some **realistic examples** with sample outputs to help beginners understand how **window functions** work in an e-commerce scenario.

---

### **1. ROW_NUMBER() – Assigns a unique row number per customer**
```sql
SELECT order_id, customer_id, order_date,
       ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date) AS row_num
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | row_num |
|----------|------------|------------|---------|
| 1        | 1          | 2024-02-01 | 1       |
| 6        | 1          | 2024-02-06 | 2       |
| 2        | 2          | 2024-02-02 | 1       |
| 7        | 2          | 2024-02-07 | 2       |

🔹 **Use Case:** Identify the first order placed by each customer.

---

### **2. RANK() – Assigns ranks with gaps**
```sql
SELECT order_id, customer_id, order_date,
       RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS rank_num
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | rank_num |
|----------|------------|------------|----------|
| 1        | 1          | 2024-02-01 | 1        |
| 6        | 1          | 2024-02-06 | 2        |
| 2        | 2          | 2024-02-02 | 1        |
| 7        | 2          | 2024-02-07 | 2        |

🔹 **Use Case:** Identify order ranking for each customer.

---

### **3. DENSE_RANK() – Assigns ranks without gaps**
```sql
SELECT order_id, customer_id, order_date,
       DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS dense_rank_num
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | dense_rank_num |
|----------|------------|------------|----------------|
| 1        | 1          | 2024-02-01 | 1              |
| 6        | 1          | 2024-02-06 | 2              |
| 2        | 2          | 2024-02-02 | 1              |
| 7        | 2          | 2024-02-07 | 2              |

🔹 **Use Case:** Useful when ranking customer orders without gaps.

---

### **4. NTILE(4) – Distributes data into 4 groups**
```sql
SELECT order_id, customer_id, order_date,
       NTILE(4) OVER (ORDER BY order_date) AS quartile
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quartile |
|----------|------------|------------|----------|
| 1        | 1          | 2024-02-01 | 1        |
| 2        | 2          | 2024-02-02 | 1        |
| 3        | 3          | 2024-02-03 | 2        |
| 4        | 4          | 2024-02-04 | 2        |
| 5        | 5          | 2024-02-05 | 3        |
| 6        | 1          | 2024-02-06 | 3        |
| 7        | 2          | 2024-02-07 | 4        |
| 8        | 3          | 2024-02-08 | 4        |

🔹 **Use Case:** Divide orders into quartiles based on order date.

---

### **5. LAG() – Get previous order quantity**
```sql
SELECT order_id, customer_id, order_date, quantity,
       LAG(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_quantity
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | prev_quantity |
|----------|------------|------------|----------|---------------|
| 1        | 1          | 2024-02-01 | 1        | NULL          |
| 6        | 1          | 2024-02-06 | 2        | 1             |
| 2        | 2          | 2024-02-02 | 2        | NULL          |
| 7        | 2          | 2024-02-07 | 1        | 2             |

🔹 **Use Case:** Compare a customer's current and previous order quantities.

---

### **6. LEAD() – Get next order quantity**
```sql
SELECT order_id, customer_id, order_date, quantity,
       LEAD(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_quantity
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | next_quantity |
|----------|------------|------------|----------|---------------|
| 1        | 1          | 2024-02-01 | 1        | 2             |
| 6        | 1          | 2024-02-06 | 2        | NULL          |
| 2        | 2          | 2024-02-02 | 2        | 1             |
| 7        | 2          | 2024-02-07 | 1        | NULL          |

🔹 **Use Case:** Identify the next order placed by each customer.

---

### **7. FIRST_VALUE() – Get the first product ordered by each customer**
```sql
SELECT order_id, customer_id, order_date, product_id,
       FIRST_VALUE(product_id) OVER (PARTITION BY customer_id ORDER BY order_date) AS first_product
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | product_id | first_product |
|----------|------------|------------|------------|--------------|
| 1        | 1          | 2024-02-01 | 1          | 1            |
| 6        | 1          | 2024-02-06 | 2          | 1            |
| 2        | 2          | 2024-02-02 | 2          | 2            |
| 7        | 2          | 2024-02-07 | 3          | 2            |

🔹 **Use Case:** Track the first product purchased by each customer.

---

### **8. LAST_VALUE() – Get the last product ordered by each customer**
```sql
SELECT order_id, customer_id, order_date, product_id,
       LAST_VALUE(product_id) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_product
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | product_id | last_product |
|----------|------------|------------|------------|-------------|
| 1        | 1          | 2024-02-01 | 1          | 2           |
| 6        | 1          | 2024-02-06 | 2          | 2           |
| 2        | 2          | 2024-02-02 | 2          | 3           |
| 7        | 2          | 2024-02-07 | 3          | 3           |

🔹 **Use Case:** Track the last product purchased by each customer.

---

### **9. SUM() OVER() – Running total of quantity ordered**
```sql
SELECT order_id, customer_id, order_date, quantity,
       SUM(quantity) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | running_total |
|----------|------------|------------|----------|---------------|
| 1        | 1          | 2024-02-01 | 1        | 1             |
| 6        | 1          | 2024-02-06 | 2        | 3             |
| 2        | 2          | 2024-02-02 | 2        | 2             |
| 7        | 2          | 2024-02-07 | 1        | 3             |

🔹 **Use Case:** Track cumulative purchases per customer.

---
Here are **more SQL window function examples** with realistic e-commerce use cases and sample outputs.

---

### **10. COUNT() OVER() – Count orders per customer**
```sql
SELECT order_id, customer_id, order_date, quantity,
       COUNT(order_id) OVER (PARTITION BY customer_id) AS total_orders
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | total_orders |
|----------|------------|------------|----------|-------------|
| 1        | 1          | 2024-02-01 | 1        | 2           |
| 6        | 1          | 2024-02-06 | 2        | 2           |
| 2        | 2          | 2024-02-02 | 2        | 2           |
| 7        | 2          | 2024-02-07 | 1        | 2           |

🔹 **Use Case:** Find the number of orders each customer placed.

---

### **11. AVG() OVER() – Running average of order quantity**
```sql
SELECT order_id, customer_id, order_date, quantity,
       AVG(quantity) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS avg_order_quantity
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | avg_order_quantity |
|----------|------------|------------|----------|--------------------|
| 1        | 1          | 2024-02-01 | 1        | 1.0                |
| 6        | 1          | 2024-02-06 | 2        | 1.5                |
| 2        | 2          | 2024-02-02 | 2        | 2.0                |
| 7        | 2          | 2024-02-07 | 1        | 1.5                |

🔹 **Use Case:** Track the **average** order quantity for each customer.

---

### **12. MAX() OVER() – Find the max quantity ordered per customer**
```sql
SELECT order_id, customer_id, order_date, quantity,
       MAX(quantity) OVER (PARTITION BY customer_id) AS max_order_quantity
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | max_order_quantity |
|----------|------------|------------|----------|--------------------|
| 1        | 1          | 2024-02-01 | 1        | 2                  |
| 6        | 1          | 2024-02-06 | 2        | 2                  |
| 2        | 2          | 2024-02-02 | 2        | 2                  |
| 7        | 2          | 2024-02-07 | 1        | 2                  |

🔹 **Use Case:** Identify the **largest quantity** a customer has ordered.

---

### **13. MIN() OVER() – Find the first order quantity per customer**
```sql
SELECT order_id, customer_id, order_date, quantity,
       MIN(quantity) OVER (PARTITION BY customer_id) AS min_order_quantity
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | min_order_quantity |
|----------|------------|------------|----------|--------------------|
| 1        | 1          | 2024-02-01 | 1        | 1                  |
| 6        | 1          | 2024-02-06 | 2        | 1                  |
| 2        | 2          | 2024-02-02 | 2        | 1                  |
| 7        | 2          | 2024-02-07 | 1        | 1                  |

🔹 **Use Case:** Find the **smallest** order quantity per customer.

---

### **14. PERCENT_RANK() – Determine order position within customer history**
```sql
SELECT order_id, customer_id, order_date, quantity,
       PERCENT_RANK() OVER (PARTITION BY customer_id ORDER BY order_date) AS order_percentile
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | order_percentile |
|----------|------------|------------|----------|------------------|
| 1        | 1          | 2024-02-01 | 1        | 0.00             |
| 6        | 1          | 2024-02-06 | 2        | 1.00             |
| 2        | 2          | 2024-02-02 | 2        | 0.00             |
| 7        | 2          | 2024-02-07 | 1        | 1.00             |

🔹 **Use Case:** Determine the **position of an order** relative to others.

---

### **15. CUME_DIST() – Find cumulative distribution of orders**
```sql
SELECT order_id, customer_id, order_date, quantity,
       CUME_DIST() OVER (PARTITION BY customer_id ORDER BY order_date) AS cume_distribution
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | cume_distribution |
|----------|------------|------------|----------|-------------------|
| 1        | 1          | 2024-02-01 | 1        | 0.5               |
| 6        | 1          | 2024-02-06 | 2        | 1.0               |
| 2        | 2          | 2024-02-02 | 2        | 0.5               |
| 7        | 2          | 2024-02-07 | 1        | 1.0               |

🔹 **Use Case:** Calculate the **cumulative percentage** of orders per customer.

---

### **16. DIFFERENCE BETWEEN CURRENT AND PREVIOUS ORDER DATE**
```sql
SELECT order_id, customer_id, order_date,
       order_date - LAG(order_date, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS days_since_last_order
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | days_since_last_order |
|----------|------------|------------|----------------------|
| 1        | 1          | 2024-02-01 | NULL                 |
| 6        | 1          | 2024-02-06 | 5                    |
| 2        | 2          | 2024-02-02 | NULL                 |
| 7        | 2          | 2024-02-07 | 5                    |

🔹 **Use Case:** Find **gaps between customer orders**.

---

### **17. FIND CUSTOMERS WHO ORDERED FOR CONSECUTIVE DAYS**
```sql
SELECT order_id, customer_id, order_date,
       order_date - LAG(order_date, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS days_diff
FROM orders
WHERE order_date - LAG(order_date, 1) OVER (PARTITION BY customer_id ORDER BY order_date) = 1;
```
**Example Output:**
| order_id | customer_id | order_date  | days_diff |
|----------|------------|------------|----------|
| 6        | 1          | 2024-02-06 | 1        |

🔹 **Use Case:** Find customers who made **back-to-back purchases**.

---

### **18. ROLLING SALES SUM OVER LAST 3 ORDERS**
```sql
SELECT order_id, customer_id, order_date, quantity,
       SUM(quantity) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS rolling_3_orders
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | rolling_3_orders |
|----------|------------|------------|----------|------------------|
| 1        | 1          | 2024-02-01 | 1        | 1                |
| 6        | 1          | 2024-02-06 | 2        | 3                |

🔹 **Use Case:** Calculate **rolling sales trends**.

---

Here are **more SQL window function examples** with detailed use cases and outputs for e-commerce analysis.

---

### **19. FIND THE CUSTOMER WITH THE HIGHEST TOTAL ORDERS**
```sql
SELECT customer_id, 
       COUNT(order_id) AS total_orders,
       RANK() OVER (ORDER BY COUNT(order_id) DESC) AS order_rank
FROM orders
GROUP BY customer_id;
```
**Example Output:**
| customer_id | total_orders | order_rank |
|------------|-------------|------------|
| 1          | 5           | 1          |
| 2          | 4           | 2          |
| 3          | 3           | 3          |

🔹 **Use Case:** Identify the **top customer** with the highest number of orders.

---

### **20. FIND THE MOST EXPENSIVE PRODUCT ORDERED BY EACH CUSTOMER**
```sql
SELECT o.customer_id, o.order_id, o.product_id, p.price,
       MAX(p.price) OVER (PARTITION BY o.customer_id) AS max_price_per_customer
FROM orders o
JOIN products p ON o.product_id = p.product_id;
```
**Example Output:**
| customer_id | order_id | product_id | price  | max_price_per_customer |
|------------|----------|------------|--------|------------------------|
| 1          | 1        | 1          | 1200.0 | 1200.0                 |
| 1          | 6        | 2          | 800.0  | 1200.0                 |
| 2          | 2        | 2          | 800.0  | 800.0                  |
| 2          | 7        | 3          | 500.0  | 800.0                  |

🔹 **Use Case:** Find the **highest-priced product** a customer has ordered.

---

### **21. FIND CUSTOMERS WHO SPENT MORE THAN AVERAGE ORDER VALUE**
```sql
SELECT o.customer_id, o.order_id, (p.price * o.quantity) AS total_order_value,
       AVG(p.price * o.quantity) OVER () AS avg_order_value
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE (p.price * o.quantity) > (SELECT AVG(p.price * o.quantity) FROM orders o JOIN products p ON o.product_id = p.product_id);
```
**Example Output:**
| customer_id | order_id | total_order_value | avg_order_value |
|------------|----------|------------------|----------------|
| 1          | 1        | 1200.0           | 670.0         |
| 2          | 2        | 1600.0           | 670.0         |

🔹 **Use Case:** Find customers **spending above average** order value.

---

### **22. CALCULATE CUMULATIVE REVENUE PER CUSTOMER**
```sql
SELECT o.customer_id, o.order_id, o.order_date, (p.price * o.quantity) AS order_value,
       SUM(p.price * o.quantity) OVER (PARTITION BY o.customer_id ORDER BY o.order_date) AS cumulative_revenue
FROM orders o
JOIN products p ON o.product_id = p.product_id;
```
**Example Output:**
| customer_id | order_id | order_date  | order_value | cumulative_revenue |
|------------|----------|------------|-------------|--------------------|
| 1          | 1        | 2024-02-01 | 1200.0      | 1200.0             |
| 1          | 6        | 2024-02-06 | 1600.0      | 2800.0             |
| 2          | 2        | 2024-02-02 | 1600.0      | 1600.0             |
| 2          | 7        | 2024-02-07 | 500.0       | 2100.0             |

🔹 **Use Case:** Track how **revenue accumulates** for each customer over time.

---

### **23. FIND BIGGEST PURCHASE BY EACH CUSTOMER**
```sql
SELECT o.customer_id, o.order_id, o.order_date, (p.price * o.quantity) AS order_value,
       MAX(p.price * o.quantity) OVER (PARTITION BY o.customer_id) AS max_order_value
FROM orders o
JOIN products p ON o.product_id = p.product_id;
```
**Example Output:**
| customer_id | order_id | order_date  | order_value | max_order_value |
|------------|----------|------------|-------------|-----------------|
| 1          | 1        | 2024-02-01 | 1200.0      | 1600.0          |
| 1          | 6        | 2024-02-06 | 1600.0      | 1600.0          |
| 2          | 2        | 2024-02-02 | 1600.0      | 1600.0          |
| 2          | 7        | 2024-02-07 | 500.0       | 1600.0          |

🔹 **Use Case:** Identify the **largest single purchase** per customer.

---

### **24. FIND CUSTOMERS WHOSE LATEST ORDER IS ABOVE AVERAGE**
```sql
SELECT o.customer_id, o.order_id, o.order_date, (p.price * o.quantity) AS last_order_value
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE o.order_date = (SELECT MAX(o2.order_date) FROM orders o2 WHERE o2.customer_id = o.customer_id)
AND (p.price * o.quantity) > (SELECT AVG(p.price * o.quantity) FROM orders o JOIN products p ON o.product_id = p.product_id);
```
**Example Output:**
| customer_id | order_id | order_date  | last_order_value |
|------------|----------|------------|------------------|
| 1          | 6        | 2024-02-06 | 1600.0           |

🔹 **Use Case:** Find customers whose **most recent order is above average**.

---

### **25. TRACK CHANGES IN ORDER QUANTITY OVER TIME**
```sql
SELECT order_id, customer_id, order_date, quantity,
       quantity - LAG(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS quantity_change
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | quantity_change |
|----------|------------|------------|----------|-----------------|
| 1        | 1          | 2024-02-01 | 1        | NULL            |
| 6        | 1          | 2024-02-06 | 2        | 1               |
| 2        | 2          | 2024-02-02 | 2        | NULL            |
| 7        | 2          | 2024-02-07 | 1        | -1              |

🔹 **Use Case:** Monitor **order volume trends** per customer.

---

### **26. FIND CUSTOMERS WHO HAVE NOT ORDERED RECENTLY**
```sql
SELECT customer_id, MAX(order_date) AS last_order_date,
       CURRENT_DATE - MAX(order_date) AS days_since_last_order
FROM orders
GROUP BY customer_id
HAVING (CURRENT_DATE - MAX(order_date)) > 30;
```
**Example Output:**
| customer_id | last_order_date | days_since_last_order |
|------------|----------------|----------------------|
| 3          | 2023-12-30      | 45                   |

🔹 **Use Case:** Identify **inactive customers** for re-engagement.

---
Here are **more advanced SQL window function examples** with detailed **e-commerce use cases** and **realistic outputs** to enhance your understanding.

---

### **27. FIND THE TIME GAP BETWEEN ORDERS FOR EACH CUSTOMER**
```sql
SELECT order_id, customer_id, order_date,
       order_date - LAG(order_date, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS days_since_last_order
FROM orders;
```
**Example Output:**
| order_id | customer_id | order_date  | days_since_last_order |
|----------|------------|------------|----------------------|
| 1        | 1          | 2024-02-01 | NULL                 |
| 6        | 1          | 2024-02-06 | 5                    |
| 2        | 2          | 2024-02-02 | NULL                 |
| 7        | 2          | 2024-02-07 | 5                    |

🔹 **Use Case:** Find **repeat purchase frequency** for customers.

---

### **28. FIND ORDERS WHERE QUANTITY INCREASED COMPARED TO PREVIOUS ORDER**
```sql
SELECT order_id, customer_id, order_date, quantity,
       quantity - LAG(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) AS quantity_change
FROM orders
WHERE quantity - LAG(quantity, 1) OVER (PARTITION BY customer_id ORDER BY order_date) > 0;
```
**Example Output:**
| order_id | customer_id | order_date  | quantity | quantity_change |
|----------|------------|------------|----------|-----------------|
| 6        | 1          | 2024-02-06 | 2        | 1               |

🔹 **Use Case:** Identify **customers increasing their order quantity**.

---

### **29. FIND THE LAST THREE ORDERS PER CUSTOMER**
```sql
SELECT order_id, customer_id, order_date,
       ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS order_rank
FROM orders
WHERE ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) <= 3;
```
**Example Output:**
| order_id | customer_id | order_date  | order_rank |
|----------|------------|------------|-----------|
| 6        | 1          | 2024-02-06 | 1         |
| 1        | 1          | 2024-02-01 | 2         |
| 7        | 2          | 2024-02-07 | 1         |

🔹 **Use Case:** Find **most recent orders** for customer service follow-ups.

---

### **30. CALCULATE AVERAGE ORDER VALUE OVER LAST 5 ORDERS**
```sql
SELECT order_id, customer_id, order_date, (p.price * o.quantity) AS order_value,
       AVG(p.price * o.quantity) OVER (PARTITION BY o.customer_id ORDER BY o.order_date ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) AS avg_last_5_orders
FROM orders o
JOIN products p ON o.product_id = p.product_id;
```
**Example Output:**
| order_id | customer_id | order_date  | order_value | avg_last_5_orders |
|----------|------------|------------|-------------|-------------------|
| 1        | 1          | 2024-02-01 | 1200.0      | 1200.0            |
| 6        | 1          | 2024-02-06 | 800.0       | 1000.0            |

🔹 **Use Case:** Track **average spending behavior** per customer.

---

### **31. FIND CUSTOMERS WHO PLACED TWO ORDERS ON THE SAME DAY**
```sql
SELECT customer_id, order_date, COUNT(order_id) AS orders_on_same_day
FROM orders
GROUP BY customer_id, order_date
HAVING COUNT(order_id) > 1;
```
**Example Output:**
| customer_id | order_date  | orders_on_same_day |
|------------|------------|--------------------|
| 2          | 2024-02-02 | 2                  |

🔹 **Use Case:** Detect **bulk orders** placed on the same day.

---

### **32. RANK PRODUCTS BASED ON TOTAL SALES PER CATEGORY**
```sql
SELECT p.category, p.product_name, SUM(o.quantity) AS total_sold,
       RANK() OVER (PARTITION BY p.category ORDER BY SUM(o.quantity) DESC) AS sales_rank
FROM orders o
JOIN products p ON o.product_id = p.product_id
GROUP BY p.category, p.product_name;
```
**Example Output:**
| category     | product_name | total_sold | sales_rank |
|-------------|-------------|------------|-----------|
| Electronics | Laptop      | 10         | 1         |
| Electronics | Smartphone  | 7          | 2         |

🔹 **Use Case:** Identify **best-selling products per category**.

---

### **33. FIND CUSTOMERS WHO ORDERED EVERY MONTH IN THE LAST 6 MONTHS**
```sql
SELECT customer_id, COUNT(DISTINCT EXTRACT(MONTH FROM order_date)) AS months_ordered
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '6 months'
GROUP BY customer_id
HAVING COUNT(DISTINCT EXTRACT(MONTH FROM order_date)) = 6;
```
**Example Output:**
| customer_id | months_ordered |
|------------|---------------|
| 1          | 6             |

🔹 **Use Case:** Identify **highly engaged customers**.

---

### **34. FIND THE MOST COMMON ORDER QUANTITY**
```sql
SELECT quantity, COUNT(*) AS frequency,
       RANK() OVER (ORDER BY COUNT(*) DESC) AS quantity_rank
FROM orders
GROUP BY quantity;
```
**Example Output:**
| quantity | frequency | quantity_rank |
|---------|----------|--------------|
| 2       | 5        | 1            |
| 1       | 3        | 2            |

🔹 **Use Case:** Understand **order patterns** for inventory planning.

---

### **35. FIND CUSTOMERS WHO HAVE BEEN CONSISTENTLY INCREASING ORDER VALUE**
```sql
SELECT customer_id, order_id, order_date, (p.price * o.quantity) AS order_value,
       LAG((p.price * o.quantity), 1) OVER (PARTITION BY o.customer_id ORDER BY o.order_date) AS prev_order_value
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE (p.price * o.quantity) > LAG((p.price * o.quantity), 1) OVER (PARTITION BY o.customer_id ORDER BY o.order_date);
```
**Example Output:**
| customer_id | order_id | order_date  | order_value | prev_order_value |
|------------|----------|------------|------------|------------------|
| 1          | 6        | 2024-02-06 | 1600.0     | 1200.0           |

🔹 **Use Case:** Identify **high-value customers** who are increasing their spending.

---

### **36. FIND CUSTOMERS WHO HAVE DROPPED THEIR SPENDING OVER TIME**
```sql
SELECT customer_id, order_id, order_date, (p.price * o.quantity) AS order_value,
       LEAD((p.price * o.quantity), 1) OVER (PARTITION BY o.customer_id ORDER BY o.order_date) AS next_order_value
FROM orders o
JOIN products p ON o.product_id = p.product_id
WHERE (p.price * o.quantity) > LEAD((p.price * o.quantity), 1) OVER (PARTITION BY o.customer_id ORDER BY o.order_date);
```
**Example Output:**
| customer_id | order_id | order_date  | order_value | next_order_value |
|------------|----------|------------|------------|------------------|
| 2          | 2        | 2024-02-02 | 1600.0     | 500.0            |

🔹 **Use Case:** Detect **potential churn risks**.

---


