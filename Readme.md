# 🌍 World Universities ETL Pipeline
**Apache Spark • Docker • PostgreSQL**

---

## 🎯 Global Project Role

This project implements an **end-to-end ETL pipeline** that ingests semi-structured JSON data containing worldwide university information, processes it using **Apache Spark**, and loads curated analytical tables into **PostgreSQL**.

The goal is to demonstrate:

- Data ingestion from JSON sources  
- Distributed data processing with Spark  
- Data quality checks and cleaning  
- Analytical aggregations  
- Loading curated datasets into a relational database  
- A reproducible, containerized environment using Docker  

This project reflects **real-world data engineering practices** and is suitable as a **portfolio project for junior data engineer roles**.

---

## 🐳 Docker Architecture & Steps (docker-compose)

Docker is used to ensure a **reproducible and isolated environment**.

### Services
- **Jupyter + Spark** (`jupyter/pyspark-notebook`)
- **PostgreSQL** (`postgres:16`)

### Steps
- Docker Compose starts both containers on the same Docker network  
- Volumes are mounted to share:
  - notebooks
  - input data
- Spark runs inside the Jupyter container
- PostgreSQL acts as the **serving layer** for analytics

### Architecture Flow
JSON File
    ↓
Spark (Docker)
    ↓
PostgreSQL (Docker)

### Start the environment
```bash
docker compose up -d
docker compose logs -f jupyter-spark
```
### What I Learned 🧠

Through this project, I gained hands-on experience with several core data engineering concepts and tools:

- Building an **end-to-end ETL pipeline** using Apache Spark
- Processing **semi-structured JSON data** with Spark (multi-line JSON handling)
- Applying **data quality checks** (nulls, duplicates, consistency of array fields)
- Performing **analytical aggregations** using Spark SQL
- Loading curated datasets into **PostgreSQL using JDBC**
- Working with **Docker and Docker Compose** to create reproducible data environments
- Understanding the interaction between Spark, Jupyter, and relational databases in a containerized setup

This project helped me move from simple data cleaning to a **production-oriented ETL mindset**.

---

## ⚠️ Challenges Faced & How I Solved Them

### PostgreSQL JDBC Driver Issue in Spark (JAR Problem)

**Problem:**  
When loading data from Spark into PostgreSQL, the pipeline failed with the following error:

ClassNotFoundException: org.postgresql.Driver

This occurred because **Apache Spark did not have access to the PostgreSQL JDBC driver** inside the Jupyter Docker container.

**Root Cause:**  
- Spark requires the PostgreSQL JDBC driver (`.jar`) to communicate with PostgreSQL.
- The default `jupyter/pyspark-notebook` image does not include this driver.
- Restarting SparkSession inside a notebook does not always reload JVM dependencies.

**Solution Implemented:**  
1. Downloaded the PostgreSQL JDBC driver (`postgresql-42.7.3.jar`) inside the container.
2. Added the JAR explicitly to Spark’s classpath.
3. Restarted the Jupyter kernel to ensure Spark loaded the driver correctly.
4. Verified the driver availability using:
   ```python
   spark._jvm.java.lang.Class.forName("org.postgresql.Driver")



