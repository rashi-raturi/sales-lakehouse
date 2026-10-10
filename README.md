# Enterprise Data Lakehouse (Medallion Architecture)

An end-to-end modern **Data Lakehouse** pipeline built on **Databricks** and **Delta Lake**, ingesting heterogeneous source data from enterprise CRM and ERP systems, applying scalable transformations, and serving business-ready dimensional models (Star Schema).



## Architecture Overview

The pipeline follows the industry-standard **Medallion Architecture** governed by Databricks **Unity Catalog**:

```
[ CRM & ERP Source CSVs ]
            │
            ▼
┌────────────────────────────────────────────────────────┐
│  BRONZE LAYER (Raw Ingestion)                          │
│  • Batch ingested from Unity Catalog Volumes           │
│  • Stored as raw Delta tables with full auditability   │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│  SILVER LAYER (Cleaned & Standardized)                 │
│  • PySpark transformations: schema enforcement, trim   │
│  • Anomaly removal, date parsing (yyyyMMdd -> Date)    │
│  • Data deduplication & business rule normalization    │
│  • Orchestrated via Databricks Jobs (dbutils)          │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│  GOLD LAYER (Business-Ready Star Schema)               │
│  • Dimension Tables: dim_customers, dim_products       │
│  • Fact Table: fact_sales                              │
│  • Surrogate key generation & analytical optimization  │
└────────────────────────────────────────────────────────┘
```


## Project Structure

```text
├── datasets/
│   ├── engineering/             # Raw CRM & ERP operational data
│   │   ├── source_crm/          # cust_info, prd_info, sales_details
│   │   └── source_erp/          # CUST_AZ12, LOC_A101, PX_CAT_G1V2
│   └── analysts/                # Curated analytical exports
├── script/
│   ├── init_lakehouse.ipynb     # Unity Catalog & schema provisioning
│   ├── bronze/
│   │   └── bronze_layer.ipynb   # Multi-source raw ingestion pipeline
│   ├── silver/
│   │   ├── crm/                 # CRM silver layer notebooks
│   │   ├── erp/                 # ERP silver layer notebooks
│   │   └── silver_orchestration.ipynb
│   └── gold/
│       ├── gold_dim_customers.ipynb
│       ├── gold_dim_products.ipynb
│       ├── gold_fact_sales.ipynb
│       └── gold_orchestration.ipynb
```


## Tech Stack

* **Platform:** Databricks, Unity Catalog
* **Storage Engine:** Delta Lake
* **Processing:** PySpark, Spark SQL
* **Data Modeling:** Star Schema (Kimball methodology)

