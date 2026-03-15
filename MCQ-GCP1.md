

### **1. Which GCP service is best suited for real-time stream processing?**
A) Cloud Dataproc  
B) Cloud Dataflow  
C) Cloud Composer  
D) BigQuery  

**Answer:** **B) Cloud Dataflow**  
(Dataflow is designed for real-time and batch stream processing using Apache Beam.)

---

### **2. Which GCP service is a fully managed orchestration tool based on Apache Airflow?**
A) Cloud Dataflow  
B) Cloud Composer  
C) Cloud Dataproc  
D) Cloud Run  

**Answer:** **B) Cloud Composer**  
(Cloud Composer is GCP’s fully managed Airflow service for workflow automation.)

---

### **3. What is the main function of Cloud Pub/Sub?**
A) Running SQL queries on large datasets  
B) Storing structured relational data  
C) Event-driven messaging service  
D) Managing Kubernetes clusters  

**Answer:** **C) Event-driven messaging service**  
(Cloud Pub/Sub is used for asynchronous event-driven messaging between services.)

---

### **4. Which service is primarily used for batch data processing with Apache Spark and Hadoop?**
A) Cloud Dataflow  
B) Cloud Dataproc  
C) BigQuery  
D) Cloud Functions  

**Answer:** **B) Cloud Dataproc**  
(Dataproc is GCP’s managed Hadoop, Spark, and Flink service for big data processing.)

---

### **5. What is the primary purpose of BigQuery Data Transfer Service?**
A) Automate data imports into BigQuery  
B) Run serverless containerized applications  
C) Process real-time streaming data  
D) Manage Kubernetes workflows  

**Answer:** **A) Automate data imports into BigQuery**  
(The BigQuery Data Transfer Service automates data ingestion from Google services and external SaaS applications.)

---

### **6. Which GCP service allows you to define event-driven serverless functions?**
A) Cloud Run  
B) Cloud Functions  
C) Cloud Composer  
D) Cloud Workflows  

**Answer:** **B) Cloud Functions**  
(Cloud Functions is a serverless execution environment for event-driven applications.)

---

### **7. What is a key advantage of using Cloud Dataflow over Cloud Dataproc?**
A) Dataflow is based on Apache Spark  
B) Dataflow supports real-time stream processing  
C) Dataflow requires manual scaling  
D) Dataflow is only for batch processing  

**Answer:** **B) Dataflow supports real-time stream processing**  
(Dataflow supports both real-time and batch processing, while Dataproc is optimized for batch jobs.)

---

### **8. Which service is best for automating ETL workflows across multiple GCP services?**
A) Cloud Functions  
B) Cloud Composer  
C) Cloud Pub/Sub  
D) Cloud Run  

**Answer:** **B) Cloud Composer**  
(Cloud Composer automates ETL workflows using Apache Airflow.)

---

### **9. What is the key feature of Cloud Workflows?**
A) Run managed Kubernetes applications  
B) Connect multiple GCP services using API calls  
C) Store and process large-scale structured data  
D) Ingest and process event-driven data  

**Answer:** **B) Connect multiple GCP services using API calls**  
(Cloud Workflows is used for orchestrating complex tasks between multiple cloud services.)

---

### **10. If you need to process IoT sensor data in real time, which combination of GCP services would be ideal?**
A) Cloud Dataproc + Cloud SQL  
B) Cloud Storage + BigQuery  
C) Cloud Pub/Sub + Cloud Dataflow  
D) Cloud Run + BigQuery  

**Answer:** **C) Cloud Pub/Sub + Cloud Dataflow**  
(Pub/Sub collects real-time events, and Dataflow processes them for analytics.)

---

### **11. Which GCP service allows you to run containerized applications in a fully managed environment?**
A) Cloud Functions  
B) Cloud Run  
C) Cloud Composer  
D) Cloud Dataflow  

**Answer:** **B) Cloud Run**  
(Cloud Run is GCP’s serverless platform for running containers.)

---

### **12. What is the main difference between Cloud Functions and Cloud Run?**
A) Cloud Functions supports only Python  
B) Cloud Run is for containerized applications, while Cloud Functions runs individual functions  
C) Cloud Functions is for big data processing, while Cloud Run is for databases  
D) Cloud Run cannot scale automatically  

