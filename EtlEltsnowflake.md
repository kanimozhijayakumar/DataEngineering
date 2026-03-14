### **Modernizing ETL to ELT: Moving from Informatica to DBT for Snowflake**
---
Traditional **ETL (Extract-Transform-Load)** tools like **Informatica** process data outside the data warehouse before loading it, while modern **ELT (Extract-Load-Transform)** using **DBT (Data Build Tool)** leverages the data warehouse’s compute power (e.g., Snowflake) for transformation.

### **Key Differences Between ETL & ELT**
| Feature | ETL (Informatica) | ELT (DBT + Snowflake) |
|---------|------------------|--------------------|
| **Processing Location** | Outside Snowflake | Inside Snowflake |
| **Transformation Timing** | Before loading data | After loading data |
| **Scalability** | Limited | Highly scalable |
| **Data Processing** | Sequential, batch-oriented | Parallel, scalable SQL-based |
| **Performance** | Dependent on ETL server | Leverages Snowflake’s compute |

---
### **Implementation Steps**
We will compare ETL using **Informatica** and ELT using **DBT**, assuming **Snowflake** as the final destination.

## **1️⃣ ETL Implementation using Informatica (Traditional Approach)**
#### **Steps in Informatica ETL**
1. **Extract**: Read data from **S3 / On-Prem DB (MySQL, Oracle, SQL Server)** using Informatica PowerCenter.
2. **Transform**: Apply data cleaning, joins, aggregations, and formatting in **Informatica Mapping Designer**.
3. **Load**: Push transformed data into **Snowflake**.

#### **Sample Informatica Workflow**
- **Source**: S3 Bucket (CSV files)
- **Transformations**:
  - Data Cleansing (NULL Handling)
  - Data Type Conversion
  - Joins, Aggregations
- **Destination**: Snowflake Table (`sales_data`)

##### **SQL in Informatica (Transformation Example)**
```sql
SELECT customer_id, 
       SUM(order_amount) AS total_sales
FROM source_sales
WHERE order_date >= '2024-01-01'
GROUP BY customer_id;
```
- Processed in Informatica before **loading** to Snowflake.

---

## **2️⃣ ELT Implementation using DBT (Modern Approach)**
#### **Steps in DBT ELT**
1. **Extract**: Use **Snowpipe** or **Fivetran** to load raw data from **S3 / MySQL / Kafka** into Snowflake.
2. **Load**: Store raw data in a **staging schema** (`stg.sales_raw`).
3. **Transform**: Use **DBT models** to clean, join, and aggregate data inside Snowflake.

---

### **Step 1: Extract & Load (Data Ingestion)**
- Use **Snowpipe** to continuously ingest files from S3 into Snowflake.

```sql
CREATE OR REPLACE STAGE raw_stage 
URL = 's3://my-bucket/sales_data'
CREDENTIALS = (AWS_KEY_ID = 'XYZ' AWS_SECRET_KEY = 'ABC');

COPY INTO raw.sales_raw
FROM @raw_stage
FILE_FORMAT = (TYPE = CSV, FIELD_OPTIONALLY_ENCLOSED_BY='"');
```
---
### **Step 2: Transform using DBT Models**
1. **Create a `staging` model (`models/staging/stg_sales.sql`)**
```sql
SELECT customer_id, 
       order_date, 
       order_amount
FROM raw.sales_raw
WHERE order_date >= '2024-01-01';
```
---
2. **Create an aggregated `fact_sales` model (`models/marts/fact_sales.sql`)**
```sql
SELECT customer_id, 
       COUNT(order_id) AS order_count, 
       SUM(order_amount) AS total_sales
FROM {{ ref('stg_sales') }}
GROUP BY customer_id;
```
---
### **Step 3: Schedule DBT Transformations**
- Run transformations inside **Snowflake** via **DBT Cloud / dbt CLI**.
```bash
dbt run --models stg_sales fact_sales
```

---

## **Comparison: Informatica (ETL) vs DBT (ELT)**
| **Aspect** | **Informatica ETL** | **DBT ELT** |
|------------|----------------------|--------------|
| **Processing** | Outside Snowflake (Informatica Server) | Inside Snowflake (SQL-based) |
| **Compute Cost** | Expensive Informatica server | Snowflake’s scalable compute |
| **Scalability** | Limited batch processing | Scales with Snowflake |
| **Development Speed** | Complex UI-based mappings | SQL-based, easy versioning (Git) |
| **Orchestration** | Requires Informatica Workflow Manager | Integrated with dbt Cloud/Airflow |

---

### **📌 Final Thoughts**
1. **Migrate Extract & Load to Snowpipe/Fivetran.**  
2. **Move transformation logic from Informatica to DBT models.**  
3. **Leverage Snowflake’s compute for fast transformations.**  
4. **Use dbt Cloud / Airflow for scheduling instead of Informatica Workflow Manager.**  
