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
                    ┌─────────────────────┐
                    │  Data Quality       │
                    │     Checks          │
                    └─────────────────────┘


       GitHub
          │
          │ Source Control
          ▼
   Databricks Notebooks
```

---

## ☁️ Azure Architecture

The project uses the following Azure components:

| Component                                 | Purpose                                         |
| ----------------------------------------- | ----------------------------------------------- |
| **Azure Data Lake Storage Gen2**          | Cloud storage for raw and processed data        |
| **Azure Databricks**                      | Data processing and transformation platform     |
| **Apache Spark**                          | Distributed data processing engine              |
| **PySpark**                               | Python-based Spark transformations              |
| **Delta Lake**                            | Reliable storage format for analytical datasets |
| **Databricks Workflows**                  | Pipeline orchestration                          |
| **Unity Catalog / Databricks governance** | Data governance and access management           |
| **Microsoft Entra ID / Managed Identity** | Identity-based access to Azure resources        |
| **GitHub**                                | Source control and version management           |

---

## 📂 Data Lake Structure

The ADLS Gen2 storage account is organized into separate layers:

```text
shopsphere/
│
├── raw/
│   ├── customers/
│   │   └── customers.csv
│   │
│   ├── products/
│   │   └── products.csv
│   │
│   └── orders/
│       └── orders.csv
│
├── bronze/
│
├── silver/
│
└── gold/
```

The raw layer contains the source CSV files, while the Bronze, Silver, and Gold layers contain progressively refined datasets.

---

# 🥉 Bronze Layer

The Bronze layer is responsible for **initial ingestion of raw source data**.

### Responsibilities

* Read raw CSV files from ADLS Gen2
* Ingest customer, product, and order data
* Preserve the source data structure as much as practical
* Store the ingested data in the Bronze layer
* Provide a reliable starting point for downstream transformations

### Notebook

```text
notebooks/01_bronze_ingestion
```

---

# 🥈 Silver Layer

The Silver layer contains **cleaned and transformed data** suitable for analytical processing.

### Typical transformation activities

* Data type standardization
* Data cleansing
* Handling invalid or inconsistent records
* Null-value handling
* Duplicate handling
* Column standardization
* Data validation
* Preparation of datasets for business-level transformations

### Notebook

```text
notebooks/02_silver_transformations
```

The goal of the Silver layer is to provide a trusted and consistent representation of the underlying e-commerce data.

---

# 🥇 Gold Layer

The Gold layer contains **business-oriented datasets** designed for analytics and reporting.

The project transforms the Silver datasets into analytical views of areas such as:

* Sales performance
* Product performance
* Category-level performance
* Customer-level sales analysis
* Daily sales trends

### Notebook

```text
notebooks/03_gold_transformations
```

The Gold layer is designed to provide datasets that can be consumed by downstream analytics and BI tools.

---

# 🔎 Data Quality

Data quality checks are implemented as a dedicated pipeline stage.

### Checks include

* Record validation
* Null checks
* Duplicate/consistency checks
* Dataset-level validation
* Pipeline-level pass/fail status

### Notebook

```text
notebooks/04_data_quality
```

The data quality stage runs after the Gold transformations to validate the processed datasets.

---

# 🔄 Pipeline Orchestration

The complete pipeline is orchestrated using **Azure Databricks Workflows**.

### Workflow

```text
bronze_ingestion
        │
        ▼
silver_transformations
        │
        ▼
gold_transformations
        │
        ▼