**Answer:** **B) Cloud Run is for containerized applications, while Cloud Functions runs individual functions**  
(Cloud Run supports full applications in containers, while Cloud Functions is for small, event-driven code snippets.)

---

### **13. If you need to schedule a data pipeline that runs every day at a specific time, which service is best?**
A) Cloud Dataflow  
B) Cloud Dataproc  
C) Cloud Composer  
D) Cloud Pub/Sub  

**Answer:** **C) Cloud Composer**  
(Cloud Composer allows scheduling and orchestration of data workflows using Airflow DAGs.)

---

### **14. Which GCP service provides "exactly-once" message delivery?**
A) Cloud Pub/Sub  
B) Cloud Functions  
C) Cloud Composer  
D) Cloud Dataflow  

**Answer:** **A) Cloud Pub/Sub**  
(Pub/Sub provides at-least-once and exactly-once message delivery guarantees.)

---

### **15. What is the best choice for running Apache Spark ML models in GCP?**
A) Cloud Dataflow  
B) Cloud Dataproc  
C) Cloud Composer  
D) Cloud SQL  

**Answer:** **B) Cloud Dataproc**  
(Dataproc supports Spark MLlib for large-scale machine learning workloads.)

Here are more **scenario-based multiple-choice questions** (MCQs) for **Google Cloud Data Pipelines**, covering real-world use cases.

---

### **16. You need to process terabytes of log data daily using Apache Spark. Which GCP service is best?**
A) Cloud Dataflow  
B) Cloud Dataproc  
C) Cloud Composer  
D) Cloud Functions  

**Answer:** **B) Cloud Dataproc**  
(Cloud Dataproc is optimized for large-scale **batch processing** using Apache Spark and Hadoop.)

---

### **17. A company needs to move its data from an on-premises MySQL database to BigQuery on a daily schedule. Which GCP service is best?**
A) Cloud Composer  
B) Cloud Functions  
C) BigQuery Data Transfer Service  
D) Cloud Run  

**Answer:** **A) Cloud Composer**  
(Cloud Composer is an **Airflow-based orchestration tool** that can schedule and automate data movement.)

---

### **18. You need to trigger a Cloud Function whenever a file is uploaded to Cloud Storage. What should you use?**
A) Cloud Pub/Sub  
B) Cloud Composer  
C) Cloud Dataproc  
D) BigQuery  

**Answer:** **A) Cloud Pub/Sub**  
(Pub/Sub **notifies** Cloud Functions of events, such as file uploads to Cloud Storage.)

---

### **19. A company is processing high-velocity IoT sensor data and requires low-latency processing. Which solution is best?**
A) Cloud Dataproc  
B) Cloud Dataflow + Pub/Sub  
C) Cloud Storage + BigQuery  
D) Cloud Functions  

**Answer:** **B) Cloud Dataflow + Pub/Sub**  
(Pub/Sub ingests IoT data in real time, and Dataflow **processes** it for low-latency insights.)

---

### **20. You need to build a cost-efficient data pipeline for batch ETL workloads. Which GCP service should you use?**
A) Cloud Dataproc  
B) Cloud Dataflow  
C) Cloud Functions  
D) Cloud Run  

**Answer:** **A) Cloud Dataproc**  
(Dataproc is **cost-efficient** for large-scale **batch** ETL processing with Hadoop/Spark.)

---

### **21. Your company needs to run a machine learning inference pipeline that processes real-time events from Cloud Pub/Sub. Which service should you use?**
A) Cloud Dataproc  
B) Cloud Dataflow  
C) Cloud Composer  
D) BigQuery ML  

**Answer:** **B) Cloud Dataflow**  
(Dataflow supports real-time **stream processing** and ML inference at scale.)

---

### **22. If you need to store structured, relational data for an OLTP (Online Transaction Processing) system, which GCP service is best?**
A) Cloud Spanner  
B) Cloud Storage  
C) Cloud Bigtable  
D) Cloud Dataflow  

**Answer:** **A) Cloud Spanner**  
(Cloud Spanner is a **relational, globally scalable** database designed for OLTP workloads.)

---

### **23. Your team needs to execute a Python-based workflow that includes multiple BigQuery jobs and data transfers. Which GCP service should you use?**
A) Cloud Composer  
B) Cloud Run  
C) Cloud Pub/Sub  
D) Cloud Functions  

