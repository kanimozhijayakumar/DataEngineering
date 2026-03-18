Here is a **PySpark script** to test **window functions** on **products, customers, and orders** tables. This includes **ranking, cumulative sums, percentiles, and NTH_VALUE() functions**.

---

### **Step 1: Setup PySpark**
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, rank, dense_rank, row_number, ntile, percent_rank, cume_dist, first, last, sum, avg, max, min
from pyspark.sql.window import Window
```

---

### **Step 2: Create a SparkSession**
```python
spark = SparkSession.builder.appName("WindowFunctionsExample").getOrCreate()
```

---

### **Step 3: Create Sample Data**
```python
from pyspark.sql import Row

customers = spark.createDataFrame([
    Row(customer_id=1, name="Alice"),
    Row(customer_id=2, name="Bob"),
    Row(customer_id=3, name="Charlie"),
])

products = spark.createDataFrame([
    Row(product_id=101, name="Laptop", price=1000),
    Row(product_id=102, name="Phone", price=700),
    Row(product_id=103, name="Tablet", price=500),
])

orders = spark.createDataFrame([
    Row(order_id=1, customer_id=1, product_id=101, amount=1000, order_date="2024-01-01"),
    Row(order_id=2, customer_id=2, product_id=102, amount=700, order_date="2024-01-02"),
    Row(order_id=3, customer_id=1, product_id=103, amount=500, order_date="2024-01-03"),
    Row(order_id=4, customer_id=3, product_id=101, amount=1000, order_date="2024-01-04"),
    Row(order_id=5, customer_id=2, product_id=103, amount=500, order_date="2024-01-05"),
])
```

---

### **Step 4: Define Window Spec**
```python
window_spec = Window.partitionBy("customer_id").orderBy("order_date")
```

---

### **Step 5: Apply Window Functions**
```python
orders_with_window = orders.withColumn("rank", rank().over(window_spec)) \
    .withColumn("dense_rank", dense_rank().over(window_spec)) \
    .withColumn("row_number", row_number().over(window_spec)) \
    .withColumn("cumulative_sum", sum("amount").over(window_spec.rowsBetween(Window.unboundedPreceding, Window.currentRow))) \
    .withColumn("percent_rank", percent_rank().over(window_spec)) \
    .withColumn("cume_dist", cume_dist().over(window_spec)) \
    .withColumn("ntile_4", ntile(4).over(window_spec))

orders_with_window.show()
```

---

### **Step 6: Join Orders with Customers and Products**
```python
final_df = orders_with_window.join(customers, "customer_id").join(products, "product_id")
final_df.select("customer_id", "name", "order_id", "product_id", "price", "amount", "order_date", "rank", "percent_rank", "cume_dist").show()
```

---

### **Explanation of Window Functions**
| Function | Description |
|----------|------------|
| `rank()` | Assigns rank with gaps (same value = same rank, next rank skipped). |
| `dense_rank()` | Assigns rank **without gaps**. |
| `row_number()` | Assigns a unique row number (no duplicates). |
| `sum()` | Calculates running total of `amount`. |
| `percent_rank()` | Returns percentile rank (0 to 1). |
| `cume_dist()` | Returns cumulative distribution (fraction of rows ≤ current row). |
| `ntile(4)` | Divides data into 4 equal groups (quartiles). |

---

### **Expected Output**
| customer_id | name   | order_id | product_id | price | amount | order_date | rank | percent_rank | cume_dist |
|------------|--------|----------|------------|-------|--------|------------|------|--------------|-----------|
| 1          | Alice  | 1        | 101        | 1000  | 1000   | 2024-01-01 | 1    | 0.0000       | 0.5000    |
| 1          | Alice  | 3        | 103        | 500   | 500    | 2024-01-03 | 2    | 1.0000       | 1.0000    |
| 2          | Bob    | 2        | 102        | 700   | 700    | 2024-01-02 | 1    | 0.0000       | 0.5000    |
| 2          | Bob    | 5        | 103        | 500   | 500    | 2024-01-05 | 2    | 1.0000       | 1.0000    |
| 3          | Charlie| 4        | 101        | 1000  | 1000   | 2024-01-04 | 1    | 0.0000       | 1.0000    |

---

### **🚀 Try It Out!**
- Modify `ntile(4)` to `ntile(10)` for deciles.
- Use `sum()` with different `ROWS BETWEEN` values.
- Add `first_value()` and `last_value()` to track **first and last purchases**.
