
---

# 🧠 **Adaptive Query Execution (AQE) – Summary Notes**

---

## ⚙️ **What It Is**

**Adaptive Query Execution (AQE)** is a **runtime optimization** feature introduced in **Spark 3.0+**.
It lets Spark **modify the query plan dynamically** after seeing the *real data size and distribution*.

---

## 🚀 **Main Goal**

Make Spark jobs **faster and smarter** by:

* Reducing unnecessary shuffles
* Balancing skewed data automatically
* Choosing optimal join types (e.g., broadcast join)
* Merging or splitting partitions dynamically

---

## ⚡ **Key Features**

| Feature                          | What It Does                                         | Benefit                                    |
| -------------------------------- | ---------------------------------------------------- | ------------------------------------------ |
| **Dynamic Plan Re-optimization** | Rewrites execution plan during runtime               | Uses actual statistics, not just estimates |
| **Coalesce Shuffle Partitions**  | Merges many small partitions into fewer              | Less overhead, better parallelism          |
| **Skew Join Handling**           | Splits large (skewed) partitions                     | Fixes slow, uneven joins                   |
| **Dynamic Join Selection**       | Converts shuffle join → broadcast join automatically | Faster joins without code change           |

---

## ⚙️ **Important Configurations**

```python
# Enable AQE (Main switch)
spark.conf.set("spark.sql.adaptive.enabled", "true")

# Enable skew join optimization
spark.conf.set("spark.sql.adaptive.skewJoin.enabled", "true")

# Merge small shuffle partitions
spark.conf.set("spark.sql.adaptive.coalescePartitions.enabled", "true")

# Allow reading local shuffle outputs
spark.conf.set("spark.sql.adaptive.localShuffleReader.enabled", "true")

# Advisory target partition size (tuning)
spark.conf.set("spark.sql.adaptive.advisoryPartitionSizeInBytes", "64MB")

# Number of shuffle partitions (initial)
spark.conf.set("spark.sql.shuffle.partitions", "200")
```

---

## 🧮 **How AQE Works (Step by Step)**

1. **Stage 1** → Spark executes normally and collects runtime stats.
2. **Stage 2** → AQE analyzes partition sizes and data skew.
3. **Stage 3** → Spark **re-optimizes** the physical plan:

   * Merges small partitions
   * Splits large skewed ones
   * Chooses better join types
4. **Stage 4** → Executes optimized plan for the rest of the job.

---

## 📊 **Benefits**

✅ Automatic performance tuning
✅ Handles skew automatically (no manual salting)
✅ Reduces shuffle overhead
✅ Makes jobs more stable and predictable
✅ No code change needed — only config

---

## ⚠️ **Minor Considerations**

| Issue                         | Description                                 |
| ----------------------------- | ------------------------------------------- |
| Slight overhead for tiny jobs | Collecting runtime stats adds a few seconds |
| Plans may vary per run        | AQE adapts dynamically (not deterministic)  |
| Streaming jobs                | AQE not supported in Structured Streaming   |

---

## 🧩 **AWS Glue Versions**

| Glue Version | Spark Version | AQE Availability                     | Default         |
| ------------ | ------------- | ------------------------------------ | --------------- |
| **Glue 3.0** | Spark 3.1.1   | ✅ Available but must enable manually | ❌ Off           |
| **Glue 4.0** | Spark 3.3.0   | ✅ Fully supported                    | ✅ On by default |

---

## ⚖️ **AQE vs. Manual Tuning**

| Aspect             | Without AQE                 | With AQE                     |
| ------------------ | --------------------------- | ---------------------------- |
| Join strategy      | Static (decided before run) | Dynamic (decided at runtime) |
| Shuffle partitions | Fixed                       | Merged / adjusted            |
| Skew handling      | Manual (salting)            | Automatic                    |
| Maintenance        | High                        | Low                          |

---

## 🧠 **Key Values to Remember**

| Setting                                           | Default | Recommended                     |
| ------------------------------------------------- | ------- | ------------------------------- |
| `spark.sql.adaptive.enabled`                      | false   | ✅ true                          |
| `spark.sql.adaptive.skewJoin.enabled`             | false   | ✅ true                          |
| `spark.sql.adaptive.coalescePartitions.enabled`   | true    | ✅ true                          |
| `spark.sql.adaptive.advisoryPartitionSizeInBytes` | 64 MB   | Adjust per data size            |
| `spark.sql.autoBroadcastJoinThreshold`            | 10 MB   | Adjust for larger lookup tables |

---

## 💬 **In Simple Words**

> AQE lets Spark **learn from the data as it runs**
> and **change its execution plan on the fly** —
> so you don’t have to guess how to tune partitions, joins, or skew.

---

### ✅ **Best Practice for Your PySpark or Glue Job**

At the top of every script:

```python
spark.conf.set("spark.sql.adaptive.enabled", "true")
spark.conf.set("spark.sql.adaptive.skewJoin.enabled", "true")
spark.conf.set("spark.sql.adaptive.coalescePartitions.enabled", "true")
spark.conf.set("spark.sql.adaptive.localShuffleReader.enabled", "true")
```