**Answer:** **A) Cloud Composer**  
(Composer is based on Apache Airflow and is ideal for scheduling **Python-based workflows**.)

---

### **24. A retail company needs to analyze customer purchase trends every hour. The data is streamed into BigQuery. Which GCP service is most suitable for this task?**
A) Cloud Storage  
B) Cloud Dataproc  
C) Cloud Dataflow  
D) Cloud Functions  

**Answer:** **C) Cloud Dataflow**  
(Dataflow is optimized for **real-time analytics** and integrates well with BigQuery.)

---

### **25. A company needs to schedule daily ETL jobs that move data from Cloud Storage to BigQuery. What’s the best option?**
A) Cloud Run  
B) Cloud Dataproc  
C) Cloud Composer  
D) Cloud Functions  

**Answer:** **C) Cloud Composer**  
(Cloud Composer automates **scheduled ETL workflows** for moving data between services.)

---

### **26. Your application needs to process customer chat messages and trigger actions based on keywords in real time. Which GCP service is best?**
A) Cloud Dataproc  
B) Cloud Dataflow  
C) Cloud Functions  
D) Cloud Storage  

**Answer:** **B) Cloud Dataflow**  
(Dataflow is ideal for **real-time text processing** with AI/ML integrations.)

---

### **27. Your company needs a fully managed, scalable SQL data warehouse. Which service should you choose?**
A) Cloud SQL  
B) BigQuery  
C) Cloud Spanner  
D) Cloud Dataproc  

**Answer:** **B) BigQuery**  
(BigQuery is GCP’s **serverless, highly scalable** data warehouse.)

---

### **28. You need to migrate an on-premises PostgreSQL database to a managed cloud database with high availability. Which service should you use?**
A) Cloud SQL  
B) BigQuery  
C) Cloud Dataproc  
D) Cloud Functions  

**Answer:** **A) Cloud SQL**  
(Cloud SQL provides **managed PostgreSQL** with high availability and automatic backups.)

---

### **29. If you need to build a real-time fraud detection pipeline for financial transactions, which services should you use?**
A) Cloud Dataproc + Cloud Functions  
B) Cloud Dataflow + Pub/Sub  
C) Cloud Storage + Cloud Run  
D) Cloud Composer + BigQuery  

**Answer:** **B) Cloud Dataflow + Pub/Sub**  
(Pub/Sub captures transactions, and Dataflow processes them in real-time for fraud detection.)

---

### **30. You need to analyze customer behavioral data from a web application and generate daily reports. What should you use?**
A) Cloud Dataproc  
B) Cloud Composer + BigQuery  
C) Cloud Functions  
D) Cloud Pub/Sub  

**Answer:** **B) Cloud Composer + BigQuery**  
(Composer schedules **daily ETL jobs**, and BigQuery stores/analyzes behavioral data.)

---

### **31. A company wants to migrate its Kafka-based event streaming platform to GCP. Which service is the best replacement?**
A) Cloud Functions  
B) Cloud Pub/Sub  
C) Cloud Dataproc  
D) Cloud SQL  

**Answer:** **B) Cloud Pub/Sub**  
(Pub/Sub is GCP’s alternative to Apache Kafka for **real-time event streaming**.)

---

### **32. A team needs a scalable NoSQL database for real-time analytics. Which GCP service is best?**
A) Cloud SQL  
B) Cloud Spanner  
C) Cloud Bigtable  
D) BigQuery  

**Answer:** **C) Cloud Bigtable**  
(Cloud Bigtable is a **NoSQL** database optimized for real-time, high-throughput workloads.)

---

### **33. Your application processes millions of log entries per second and stores aggregated results in BigQuery. Which service should you use?**
A) Cloud Dataproc  
B) Cloud Dataflow  
C) Cloud Storage  
D) Cloud SQL  

**Answer:** **B) Cloud Dataflow**  
(Dataflow efficiently processes massive streaming logs and integrates with BigQuery.)

---

### **34. A company needs to run ad-hoc SQL queries on petabytes of structured data. Which service is best?**
A) Cloud SQL  
B) Cloud Bigtable  
C) BigQuery  
D) Cloud Spanner  

**Answer:** **C) BigQuery**  
(BigQuery is designed for **serverless, high-performance SQL queries** on massive datasets.)

---

