# 🚀 Sales Data Engineering Pipeline (Bronze → Silver → Gold)

## 📌 Project Overview

This project demonstrates an end-to-end **data engineering pipeline** using **PySpark** and **Delta Lake**, following the **Medallion Architecture (Bronze, Silver, Gold layers)**.

The pipeline simulates real-world data processing by ingesting raw data, cleaning and transforming it, and generating business-level insights.

---

## 🏗️ Architecture

### 🥉 Bronze Layer (Raw Data Ingestion)

* Data is ingested from raw CSV file created using a script such that data is dirty replicating real world case.
* Stored as **Delta Tables**
* Schema is inferred automatically
* No transformations applied

**Purpose:**

* Preserve raw data as-is
* Provide a reliable source of truth
* Enable **ACID transactions** using Delta Lake

---

### 🥈 Silver Layer (Data Cleaning & Transformation)

The Silver layer improves data quality and prepares it for analysis.

**Transformations performed:**

* ✅ **Safe Data Type Casting**

  * Converted `sales` from string → double using `try_cast` to prevent pipeline failure

* ✅ **Handling Invalid Data**

  * Invalid values (e.g., "abc") converted to NULL and handled safely

* ✅ **Null Handling**

  * Replaced NULL `sales` with `0` (based on business logic)
  * Replaced NULL `city` with `"unknown"`

* ✅ **Data Standardization**

  * Converted all `city` values to lowercase to avoid duplicate grouping issues

* ✅ **Duplicate Handling**

  * Validated duplicate records using count comparison
  * Removed exact duplicate rows

---

### 🥇 Gold Layer (Business-Level Aggregation)

The Gold layer provides **analytics-ready data** for reporting and dashboards.

**Tables created:**

#### 📊 1. Total Sales per City

* Aggregated total sales for each city

#### 📉 3. KPI Metrics

* Total sales
* Average sales
* Total number of records

---

## 🛠️ Technologies Used

* **PySpark**
* **Delta Lake**
* **Databricks / Apache Spark**
* **CSV (Raw Data Source)**

---

## 📊 Pipeline Flow

```text
Raw CSV Data 
     ↓
Bronze Layer (Delta - Raw Data)
     ↓
Silver Layer (Cleaned & Standardized Data)
     ↓
Gold Layer (Aggregated Business Insights)
```

---

## 🧠 Key Learnings

* Implementing **Medallion Architecture (Bronze → Silver → Gold)**
* Handling **dirty and inconsistent data**
* Using **try_cast for safe transformations**
* Understanding **NULL vs 0 in business logic**
* Performing **data standardization and deduplication**
* Building **aggregation pipelines for analytics**

---

## 📁 Repository Structure

```text
project/
│
├── data/
│   └── sales_data_large.csv Generated using sales_data_generation_script.ipynb such that data created is Dirty.
│
├── notebooks/
│   ├── bronze_layer.ipynb
│   ├── silver_layer.ipynb
│   └── gold_layer.ipynb
│
└── README.md
```

---

## 🚀 Future Enhancements

* Add **data partitioning for performance optimization**
* Implement **incremental data loading**
* Introduce **data quality validation checks**
* Integrate with **Azure Data Engineering tools (ADF, Data Lake, Synapse)**

---

## 💡 Summary

This project replicates a real-world data pipeline by handling:

* Invalid data
* Missing values
* Inconsistent formats
* Duplicate records

and transforming them into **clean, structured, and business-ready datasets**.
