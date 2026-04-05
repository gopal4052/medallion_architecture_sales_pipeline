# 🧪 Pipeline Validation Report

## 📌 Objective

Validate correctness of incremental data pipeline across Bronze, Silver, and Gold layers.

---

## 🔹 Test 1: Initial Run

### Input

* Single CSV file (~10,000 rows)

### Observations

| Layer  | Result                         |
| ------ | ------------------------------ |
| Bronze | 10,000 rows ingested           |
| Silver | 9,999 rows after deduplication |
| Gold   | Aggregated totals per city     |

### Validation

* Silver transformations applied correctly
* Gold matches Silver aggregation

---

## 🔹 Test 2: Incremental Run

### Input

* New CSV file added

### Observations

| Layer  | Result                              |
| ------ | ----------------------------------- |
| Bronze | 20,000 total rows                   |
| Silver | 19,998 total rows                   |
| Silver | Two distinct ingestion_time batches |
| Gold   | Updated cumulative totals           |

### Key Validation

```sql
SELECT ingestion_time, COUNT(*)
FROM silver_table
GROUP BY ingestion_time;
```

Result:

* Batch 1 → 9999 rows
* Batch 2 → 9999 rows

### Conclusion

* Only new data processed
* No reprocessing of historical data

---

## 🔹 Test 3: No New Data

### Input

* No file in source folder

### Observations

| Layer  | Result           |
| ------ | ---------------- |
| Silver | 0 rows processed |
| Gold   | No change        |

### Validation

```sql
SELECT COUNT(*) FROM silver_table;
```

Result:

* Remains unchanged

### Conclusion

* Pipeline correctly skips processing
* No unnecessary recomputation

---

## 🔹 Data Quality Validation

| Check                | Result                     |
| -------------------- | -------------------------- |
| Invalid sales values | Converted to 0             |
| Null city values     | Replaced with "unknown"    |
| Case inconsistency   | Standardized using lower() |
| Duplicate rows       | Removed successfully       |

---

## 🎯 Final Conclusion

The pipeline successfully:

* Processes data incrementally
* Avoids reprocessing
* Maintains cumulative aggregates
* Handles dirty data correctly
* Works correctly across multiple runs

---
