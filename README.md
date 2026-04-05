# 🚀 Sales Data Engineering Pipeline (Incremental | Bronze → Silver → Gold)

## 📌 Project Overview

This project implements an **end-to-end incremental data engineering pipeline** using **PySpark** and **Delta Lake**, following the **Medallion Architecture (Bronze → Silver → Gold)**.

The pipeline simulates real-world production scenarios by:

* ingesting **dirty raw data**
* processing only **new incremental data**
* maintaining **cumulative business metrics**
* ensuring **data reliability using a control table**

---

## 🏗️ Architecture

### 🥉 Bronze Layer (Raw Data Ingestion)

* Reads raw CSV files from source folder
* Adds metadata column: `ingestion_time`
* Stores data as **append-only Delta table**

**Purpose:**

* Preserve full historical raw data
* Act as **source of truth**
* Enable replay and auditing

---

### 🥈 Silver Layer (Incremental Cleaning & Transformation)

The Silver layer processes **only new data** using:

```python
ingestion_time > last_run_time
```

**Transformations performed:**

* ✅ **Safe Data Type Casting**

  * `sales` converted using `try_cast` to avoid failures

* ✅ **Invalid Data Handling**

  * Invalid values (e.g., `"abc"`) converted to NULL → handled safely

* ✅ **Null Handling**

  * `sales` → 0
  * `city` → `"unknown"`

* ✅ **Standardization**

  * Converted `city` to lowercase

* ✅ **Deduplication**

  * Removed duplicate records within batch

**Purpose:**

* Process only incremental data
* Improve data quality
* Avoid reprocessing historical data

---

### 🥇 Gold Layer (Cumulative Business Metrics)

The Gold layer generates analytics-ready datasets using **MERGE (UPSERT)**.

#### 📊 Total Sales per City

```sql
MERGE INTO gold_table
USING incremental_data
ON city
WHEN MATCHED THEN UPDATE
WHEN NOT MATCHED THEN INSERT
```

**Purpose:**

* Maintain **cumulative aggregates**
* Avoid full recomputation
* Enable efficient analytics

---

## 🧠 Incremental Processing Design

### 🔹 Approach Used

* **Batch-level incremental processing**
* No source timestamp → uses `ingestion_time`

---

### 🔹 Control Table (Checkpointing)

Tracks pipeline state:

| pipeline_name  | last_run_time | status  |
| -------------- | ------------- | ------- |
| sales_pipeline | timestamp     | SUCCESS |

**Role:**

* Ensures only new data is processed
* Prevents duplicate processing
* Enables restart-safe pipeline

---

## 🔁 Pipeline Flow

```text
Raw CSV Files
      ↓
Bronze (Append Raw Data + ingestion_time)
      ↓
Silver (Process only new data using control table)
      ↓
Gold (MERGE incremental aggregates)
      ↓
Update Control Table (after success)
```

---

## 🧪 Pipeline Validation

The pipeline was validated through multiple test scenarios:

### ✅ Test 1: Initial Load

* All data processed
* Gold matches Silver aggregation

### ✅ Test 2: Incremental Load

* Only new files processed
* No reprocessing of old data
* Gold updated cumulatively

### ✅ Test 3: No New Data

* Silver processed 0 rows
* Gold remained unchanged

### ✅ Data Quality Validation

* Invalid values handled
* Nulls replaced correctly
* Duplicate rows removed

---

## 🛠️ Technologies Used

* PySpark
* Delta Lake
* Databricks / Apache Spark
* CSV (Raw Data Source)

---

## 📁 Repository Structure

```text
project/
│
├── data/
│   └── generated CSV files (multiple batches)
│
├── notebooks/
│   ├── bronze_layer.ipynb
│   ├── silver_layer.ipynb
│   └── gold_layer.ipynb
│
├── validation/
│   └── pipeline_validation.md
│
└── README.md
```

---

## 🚀 Future Enhancements

* Use **Auto Loader** for file-based incremental ingestion
* Add **orchestration (ADF / Airflow)**
* Implement **failure handling (status = FAILED)**
* Add **data quality checks (expectations)**
* Optimize using **partitioning**

---

## 💡 Summary

This project demonstrates:

* Incremental data processing
* Medallion architecture implementation
* Data cleaning and standardization
* Deduplication strategies
* Delta Lake MERGE (upsert logic)
* Control-table-based checkpointing

It reflects a **production-style batch data pipeline design**.

---
