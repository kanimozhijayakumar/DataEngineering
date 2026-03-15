### **List of SQL Window Functions**
Window functions perform calculations **across a set of table rows** that are **related to the current row**, without collapsing the result into a single output row (unlike aggregate functions).

---

## **1. Ranking Functions**
Used to assign a ranking to rows within a partition.

| Function | Description |
|----------|------------|
| **`RANK()`** | Assigns a **ranking** with **gaps** if values are the same. |
| **`DENSE_RANK()`** | Assigns a **ranking without gaps** (no skipped numbers). |
| **`ROW_NUMBER()`** | Assigns a **unique number to each row**, even if values are the same. |
| **`NTILE(n)`** | Divides rows into `n` **equal buckets** and assigns a bucket number. |

**Example: Ranking Orders by Amount**
```sql
SELECT 
    order_id, 
    customer_id, 
    amount, 
    RANK() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS rank,
    DENSE_RANK() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS dense_rank,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS row_num
FROM orders;
```

---

## **2. Offset (Lag/Lead) Functions**
Used to fetch values from previous or next rows.

| Function | Description |
|----------|------------|
| **`LAG(column, offset, default_value)`** | Fetches the value **before** the current row (default offset = 1). |
| **`LEAD(column, offset, default_value)`** | Fetches the value **after** the current row (default offset = 1). |

**Example: Compare Current Order with Previous & Next Orders**
```sql
SELECT 
    order_id, 
    customer_id, 
    amount, 
    LAG(amount, 1, 0) OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_order,
    LEAD(amount, 1, 0) OVER (PARTITION BY customer_id ORDER BY order_date) AS next_order
FROM orders;
```

---

## **3. Value Selection Functions**
Used to get the **first or last value** within a window frame.

| Function | Description |
|----------|------------|
| **`FIRST_VALUE(column)`** | Returns the **first value** in the partition. |
| **`LAST_VALUE(column)`** | Returns the **last value** in the partition (requires frame adjustment). |
| **`NTH_VALUE(column, n)`** | Returns the **nth value** in the partition. |

**Example: Get First and Last Order Amount for Each Customer**
```sql
SELECT 
    order_id, 
    customer_id, 
    amount, 
    FIRST_VALUE(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS first_order,
    LAST_VALUE(amount) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_order,
    NTH_VALUE(amount, 2) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS second_order
FROM orders;
```

---

## **4. Aggregate Window Functions**
Perform cumulative calculations **without grouping rows**.

| Function | Description |
|----------|------------|
| **`SUM(column)`** | Calculates running sum. |
| **`AVG(column)`** | Calculates running average. |
| **`MIN(column)`** | Returns minimum value in window frame. |
| **`MAX(column)`** | Returns maximum value in window frame. |
| **`COUNT(column)`** | Returns running count of rows. |

**Example: Running Total & Average Order Amount**
```sql
SELECT 
    order_id, 
    customer_id, 
    amount, 
    SUM(amount) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total,
    AVG(amount) OVER (PARTITION BY customer_id ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM orders;
```

---

## **5. Percentile & Distribution Functions**
Used for percentile-based ranking and distribution.

| Function | Description |
|----------|------------|
| **`PERCENT_RANK()`** | Relative ranking between `0-1` (based on percentile). |
| **`CUME_DIST()`** | Cumulative distribution (fraction of rows ≤ current row). |

**Example: Calculate Percentile Ranking**
```sql
SELECT 
    order_id, 
    customer_id, 
    amount, 
    PERCENT_RANK() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS percentile_rank,
    CUME_DIST() OVER (PARTITION BY customer_id ORDER BY amount DESC) AS cumulative_dist
FROM orders;
```

---

## **Summary: When to Use Which Function**
| Function Type | Functions | Purpose |
|--------------|----------|---------|
| **Ranking** | `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`, `NTILE(n)` | Rank rows within a partition |
| **Offsets** | `LAG()`, `LEAD()` | Compare with previous/next row |
| **First/Last** | `FIRST_VALUE()`, `LAST_VALUE()`, `NTH_VALUE(n)` | Retrieve first, last, or nth value in a partition |
| **Aggregates** | `SUM()`, `AVG()`, `MIN()`, `MAX()`, `COUNT()` | Running totals, averages, and aggregates |
| **Percentiles** | `PERCENT_RANK()`, `CUME_DIST()` | Compute percentiles |

---
