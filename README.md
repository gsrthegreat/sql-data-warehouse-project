---

# Data Warehouse Project: SQL Server Medallion Architecture

## 📌 Project Overview

This project implements a **three-tier data warehouse architecture** (Bronze, Silver, Gold) within Microsoft SQL Server. The pipeline automates the ingestion of raw **CSV files**, transforms them into a cleaned state, and finally models them into a **Star Schema** to provide high-performance business intelligence outputs.

### 🏗 Architecture Layers

The project follows the **Medallion Architecture** to ensure data quality and lineage:

1. **Bronze (Raw) Layer**: Ingests raw CSV files "as-is" into SQL Server staging tables using `BULK INSERT`. This preserves the original data for auditability.
2. **Silver (Cleaned) Layer**: Performs data cleansing, handles NULL values, standardizes formats, and removes duplicates.
3. **Gold (Curated) Layer**: Houses the final **Star Schema** (Fact and Dimension tables). This layer is optimized for reporting tools like Power BI or Tableau.

---

## 🚀 Getting Started

### Prerequisites

* **Microsoft SQL Server** (2016 or later recommended)
* **SQL Server Management Studio (SSMS)** or Azure Data Studio
* Raw data files in `.csv` format located in a local folder (e.g., `C:\SQL_Ingest\`)

### Folder Structure

```text
├── data/               # Raw CSV input files
├── scripts/
│   ├── 01_init_db.sql  # Database and Schema creation
│   ├── 02_bronze.sql   # DDL and BULK INSERT scripts
│   ├── 03_silver.sql   # Transformation logic (Cleaning)
│   └── 04_gold.sql     # Dimensional Modeling (Facts/Dims)
└── README.md

```

---

## 🛠 ETL Workflow

### 1. Bronze Layer (Ingestion)

Data is pulled from the source folder. Use the following pattern for ingestion:

```sql
BULK INSERT Bronze.Raw_Sales
FROM 'C:\SQL_Ingest\sales_data.csv'
WITH (
    FIELDTERMINATOR = ',',
    ROWTERMINATOR = '\n',
    FIRSTROW = 2
);

```

### 2. Silver Layer (Transformation)

In this tier, we apply business rules and data typing:

* **Deduplication**: Removing redundant records.
* **Data Casting**: Converting strings to `INT`, `DECIMAL`, or `DATETIME`.
* **Handling Nulls**: Using `ISNULL()` or `COALESCE()` to provide default values.

### 3. Gold Layer (Business Output)

The final output is a **Star Schema** designed for maximum query speed:

* **Dimension Tables**: `dim_customers`, `dim_products`, `dim_date`.
* **Fact Tables**: `fact_sales`, `fact_inventory`.

---

## 📊 Business Value

* **Single Source of Truth**: Unified view of disparate CSV inputs.
* **Performance**: Aggregated views in the Gold layer reduce report loading times.
* **Scalability**: New CSV sources can be added by simply creating new Bronze staging tables.

---

## 🔮 Future Enhancements

* Automate the folder monitoring using **SQL Server Agent** or **SSIS**.
* Implement **SCD Type 2** (Slowly Changing Dimensions) to track historical changes.
* Connect to **Power BI** for real-time dashboarding.

---