### **35. You need to develop a low-latency API that processes incoming streaming data and responds in real-time. Which service is best?**
A) Cloud Dataproc  
B) Cloud Functions  
C) Cloud Run  
D) Cloud Dataflow  

**Answer:** **C) Cloud Run**  
(Cloud Run is **ideal for real-time APIs**, as it supports fully managed containers.)

---

### **36. A company needs to migrate real-time data from their on-premise Kafka cluster to GCP. Which service should they use?**  
A) Cloud Composer  
B) Cloud Pub/Sub  
C) BigQuery  
D) Cloud Dataproc  

**Answer:** **B) Cloud Pub/Sub**  
(Cloud Pub/Sub is GCP's alternative to Kafka for real-time messaging and event-driven architectures.)

---

### **37. You have a machine learning model that processes streaming customer interactions. Which GCP service is best for deploying the model in a data pipeline?**  
A) Cloud Functions  
B) Cloud Dataflow  
C) Vertex AI + Cloud Dataflow  
D) BigQuery ML  

**Answer:** **C) Vertex AI + Cloud Dataflow**  
(Vertex AI handles ML model deployment, while Dataflow processes the real-time data for inference.)

---

### **38. Which of the following is NOT a valid use case for Cloud Dataproc?**  
A) Large-scale batch processing with Apache Spark  
B) Real-time streaming of sensor data  
C) Running machine learning jobs using Hadoop  
D) ETL operations on large datasets  

**Answer:** **B) Real-time streaming of sensor data**  
(Dataproc is best for batch processing, while **Cloud Dataflow** is preferred for real-time streaming.)

---

### **39. A business needs to create a data pipeline that automatically moves new Google Ads data into BigQuery daily. Which GCP service is best?**  
A) Cloud Composer  
B) BigQuery Data Transfer Service  
C) Cloud Functions  
D) Cloud Dataproc  

**Answer:** **B) BigQuery Data Transfer Service**  
(This service automates scheduled transfers from Google Ads, YouTube, and other SaaS apps to BigQuery.)

---

### **40. Your company wants to collect logs from multiple microservices in real-time and analyze them in BigQuery. What’s the best combination of GCP services?**  
A) Cloud Storage + BigQuery  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Functions + Cloud Composer  
D) Cloud Run + Cloud SQL  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Pub/Sub collects logs, Dataflow processes them, and BigQuery stores them for analysis.)

---

### **41. Which GCP service provides built-in support for managing DAG-based workflows?**  
A) Cloud Composer  
B) Cloud Dataflow  
C) Cloud Run  
D) Cloud Storage  

**Answer:** **A) Cloud Composer**  
(Cloud Composer is built on **Apache Airflow** and is used for scheduling and managing **DAG-based workflows**.)

---

### **42. A fintech company needs to process millions of financial transactions per second for fraud detection. What’s the best GCP architecture?**  
A) Cloud Functions + BigQuery  
B) Cloud Dataflow + Pub/Sub + BigQuery  
C) Cloud Run + Cloud SQL  
D) Cloud Storage + Cloud Dataproc  

**Answer:** **B) Cloud Dataflow + Pub/Sub + BigQuery**  
(Dataflow processes the real-time transactions from **Pub/Sub**, and **BigQuery** is used for analytics.)

---

### **43. A retail company needs to schedule a daily pipeline that transfers customer data from Cloud Storage to BigQuery. What’s the best solution?**  
A) Cloud Dataflow  
B) Cloud Composer  
C) Cloud Pub/Sub  
D) Cloud Functions  

**Answer:** **B) Cloud Composer**  
(Cloud Composer automates **daily scheduled ETL workflows** using Apache Airflow.)

---

### **44. Which GCP service is best suited for interactive querying of large-scale structured data?**  
A) Cloud Bigtable  
B) Cloud SQL  
C) Cloud Dataproc  
D) BigQuery  

**Answer:** **D) BigQuery**  
(BigQuery is optimized for **fast, interactive querying** on massive datasets.)

---

### **45. You need to process JSON event logs in real-time and store them in Cloud Bigtable for further analysis. Which GCP service should you use?**  
A) Cloud Composer  
B) Cloud Dataflow  
C) Cloud Storage  
D) Cloud SQL  

**Answer:** **B) Cloud Dataflow**  
(Dataflow is best for **real-time streaming processing**, and **Bigtable** is used for storing large-scale, high-throughput NoSQL data.)

---

