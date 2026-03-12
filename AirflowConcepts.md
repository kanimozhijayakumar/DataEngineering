### **Advanced Apache Airflow Concepts for Data Engineers 🚀**

As an **experienced Data Engineer**, you should master **Apache Airflow** beyond just creating DAGs. You need to understand **scalability, security, advanced scheduling, data lineage, and best practices** for running production-grade pipelines.

---

## **1. Airflow Architecture Deep Dive**
Understanding Airflow’s **distributed execution** is key to scaling pipelines.

### **🔹 Core Components**
| Component | Description |
|-----------|------------|
| **Scheduler** | Manages DAG execution, schedules tasks, and handles retries. |
| **Executor** | Determines where tasks run (LocalExecutor, CeleryExecutor, KubernetesExecutor). |
| **Web Server (UI)** | Web interface to monitor DAGs, logs, and failures. |
| **Metadata Database** | Stores DAG definitions, task status, and connections. |
| **Workers** | Execute tasks in distributed mode (Celery/K8s). |

### **🔹 Executors (Task Execution Modes)**
| Executor | When to Use |
|----------|------------|
| **LocalExecutor** | Small-scale deployments (single machine). |
| **CeleryExecutor** | **Distributed execution** using message queues (RabbitMQ, Redis). |
| **KubernetesExecutor** | **Dynamic scaling**, each task runs in a separate K8s pod. |
| **DaskExecutor** | Parallel task execution using Dask clusters. |

---

## **2. DAG Optimization for Large Workflows**
### **🔹 Parallel Processing & Task Concurrency**
1. **Increase `max_active_runs_per_dag`** – Controls how many DAG runs can execute simultaneously.
2. **Use `max_active_tasks`** – Sets max tasks per DAG to avoid overwhelming the system.
3. **Use `depends_on_past=False`** – Ensures tasks don't wait unnecessarily.
4. **Task Parallelism** – Split large tasks into multiple smaller tasks.

```python
dag = DAG(
    dag_id="optimized_dag",
    max_active_runs=3,  # Allow 3 concurrent runs
    concurrency=8,  # Allow 8 parallel tasks
    schedule_interval="@hourly",
    start_date=datetime(2024, 3, 1),
)
```

### **🔹 Dynamic DAG Generation (Avoid Hardcoded DAGs)**
Instead of **manually creating 100s of DAGs**, generate them dynamically.

```python
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime

dag_list = ["dag1", "dag2", "dag3"]

for dag_id in dag_list:
    dag = DAG(
        dag_id=dag_id,
        schedule_interval="@daily",
        start_date=datetime(2024, 3, 1),
        catchup=False
    )

    task = BashOperator(
        task_id="echo_task",
        bash_command=f"echo Running {dag_id}",
        dag=dag
    )
```
This will **dynamically create multiple DAGs**.

---

## **3. XComs and Data Passing Between Tasks**
XComs (Cross-Communications) allow tasks to share data.

### **🔹 Push & Pull Data in DAGs**
```python
from airflow.operators.python_operator import PythonOperator
from airflow import DAG
from datetime import datetime

def push_data(**kwargs):
    kwargs['ti'].xcom_push(key='message', value='Hello from Task A')

def pull_data(**kwargs):
    message = kwargs['ti'].xcom_pull(task_ids='task_a', key='message')
    print(f"Received: {message}")

dag = DAG("xcom_example", start_date=datetime(2024, 3, 1), schedule_interval="@daily")

task_a = PythonOperator(task_id="task_a", python_callable=push_data, provide_context=True, dag=dag)
task_b = PythonOperator(task_id="task_b", python_callable=pull_data, provide_context=True, dag=dag)

task_a >> task_b  # Task dependency
```
- Task **A** pushes data into XCom.
- Task **B** retrieves data.

---

## **4. Sensors for Event-Driven Pipelines**
Sensors **wait for external events** before proceeding.

