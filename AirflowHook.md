### **What is an Airflow Hook?**
A **Hook** in Apache Airflow is a **wrapper around external systems (databases, APIs, cloud services, etc.)** that simplifies authentication and data access. Hooks allow you to connect to these services easily using **Airflow Connections**.

---

## **Why Use Hooks in Airflow?**
✅ **Reusability** – Instead of manually handling authentication in every DAG, use hooks.  
✅ **Abstraction** – Hooks provide an easy-to-use interface for interacting with external systems.  
✅ **Security** – Credentials are stored in **Airflow Connections**, not in code.  
✅ **Integration** – Many hooks are built-in for popular services like AWS, GCP, PostgreSQL, etc.

---

## **How Hooks Work in Airflow**
1. **Define a Connection** in **Airflow UI (`Admin -> Connections`)** with credentials.
2. **Use a Hook** in your DAG to fetch data or interact with the external service.
3. **Execute Queries, API Calls, or File Transfers** via Hooks.

---

## **Types of Airflow Hooks**
| Hook Type | Purpose |
|-----------|---------|
| **Database Hooks** | Connect to databases like PostgreSQL, MySQL, Snowflake, etc. |
| **Cloud Hooks** | Work with AWS, GCP, Azure (S3, BigQuery, Redshift, etc.). |
| **API Hooks** | Call external REST APIs (HTTP, Slack, Google Sheets). |
| **File Transfer Hooks** | Transfer files via SFTP, FTP, or Google Cloud Storage. |

---

## **1. Database Hooks**
### **PostgreSQL Hook**
Used to connect to **PostgreSQL** databases.

```python
from airflow.providers.postgres.hooks.postgres import PostgresHook

def fetch_data():
    pg_hook = PostgresHook(postgres_conn_id="my_postgres")
    conn = pg_hook.get_conn()
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM users LIMIT 10")
    result = cursor.fetchall()
    print(result)
```
- **`postgres_conn_id="my_postgres"`** references the connection stored in Airflow.
- Queries can be executed using `.get_conn()` or `.get_pandas_df()`.

---

### **MySQL Hook**
Used to interact with **MySQL** databases.

```python
from airflow.providers.mysql.hooks.mysql import MySqlHook

def fetch_data():
    mysql_hook = MySqlHook(mysql_conn_id="my_mysql")
    df = mysql_hook.get_pandas_df(sql="SELECT * FROM customers LIMIT 10")
    print(df)
```
- Retrieves data as a **Pandas DataFrame**.

---

## **2. Cloud Hooks**
### **AWS S3 Hook**
Used to read and write data from **AWS S3**.

```python
from airflow.providers.amazon.aws.hooks.s3 import S3Hook

def upload_file_to_s3():
    s3_hook = S3Hook(aws_conn_id="aws_default")
    s3_hook.load_file(filename="/tmp/myfile.csv", key="data/myfile.csv", bucket_name="my-bucket", replace=True)
```
- Uses the `aws_conn_id` stored in Airflow.
- Uploads a file to S3.

---

### **Google Cloud BigQuery Hook**
Used to interact with **Google BigQuery**.

```python
from airflow.providers.google.cloud.hooks.bigquery import BigQueryHook

def run_bigquery():
    bq_hook = BigQueryHook(gcp_conn_id="my_gcp")
    sql_query = "SELECT COUNT(*) FROM my_dataset.my_table"
    result = bq_hook.get_pandas_df(sql=sql_query)
    print(result)
```
- Runs a query in BigQuery and **returns results as a DataFrame**.

---

## **3. API Hooks**
### **HTTP Hook**
Used for making **REST API calls**.

```python
from airflow.providers.http.hooks.http import HttpHook

def call_api():
    http_hook = HttpHook(method="GET", http_conn_id="my_api")
    response = http_hook.run(endpoint="/v1/data")
    print(response.text)
```
- Uses an Airflow Connection (`http_conn_id`) to store the **API base URL**.

---

### **Slack Hook**
Used for sending messages to **Slack**.

```python
from airflow.providers.slack.hooks.slack_webhook import SlackWebhookHook

def send_slack_alert():
    slack_hook = SlackWebhookHook(slack_webhook_conn_id="slack_conn")
    slack_hook.send(text="Airflow DAG completed successfully!")
```
- Uses `slack_webhook_conn_id` for authentication.

---

## **4. File Transfer Hooks**
### **SFTP Hook**
Used for transferring files via **SFTP**.

```python
from airflow.providers.sftp.hooks.sftp import SFTPHook

def upload_file_sftp():
    sftp_hook = SFTPHook(ftp_conn_id="my_sftp")
    sftp_hook.store_file(remote_path="/remote/data.csv", local_path="/tmp/data.csv")
```
- Uploads a file to an **SFTP server**.

---

## **Using Hooks in Airflow DAGs**
Hooks are commonly used inside **PythonOperator** tasks in DAGs.

```python
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from airflow.providers.postgres.hooks.postgres import PostgresHook
from datetime import datetime

def extract_data():
    pg_hook = PostgresHook(postgres_conn_id="my_postgres")
    df = pg_hook.get_pandas_df(sql="SELECT * FROM sales_data")
    df.to_csv('/tmp/sales.csv', index=False)

dag = DAG(
    dag_id="hook_example_dag",
    schedule_interval="@daily",
    start_date=datetime(2024, 3, 1),
    catchup=False
)

extract_task = PythonOperator(
    task_id="extract_data",
    python_callable=extract_data,
    dag=dag
)

extract_task
```
- This DAG extracts **PostgreSQL data** using `PostgresHook` and saves it to a file.

---

## **Summary**
| Hook Type | Example |
|-----------|---------|
| **PostgreSQL Hook** | `PostgresHook(postgres_conn_id="my_postgres")` |
| **MySQL Hook** | `MySqlHook(mysql_conn_id="my_mysql")` |
| **AWS S3 Hook** | `S3Hook(aws_conn_id="aws_default")` |
| **BigQuery Hook** | `BigQueryHook(gcp_conn_id="my_gcp")` |
| **HTTP Hook** | `HttpHook(http_conn_id="my_api")` |
| **Slack Hook** | `SlackWebhookHook(slack_webhook_conn_id="slack_conn")` |
| **SFTP Hook** | `SFTPHook(ftp_conn_id="my_sftp")` |

---

### **🔹 Why Should You Use Hooks?**
✅ **Simplifies Authentication** – No hardcoding credentials.  
✅ **Reusable Code** – Use Hooks across multiple DAGs.  
✅ **Secure Storage** – Credentials stored in **Airflow Connections**.  
✅ **Better Maintainability** – Hooks abstract connection logic.
