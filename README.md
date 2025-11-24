# 🏦 Building Banking Ingestion Pipeline 

![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?logo=snowflake&logoColor=white)
![DBT](https://img.shields.io/badge/dbt-FF694B?logo=dbt&logoColor=white)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?logo=apacheairflow&logoColor=white)
![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?logo=apachekafka&logoColor=white)
![Debezium](https://img.shields.io/badge/Debezium-EF3B2D?logo=apache&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?logo=git&logoColor=white)
![CI/CD](https://img.shields.io/badge/CI%2FCD-000000?logo=githubactions&logoColor=white)

---

## 📌 What this project is

This repo demonstrates a modern data stack pipeline for a banking domain using:
OLTP source in Postgres
Change Data Capture (CDC) via Debezium → Kafka
Landing layer as partitioned Parquet on S3-compatible MinIO
Warehouse loading into Snowflake via Airflow
Transformations + SCD Type-2 history using dbt snapshots
Marts (dimensions + fact table) built incrementally
CI/CD that validates and deploys dbt models to Snowflake

👉 Even though the project runs locally with Docker, everything is designed to map cleanly to AWS production patterns (see AWS section below).

---

## 🎯 Problem statement

Banks need analytics on customers, accounts, and transactions without running heavy queries on OLTP systems and while keeping historical changes (ex: account balance or customer email updates).
This project solves:

Reliable ingestion from transactional source
Near-real-time updates using CDC
Cost-friendly lake landing (Parquet + partitions)
Clean modeling into facts/dimensions
SCD2 history for audit + compliance
Automated orchestration + deployment

---

## 🏗️ Architecture  

<img width="5647" height="3107" alt="Architecture" src="https://github.com/user-attachments/assets/7521ea8a-451e-46ff-9db0-71dd6ddf8181" />


**Pipeline Flow:**
1. **Data Generator** → Simulates banking transactions, accounts & customers (via Faker).  
2. **Kafka + Debezium** → Streams change data (CDC) into MinIO (S3-compatible storage).  
3. **Airflow** → Orchestrates data ingestion & snapshots into Snowflake.  
4. **Snowflake** → Cloud Data Warehouse (Bronze → Silver → Gold).  
5. **DBT** → Applies transformations, builds marts & snapshots (SCD Type-2).  
6. **CI/CD with GitHub Actions** → Automated tests, build & deployment.  

---

## ⚡ Tech Stack
- **Snowflake** → Cloud Data Warehouse  
- **DBT** → Transformations, testing, snapshots (SCD Type-2)  
- **Apache Airflow** → Orchestration & DAG scheduling  
- **Apache Kafka + Debezium** → Real-time streaming & CDC  
- **MinIO** → S3-compatible object storage  
- **Postgres** → Source OLTP system  
- **Python (Faker)** → Data simulation  
- **Docker & docker-compose** → Containerized setup  
- **Git & GitHub Actions** → CI/CD workflows  

---

## ✅ Key Features
- **PostgreSQL OLTP**: Source relational database with ACID guarantees (customers, accounts, transactions)  
- **Simulated banking system**: customers, accounts, and transactions  
- **Change Data Capture (CDC)** via Kafka + Debezium (capturing Postgres WAL)  
- **Raw → Staging → Fact/Dimension** models in DBT  
- **Snapshots for history tracking** (slowly changing dimensions)  
- **Automated pipeline orchestration** using Airflow  
- **CI/CD pipeline** with dbt tests + GitHub Actions  

---

## 📂 Repository Structure
```text
banking-modern-datastack/
├── .github/workflows/         # CI/CD pipelines (ci.yml, cd.yml)
├── banking_dbt/              # DBT project
│   ├── models/
│   │   ├── staging/           # Staging models
│   │   ├── marts/             # Facts & dimensions
│   │   └── sources.yml
│   ├── snapshots/             # SCD2 snapshots
│   └── dbt_project.yml
├── consumer
│   └── kafka_to_minio.py
├── data-generator/            # Faker-based data simulator
│   └── faker_generator.py
├── docker/                    # Airflow DAGs, plugins, etc.
│   ├── dags/                  # DAGs (minio_to_snowflake, scd_snapshots)
├── kafka-debezium/            # Kafka connectors & CDC logic
│   └── generate_and_post_connector.py
├── postgres/                  # Postgres schema (OLTP DDL & seeds)
│   └── schema.sql
├── .gitignore
├── docker-compose.yml         # Containerized infra
├── dockerfile-airflow.dockerfile
├── requirements.txt
└── README.md
```

---

## ⚙️ Step-by-Step Implementation  

### **1. Data Simulation**  
- Generated synthetic banking data (**customers, accounts, transactions**) using **Faker**.  
- Inserted data into **PostgreSQL (OLTP)** so the system behaves like a real transactional database (**ACID, constraints**).  
- Controlled generation via `config.yaml`.  

---

### **2. Kafka + Debezium CDC**  
- Set up **Kafka Connect & Debezium** to capture changes from **Postgres**.  
- Streamed **CDC events** into **MinIO**.  

---

### **3. Airflow Orchestration**  
- Built DAGs to:  
  - Ingest **MinIO data → Snowflake (Bronze)**.  
  - Schedule **snapshots & incremental loads**.  

---

### **4. Snowflake Warehouse**  
- Organized into **Bronze → Silver → Gold layers**.  
- Created **staging schemas** for ingestion.  

---

### **5. DBT Transformations**  
- **Staging models** → cleaned source data.  
- **Dimension & fact models** → built marts.  
- **Snapshots** → tracked history of accounts & customers.  

---

### **6. CI/CD with GitHub Actions**  
- **ci.yml** → Lint, dbt compile, run tests.  
- **cd.yml** → Deploy DAGs & dbt models on merge.  

---

## 📊 Final Deliverables  
- **Automated CDC pipeline** from Postgres → Snowflake  
- **DBT models** (facts, dimensions, snapshots)  
- **Orchestrated DAGs in Airflow**  
- **Synthetic banking dataset** for demos  
- **CI/CD workflows** ensuring reliability  

---

## 👤 About the Author

**Deeba Farheen H N**  
Data Engineer | M.S. Data Science @ University of Michigan  
I specialize in building scalable data systems and transforming raw operational data into actionable intelligence.

- 🔗 [LinkedIn](https://linkedin.com/in/deeba-farheen-h-n)
- 💻 [GitHub](https://github.com/deebafar04)
- 📫 [deeba@umich.edu](mailto:deeba@umich.edu)

> 🌟 If you found this project helpful or insightful, feel free to star the repo and connect!