### **46. A gaming company wants to analyze live player events and generate personalized recommendations in real-time. Which services should they use?**  
A) Cloud Dataproc + Cloud Spanner  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Functions + Cloud SQL  
D) Cloud Storage + Cloud Composer  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Pub/Sub ingests real-time player events, Dataflow processes them, and BigQuery analyzes them for recommendations.)

---

### **47. A company wants to process streaming IoT sensor data and make decisions in real time. What is the best service combination?**  
A) Cloud Storage + BigQuery  
B) Cloud Pub/Sub + Cloud Dataflow  
C) Cloud Dataproc + Cloud Composer  
D) Cloud SQL + Cloud Run  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow**  
(Pub/Sub captures **real-time IoT data**, and Dataflow processes it for analytics or decision-making.)

---

### **48. Your company wants a data lake for storing raw unstructured data before processing it in a pipeline. Which service should you use?**  
A) Cloud SQL  
B) Cloud Storage  
C) Cloud Bigtable  
D) Cloud Spanner  

**Answer:** **B) Cloud Storage**  
(Cloud Storage is GCP’s recommended **data lake** for storing unstructured/big data.)

---

### **49. Which service is most suitable for long-term, low-cost data archival?**  
A) Cloud Spanner  
B) Cloud SQL  
C) Cloud Storage (Coldline or Archive)  
D) Cloud Bigtable  

**Answer:** **C) Cloud Storage (Coldline or Archive)**  
(Coldline/Archive storage is designed for **low-cost, long-term data retention**.)

---

### **50. You need to run SQL analytics on billions of rows in under a second. Which GCP service should you use?**  
A) Cloud SQL  
B) Cloud Bigtable  
C) BigQuery  
D) Cloud Dataproc  

**Answer:** **C) BigQuery**  
(BigQuery is optimized for **serverless SQL queries on petabyte-scale datasets**.)

---

### **51. You need a globally distributed, strongly consistent relational database for OLTP workloads. Which service should you choose?**  
A) Cloud SQL  
B) Cloud Bigtable  
C) Cloud Spanner  
D) BigQuery  

**Answer:** **C) Cloud Spanner**  
(Cloud Spanner is a **relational, highly available** database with global consistency.)

---

### **52. Your company needs to trigger an event-driven workflow when new files are uploaded to Cloud Storage. What’s the best GCP service?**  
A) Cloud Functions  
B) Cloud Run  
C) Cloud SQL  
D) BigQuery  

**Answer:** **A) Cloud Functions**  
(Cloud Functions can be triggered by Cloud Storage file uploads for **event-driven workflows**.)

---

### **53. A company needs to generate daily sales reports by aggregating data from multiple sources into a centralized warehouse. What’s the best GCP service?**  
A) Cloud Dataproc  
B) Cloud Composer + BigQuery  
C) Cloud Functions  
D) Cloud Storage  

**Answer:** **B) Cloud Composer + BigQuery**  
(Composer automates **daily ETL workflows**, and BigQuery stores & analyzes the data.)

---

### **54. A financial institution needs to ensure exactly-once message processing for transaction events. Which GCP service provides this guarantee?**  
A) Cloud Pub/Sub  
B) Cloud Dataflow  
C) Cloud Dataproc  
D) Cloud Composer  

**Answer:** **B) Cloud Dataflow**  
(Dataflow provides **exactly-once** message processing with Apache Beam.)

---

### **55. A company needs to migrate a high-volume transactional PostgreSQL database to the cloud with minimal downtime. Which service should they use?**  
A) Cloud SQL  
B) Cloud Spanner  
C) BigQuery  
D) Cloud Storage  

**Answer:** **A) Cloud SQL**  
(Cloud SQL provides managed PostgreSQL with replication and **minimal downtime migrations**.)

---

### **56. Your data pipeline needs to process JSON data from an API, transform it, and store it in BigQuery every 15 minutes. Which solution is best?**  
A) Cloud Functions + BigQuery  
B) Cloud Dataflow + Cloud Storage  
C) Cloud Composer + BigQuery  
D) Cloud SQL + BigQuery  

**Answer:** **C) Cloud Composer + BigQuery**  
(Cloud Composer schedules ETL workflows for fetching and transforming API data.)

---

