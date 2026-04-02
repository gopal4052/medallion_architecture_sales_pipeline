
# 🚀 Data Engineering Pipeline (Bronze → Silver)

## 📌 Project Overview

This project demonstrates a real-world **data engineering pipeline** using PySpark and Delta Lake, implementing a **Medallion Architecture (Bronze → Silver layers)**.

The goal is to simulate how raw data is ingested, cleaned, and prepared for downstream analytics.

---

## 🏗️ Architecture

### 🥉 Bronze Layer (Raw Ingestion)

* Data is ingested from raw CSV file generated manually by creating a script
* Stored as **Delta Tables**
* Schema inferred automatically
* No transformations applied

**Key Features:**

* Preserves raw data as-is
* Enables **ACID transactions** using Delta Lake
* Acts as a source of truth for downstream layers

---

### 🥈 Silver Layer (Data Cleaning & Transformation)

Data from the Bronze layer is processed to improve quality and consistency.

**Transformations performed:**

* ✅ **Data Type Casting**

  * Converted `sales` column from string → double using safe casting (`try_cast`)

* ✅ **Handling Invalid Values**

  * Invalid entries (e.g., "abc") converted to NULL, then handled

* ✅ **Null Handling**

  * Replaced NULL `sales` with `0` (based on business logic)
  * Replaced NULL `city` with `"unknown"`

* ✅ **Data Standardization**

  * Converted `city` values to lowercase to avoid duplicate groupings

* ✅ **Duplicate Handling**

  * Identified duplicates using record counts
  * Removed exact duplicate rows safely

---

## 🛠️ Technologies Used

* **PySpark**
* **Delta Lake**
* **Databricks / Spark Environment**
* **CSV (Raw Data Source)**

---

## 📊 Pipeline Flow

```text
Raw CSV Data
     ↓
Bronze Layer (Delta Table - Raw Data)
     ↓
Silver Layer (Cleaned & Standardized Data)
```

---

## 🧠 Key Learnings

* Importance of **safe data casting (`try_cast`)** in real-world pipelines
* Difference between **NULL vs 0** and applying business logic
* Handling **dirty and inconsistent data**
* Understanding **data immutability in PySpark**
* Designing pipelines using **layered architecture**

---

## 🚀 Next Steps

* Build **Gold Layer** for aggregations and business insights
* Implement **data quality checks**
* Add **pipeline automation**

---

## 📁 Repository Structure (Current)

```text
project/
│
├── data/
│   └── raw_sales_data.csv
│
├── notebooks/
│   ├── bronze_layer.ipynb
│   └── silver_layer.ipynb
│
└── README.md
```

---

## 💡 Note

This project simulates real-world data issues such as:

* Invalid values
* Missing data
* Inconsistent formats
* Duplicate records

and demonstrates how to handle them using scalable data engineering practices.
>>>>>>> Stashed changes