### **🔹 Example: Wait for a file in S3**
```python
from airflow.providers.amazon.aws.sensors.s3_key import S3KeySensor
from airflow import DAG
from datetime import datetime

dag = DAG("s3_sensor_dag", start_date=datetime(2024, 3, 1), schedule_interval="@daily")

s3_sensor = S3KeySensor(
    task_id="wait_for_file",
    bucket_name="my-bucket",
    bucket_key="data.csv",
    aws_conn_id="aws_default",
    poke_interval=60,  # Check every 60 seconds
    timeout=3600,  # Stop after 1 hour
    dag=dag
)
```
- The DAG **waits until `data.csv` appears in S3** before proceeding.

---

## **5. Task Dependency Management**
### **🔹 Trigger Rules for Complex Dependencies**
| Trigger Rule | Description |
|-------------|-------------|
| `all_success` | Runs only if **all upstream tasks** succeed (default). |
| `one_success` | Runs if **at least one upstream task** succeeds. |
| `all_failed` | Runs only if **all upstream tasks fail**. |
| `none_failed` | Runs if **no upstream task has failed**. |

```python
task_c = PythonOperator(task_id="task_c", python_callable=my_func, dag=dag)
task_c.trigger_rule = "one_success"
```
- **`task_c` runs even if one of its upstream tasks succeeds**.

---

## **6. External Task Triggers (Trigger DAGs from Other DAGs)**
DAGs can **trigger other DAGs dynamically**.

```python
from airflow.operators.trigger_dagrun import TriggerDagRunOperator

trigger_task = TriggerDagRunOperator(
    task_id="trigger_another_dag",
    trigger_dag_id="target_dag",
    wait_for_completion=True,
    dag=dag
)
```
- **`trigger_another_dag` runs `target_dag`**.

---

## **7. Scaling & High Availability**
### **🔹 Celery Executor for Distributed Execution**
1. **Use `CeleryExecutor`** for large workloads:
   - Tasks run on separate workers.
   - Uses Redis/RabbitMQ for message queuing.

```ini
executor = CeleryExecutor
broker_url = redis://localhost:6379/0
result_backend = db+postgresql://airflow:airflow@localhost:5432/airflow
```

### **🔹 Kubernetes Executor for Auto-Scaling**
- **Each Airflow task runs as an isolated K8s pod**.
- No need for Celery Workers.

```ini
executor = KubernetesExecutor
```

---

## **8. Monitoring & Alerts**
### **🔹 Set Up Slack Alerts**
Notify failures in **Slack**.

```python
from airflow.providers.slack.hooks.slack_webhook import SlackWebhookHook

def slack_alert(context):
    slack_hook = SlackWebhookHook(slack_webhook_conn_id="slack_conn")
    slack_hook.send(text=f"Task Failed: {context['task_instance'].task_id}")

task_a = PythonOperator(
    task_id="task_a",
    python_callable=my_func,
    on_failure_callback=slack_alert,
    dag=dag
)
```

---

## **9. Airflow Data Lineage & Metadata Management**
For tracking **data flow across DAGs**:
- Use **OpenLineage Integration** for **data lineage visualization**.
- Store DAG run metadata in **Google BigQuery** for tracking.

---

## **10. Security Best Practices**
### **🔹 Secrets Management**
1. **Store secrets in AWS Secrets Manager, HashiCorp Vault, or Airflow Variables.**
2. **Use Environment Variables Instead of Hardcoded Credentials.**

```bash
export POSTGRES_PASSWORD="my_secure_password"
```
Access in DAG:
```python
import os
password = os.getenv("POSTGRES_PASSWORD")
```

---

## **Final Thoughts**
As a **Senior Data Engineer**, your expertise in Airflow should include:
✅ **Scaling with Celery & Kubernetes Executors.**  
✅ **Optimizing DAGs for performance & parallelism.**  
✅ **Building event-driven data pipelines with Sensors & Triggers.**  
✅ **Ensuring security & monitoring (Slack alerts, OpenLineage).**  
✅ **Leveraging dynamic DAG creation for scalability.**  