data_quality
```

The Databricks Workflow executes the tasks sequentially so that each layer is created before the next stage begins.

### Workflow Name

```text
ShopSphere_ETL_Pipeline
```

The workflow has been successfully executed end-to-end.

---

# 🔐 Security and Access

The project uses Azure identity-based access rather than embedding storage credentials directly into notebooks.

An **Azure Databricks Access Connector** with a managed identity is used to provide access to ADLS Gen2 through Azure RBAC.

This avoids storing storage account keys or other sensitive credentials directly in the source code.

The repository therefore does **not** contain:

* Storage account keys
* Passwords
* Client secrets
* Access tokens
* Raw production credentials

---

# 🔀 GitHub Integration

The Databricks notebooks are maintained in GitHub using **Databricks Git integration**.

Repository structure:

```text
shopsphere-azure-databricks/
│
├── README.md
│
├── notebooks/
│   ├── 01_bronze_ingestion
│   ├── 02_silver_transformations
│   ├── 03_gold_transformations
│   └── 04_data_quality
│
└── .gitignore
```

Git provides:

* Version control
* Change tracking
* Collaboration
* Source-code history
* Portfolio visibility

The Databricks GitHub App provides the required repository access for pushing changes from Databricks.

---

# 🛠️ Technologies

```text
Azure
├── Azure Data Lake Storage Gen2
├── Azure Databricks
├── Microsoft Entra ID
└── Azure RBAC

Databricks
├── Apache Spark
├── PySpark
├── Delta Lake
├── Databricks Workflows
└── Unity Catalog

Development
├── Python
├── SQL
└── Git / GitHub
```

---

# 📊 Medallion Architecture

ShopSphere follows the Medallion Architecture pattern:

| Layer      | Purpose                            |
| ---------- | ---------------------------------- |
| **Bronze** | Raw ingested data                  |
| **Silver** | Cleaned and validated data         |
| **Gold**   | Business-ready analytical datasets |

This separation makes the pipeline easier to maintain, troubleshoot, and extend.

---

# 🚀 Pipeline Execution

The pipeline can be executed through the Databricks Workflow:

```text
ShopSphere_ETL_Pipeline
        │
        ├── 01_bronze_ingestion
        │
        ├── 02_silver_transformations
        │
        ├── 03_gold_transformations
        │
        └── 04_data_quality
```

Each stage produces output for the next stage and the final data quality task validates the pipeline results.

---

# 🎯 Key Data Engineering Concepts Demonstrated

This project demonstrates practical experience with:

* Cloud data lake architecture
* Azure Data Lake Storage Gen2
* Azure Databricks
* Apache Spark
* PySpark
* DataFrame transformations
* Delta Lake
* Medallion architecture
* ETL/ELT pipeline development
* Data cleansing
* Data quality validation
* Workflow orchestration
* Azure RBAC
* Managed identities
* Git and GitHub integration
* Source-controlled Databricks development

---

# 💡 AWS → Azure Mapping

The project is also designed to demonstrate the transition from AWS-based data engineering concepts to Azure.

| AWS                  | Azure                               |
| -------------------- | ----------------------------------- |
| Amazon S3            | Azure Data Lake Storage Gen2        |
| AWS IAM              | Azure RBAC / Microsoft Entra ID     |
| Databricks on AWS    | Azure Databricks                    |
| S3 data lake         | ADLS Gen2 data lake                 |
| IAM-based access     | Managed Identity / Access Connector |
| Databricks Workflows | Databricks Workflows                |
| Delta Lake           | Delta Lake                          |

The core Spark and Delta Lake concepts remain largely consistent between AWS and Azure, while storage, identity, and cloud-resource management differ.

---

# 📈 Future Enhancements

Potential future improvements include:

* Parameterizing the pipeline for different environments
* Incremental data ingestion
* Automated schema evolution
* Advanced data quality reporting
* Pipeline monitoring and alerting
* Slowly Changing Dimensions
* Error handling and retry mechanisms
* CI/CD integration
* Power BI reporting
* Automated testing
* Infrastructure as Code using Terraform
* Production-style development and deployment environments

---

# 👩‍💻 Project Author

**Ayesha**

This project was created as a hands-on demonstration of modern cloud data engineering using **Azure, Databricks, PySpark, Delta Lake, and GitHub**.
