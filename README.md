# ShopSphere — Azure Databricks E-Commerce Data Engineering Pipeline

## 📌 Project Overview

**ShopSphere** is an end-to-end e-commerce data engineering pipeline built using **Azure Data Lake Storage Gen2, Azure Databricks, PySpark, Apache Spark, and Delta Lake**.

The project demonstrates how raw e-commerce data can be ingested, transformed, validated, and organized into business-ready datasets using a **Bronze → Silver → Gold medallion architecture**.

The pipeline is orchestrated using **Databricks Workflows**, while the source code is maintained in **GitHub** through Databricks Git integration.

---

## 🏗️ Architecture

```text
                    ┌─────────────────────┐
                    │   Raw CSV Files     │
                    │                     │
                    │ Customers           │
                    │ Products            │
                    │ Orders              │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   ADLS Gen2         │
                    │   Raw Layer         │
                    └──────────┬──────────┘
                               │
                               ▼
              ┌─────────────────────────────────┐
              │       Azure Databricks          │
              │          PySpark / Spark        │
              └────────────────┬────────────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Bronze Layer      │
                    │   Raw/Ingested Data │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Silver Layer      │
                    │ Cleaned & Validated │
                    │       Data          │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Gold Layer       │
                    │ Business-Ready Data │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌───
```