### **57. You need to analyze video metadata and logs in real time for an online streaming platform. Which GCP services should you use?**  
A) Cloud Dataflow + Cloud Pub/Sub  
B) Cloud Storage + Cloud SQL  
C) Cloud Dataproc + Cloud Composer  
D) BigQuery + Cloud Functions  

**Answer:** **A) Cloud Dataflow + Cloud Pub/Sub**  
(Pub/Sub ingests **real-time video metadata**, and Dataflow processes logs for analytics.)

---

### **58. You are processing high-frequency IoT sensor data, but your pipeline is dropping events under heavy load. How can you improve reliability?**  
A) Increase Pub/Sub message retention  
B) Switch to Cloud Dataproc  
C) Use Cloud Run instead of Dataflow  
D) Reduce the Dataflow processing window  

**Answer:** **A) Increase Pub/Sub message retention**  
(Pub/Sub **retains messages** for 7 days by default, ensuring reliability under high loads.)

---

### **59. A company wants to provide real-time pricing recommendations for e-commerce customers. What’s the best GCP architecture?**  
A) Cloud Dataflow + Vertex AI  
B) Cloud Dataproc + Cloud Spanner  
C) Cloud Composer + BigQuery  
D) Cloud Run + Cloud SQL  

**Answer:** **A) Cloud Dataflow + Vertex AI**  
(Dataflow processes customer activity **in real-time**, and Vertex AI generates pricing predictions.)

---

### **60. Which GCP service allows you to orchestrate ETL workflows across multiple cloud services?**  
A) Cloud Functions  
B) Cloud Composer  
C) Cloud Storage  
D) Cloud Dataflow  

**Answer:** **B) Cloud Composer**  
(Cloud Composer **schedules** and **automates** multi-step ETL workflows using Apache Airflow.)

---

### **61. Your company needs a globally distributed database for managing user sessions. Which GCP service is best?**  
A) Cloud Spanner  
B) Cloud Bigtable  
C) Cloud SQL  
D) BigQuery  

**Answer:** **A) Cloud Spanner**  
(Cloud Spanner is a **globally distributed, ACID-compliant database** ideal for session management.)

---

### **62. A company wants to use an AI-based chatbot that analyzes live chat conversations and recommends responses. Which services are best?**  
A) Cloud Functions + Cloud Storage  
B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI  
C) Cloud Dataproc + Cloud SQL  
D) Cloud SQL + Cloud Composer  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI**  
(Pub/Sub ingests chat messages, Dataflow processes them, and Vertex AI generates **response recommendations**.)

---

### **63. Your application collects user activity logs and needs to analyze them weekly to detect anomalies. Which solution should you use?**  
A) Cloud Pub/Sub + Cloud Dataflow  
B) Cloud Storage + BigQuery + Cloud Composer  
C) Cloud Dataproc + Cloud SQL  
D) Cloud Run + Cloud Functions  

**Answer:** **B) Cloud Storage + BigQuery + Cloud Composer**  
(Logs are stored in **Cloud Storage**, analyzed in **BigQuery**, and scheduled with **Cloud Composer**.)

---

### **64. A company needs to automate a nightly ETL pipeline that extracts data from an API and loads it into BigQuery. Which tool is best?**  
A) Cloud Run  
B) Cloud Composer  
C) Cloud Dataflow  
D) Cloud Functions  

**Answer:** **B) Cloud Composer**  
(Cloud Composer schedules ETL jobs to fetch API data and load it into **BigQuery**.)

---

### **65. A financial services company needs a data pipeline that processes stock market data in real time and detects anomalies. What’s the best approach?**  
A) Cloud Dataproc + Cloud SQL  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Storage + Cloud Run  
D) Cloud Functions + Cloud Spanner  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Pub/Sub collects **real-time stock data**, Dataflow detects anomalies, and BigQuery stores insights.)

---

### **66. A marketing team wants to analyze customer sentiment from social media streams in real-time. Which solution is best?**  
A) Cloud Storage + Cloud SQL  
B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI  
C) Cloud Composer + BigQuery  
D) Cloud Dataproc + Cloud Functions  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI**  
(Pub/Sub collects **social media data**, Dataflow processes it, and Vertex AI runs **sentiment analysis**.)

---

### **67. A team wants to migrate their ETL workflows from on-premises Airflow to GCP. Which service should they use?**  
A) Cloud Run  
B) Cloud Functions  
C) Cloud Composer  
D) Cloud Dataflow  

