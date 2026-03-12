In **Apache Airflow**, there are different types of **variables** and ways to store and retrieve them for managing configurations dynamically.

### 1. **Airflow Variables**
   - Used for storing **key-value pairs** globally across DAGs.
   - Stored in **Airflow Metadata Database**.
   - Can be accessed using:
     ```python
     from airflow.models import Variable
     my_var = Variable.get("my_variable_key")
     ```
   - **Types:**
     - **String** (default)
     - **JSON** (if `deserialize_json=True`)

   **Example:**
   ```python
   my_json_var = Variable.get("my_json_variable", deserialize_json=True)
   print(my_json_var["key_name"])
   ```

   - Can be **set** via CLI:
     ```bash
     airflow variables set my_variable_key "my_value"
     ```
   - Can be accessed via the **Airflow UI** (`Admin -> Variables`).

---

### 2. **Environment Variables**
   - Used for global configurations across Airflow instances.
   - Can be set in the system and accessed inside DAGs.
   - Useful for **storing secrets** instead of exposing them in DAGs.

   **Example:**
   ```python
   import os
   my_env_var = os.getenv("MY_ENV_VARIABLE")
   ```
   - To set an environment variable:
     ```bash
     export MY_ENV_VARIABLE="my_secret_value"
     ```

---

### 3. **XCom (Cross-Communication Variables)**
   - Used for passing small pieces of data **between tasks** within a DAG.
   - Stored in the **Airflow Metadata Database**.
   - Retrieved using `xcom_push()` and `xcom_pull()`.

   **Example (Push Data from Task A):**
   ```python
   from airflow.operators.python_operator import PythonOperator

   def push_xcom_value(**kwargs):
       kwargs['ti'].xcom_push(key='my_key', value='Hello XCom')

   task_a = PythonOperator(
       task_id='push_xcom',
       python_callable=push_xcom_value,
       provide_context=True,
       dag=dag
   )
   ```

   **Example (Pull Data in Task B):**
   ```python
   def pull_xcom_value(**kwargs):
       value = kwargs['ti'].xcom_pull(task_ids='push_xcom', key='my_key')
       print(f"Received: {value}")

   task_b = PythonOperator(
       task_id='pull_xcom',
       python_callable=pull_xcom_value,
       provide_context=True,
       dag=dag
   )
   ```

---

### 4. **Airflow Connections (for Credentials & External Services)**
   - Stored in Airflow's metadata database.
   - Used for securely storing **database credentials, API keys, cloud service connections, etc.**
   - Managed via **Airflow UI (`Admin -> Connections`)**.
   - Accessed in DAGs using `BaseHook`.

   **Example:**
   ```python
   from airflow.hooks.base import BaseHook

   conn = BaseHook.get_connection("my_connection_id")
   print(conn.host, conn.login, conn.password)
   ```

---

### 5. **DAG Parameters**
   - Passed when triggering a DAG manually or via API.
   - Can be used instead of variables for dynamic configurations.

   **Example:**
   ```python
   from airflow.models.param import Param

   default_args = {
       'owner': 'airflow',
       'start_date': datetime(2024, 1, 1),
   }

   dag = DAG(
       dag_id="dag_with_params",
       default_args=default_args,
       params={"my_param": Param("default_value", type="string")},
       schedule_interval=None,
   )

   def print_param(**kwargs):
       param_value = kwargs['dag_run'].conf.get('my_param', 'default_value')
       print(f"Parameter Value: {param_value}")

   task = PythonOperator(
       task_id="print_param_task",
       python_callable=print_param,
       provide_context=True,
       dag=dag
   )
   ```

---

## **Summary**
| Type                | Storage Location        | Purpose |
|---------------------|------------------------|---------|
| **Airflow Variables** | Airflow Metadata DB | Store key-value pairs across DAGs |
| **Environment Variables** | System/Container | Store global config and secrets |
| **XCom Variables** | Airflow Metadata DB | Pass data between tasks in a DAG |
| **Airflow Connections** | Airflow Metadata DB | Store credentials for external services |
| **DAG Parameters** | Runtime (API/UI) | Dynamically set values when triggering a DAG |