**Answer:** **C) Cloud Composer**  
(Cloud Composer is **GCP’s managed Apache Airflow** service for ETL workflow automation.)

---

### **68. You need a scalable, managed service for real-time feature engineering for machine learning models. Which service should you use?**  
A) Cloud Composer  
B) Cloud Dataflow  
C) Cloud Dataproc  
D) BigQuery ML  

**Answer:** **B) Cloud Dataflow**  
(Dataflow is used for **real-time feature engineering** before model training.)

---

### **69. A healthcare company needs to analyze patient data in real-time for early disease detection. What’s the best approach?**  
A) Cloud Dataproc + Cloud SQL  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Functions + Cloud Run  
D) Cloud Storage + Cloud Composer  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Pub/Sub **ingests** real-time data, Dataflow **processes** it, and BigQuery **analyzes trends**.)

---

### **70. A company wants to migrate its existing Spark-based ETL jobs to GCP without rewriting them. Which service should they use?**  
A) Cloud Dataproc  
B) Cloud Dataflow  
C) Cloud Composer  
D) BigQuery  

**Answer:** **A) Cloud Dataproc**  
(Cloud Dataproc runs **Apache Spark** without requiring code changes.)

---
Here are **more advanced multiple-choice and scenario-based questions** focusing on **Google Cloud Data Pipelines**, **Big Data Processing**, and **Real-Time Analytics** in GCP.

---

### **71. A logistics company needs to track shipments in real-time and generate alerts for delays. Which GCP services should they use?**  
A) Cloud Pub/Sub + Cloud Dataflow + Cloud Functions  
B) Cloud Composer + BigQuery  
C) Cloud Storage + Cloud SQL  
D) Cloud Run + Cloud Dataproc  

**Answer:** **A) Cloud Pub/Sub + Cloud Dataflow + Cloud Functions**  
(Pub/Sub collects real-time shipment updates, Dataflow processes them, and Cloud Functions trigger alerts.)

---

### **72. A media company wants to transcribe thousands of videos daily and analyze speech data. What’s the best GCP approach?**  
A) Cloud Storage + Cloud SQL  
B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI Speech-to-Text  
C) Cloud Functions + BigQuery  
D) Cloud Dataproc + Cloud Composer  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI Speech-to-Text**  
(Pub/Sub streams audio, Dataflow processes it, and Vertex AI Speech-to-Text transcribes the content.)

---

### **73. A retailer wants to detect fraudulent transactions within seconds. Which architecture should they use?**  
A) Cloud Dataproc + Cloud Spanner  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Storage + Cloud Run  
D) Cloud Composer + Cloud SQL  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Pub/Sub ingests real-time transactions, Dataflow analyzes fraud patterns, and BigQuery stores results.)

---

### **74. Your team needs to create an AI-driven recommendation system that updates recommendations in real-time based on user activity. Which services should be used?**  
A) Cloud Pub/Sub + Cloud Dataflow + Vertex AI  
B) Cloud Composer + BigQuery  
C) Cloud SQL + Cloud Run  
D) Cloud Dataproc + Cloud Functions  

**Answer:** **A) Cloud Pub/Sub + Cloud Dataflow + Vertex AI**  
(Pub/Sub collects user actions, Dataflow processes them, and Vertex AI generates **real-time recommendations**.)

---

### **75. Your company needs a cloud-native solution for ad-hoc SQL queries on petabytes of structured data. What should you use?**  
A) Cloud SQL  
B) Cloud Spanner  
C) BigQuery  
D) Cloud Dataproc  

**Answer:** **C) BigQuery**  
(BigQuery is a **serverless, scalable data warehouse** designed for fast **SQL-based analytics** on huge datasets.)

---

### **76. A fintech startup wants to analyze high-frequency stock trading data and detect anomalies in real time. Which GCP services should be used?**  
A) Cloud Composer + BigQuery  
B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI  
C) Cloud Storage + Cloud SQL  
D) Cloud Run + Cloud Functions  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI**  
(Pub/Sub streams stock market data, Dataflow processes it, and Vertex AI detects anomalies.)

---

### **77. Your company wants to run ETL jobs that integrate data from multiple cloud providers and schedule them automatically. Which GCP service is best?**  
A) Cloud Dataproc  
B) Cloud Composer  
C) Cloud Functions  
D) Cloud Spanner  

**Answer:** **B) Cloud Composer**  
(Cloud Composer is a **multi-cloud workflow orchestrator** that automates ETL pipelines.)

---

### **78. A game development company needs to analyze player behavior in real-time and generate recommendations. What’s the best GCP architecture?**  
A) Cloud Storage + Cloud SQL  
B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI  
C) Cloud Dataproc + Cloud Functions  
D) Cloud Composer + BigQuery  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI**  
(Real-time **gameplay data** is streamed via Pub/Sub, processed in Dataflow, and analyzed using Vertex AI.)

---

### **79. A large-scale IoT platform needs to store sensor readings in a high-throughput, low-latency database. What’s the best option?**  
A) Cloud Spanner  
B) Cloud SQL  
C) Cloud Bigtable  
D) BigQuery  

**Answer:** **C) Cloud Bigtable**  
(Bigtable is a **NoSQL** database optimized for **high-throughput, real-time sensor data storage**.)

---

### **80. Your team is using Apache Spark for machine learning and big data analytics. Which GCP service allows you to run Spark jobs with minimal changes?**  
A) Cloud Composer  
B) Cloud Dataflow  
C) Cloud Dataproc  
D) Cloud Run  

**Answer:** **C) Cloud Dataproc**  
(Dataproc provides a **managed Apache Spark & Hadoop environment** for big data analytics.)

---

### **81. A video streaming platform wants to process millions of log events per second to detect unusual patterns. Which services should they use?**  
A) Cloud Functions + BigQuery  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Composer + Cloud Storage  
D) Cloud Run + Cloud SQL  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Pub/Sub ingests logs, Dataflow processes anomalies, and BigQuery enables fast querying.)

---

### **82. A financial institution needs to move sensitive customer data from Cloud Storage to BigQuery with **encryption and access control**. Which service should they use?**  
A) Cloud Functions  
B) Cloud Dataflow  
C) Cloud SQL  
D) Cloud Run  

**Answer:** **B) Cloud Dataflow**  
(Dataflow ensures **encryption at rest and in transit** and allows **fine-grained IAM controls**.)

---

### **83. A healthcare startup needs a pipeline that processes patient data in real-time for early disease detection. What’s the best approach?**  
A) Cloud Dataproc + Cloud SQL  
B) Cloud Pub/Sub + Cloud Dataflow + BigQuery  
C) Cloud Storage + Cloud Composer  
D) Cloud Run + Cloud Functions  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + BigQuery**  
(Real-time **health data is streamed via Pub/Sub**, processed using Dataflow, and stored in BigQuery.)

---

### **84. A manufacturing company wants to detect machine failures based on sensor data streams. Which services should they use?**  
A) Cloud Storage + BigQuery  
B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI  
C) Cloud Dataproc + Cloud SQL  
D) Cloud Run + Cloud Spanner  

**Answer:** **B) Cloud Pub/Sub + Cloud Dataflow + Vertex AI**  
(Pub/Sub captures sensor data, Dataflow processes it, and Vertex AI detects failures.)

---

### **85. A research team needs to analyze genomic data with Apache Spark in a scalable cloud environment. Which service should they use?**  
A) Cloud Composer  
B) Cloud Dataproc  
C) BigQuery  
D) Cloud SQL  

**Answer:** **B) Cloud Dataproc**  
(Dataproc is optimized for **Apache Spark-based genomic analysis** at scale.)

---

### **86. Your company needs to move hundreds of terabytes of historical data from on-premises to Google Cloud. Which service should they use?**  
A) Cloud Storage Transfer Service  
B) Cloud Dataflow  
C) Cloud Spanner  
D) Cloud SQL  

**Answer:** **A) Cloud Storage Transfer Service**  
(Cloud Storage Transfer Service is designed for **large-scale data migrations**.)

---

### **87. A retail company needs to process transaction logs and generate business reports every morning. Which GCP services should they use?**  
A) Cloud Storage + Cloud Composer + BigQuery  
B) Cloud Dataproc + Cloud SQL  
C) Cloud Run + Cloud Functions  
D) Cloud Pub/Sub + Vertex AI  

**Answer:** **A) Cloud Storage + Cloud Composer + BigQuery**  
(Logs are stored in **Cloud Storage**, Composer schedules ETL, and BigQuery analyzes sales trends.)

---





