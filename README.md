Fintech Data Migration & Analytics Pipeline
===========================================

An industrial-grade end-to-end data engineering project that modernizes legacy financial data infrastructure. This pipeline migrates historical records from an **Azure SQL Database** (MSSQL) to a **Delta Lake** architecture on **Azure Data Lake Storage (ADLS) Gen2** , utilizing a Medallion Architecture for scalable analytics.

🏗️ Architecture Overview
-------------------------

The project implements a multi-stage ETL/ELT process orchestrated via** Azure Synapse Analytics **:

1.  **Source:** Azure SQL Database (Transactional Fintech Data).

2.  **Bronze Layer (Ods):** Dynamic ingestion of raw data into ADLS Gen2 (Parquet format).

3.  **Silver Layer (Cleansed):** Data cleansing and schema enforcement using PySpark.

4.  **Gold Layer (Curated):** Business-level aggregations and KPI calculations for reporting.

5.  **Monitoring:** Automated alerting via Azure Logic Apps and Web Activities.

* * * * *

🛠️ Tech Stack
--------------

-   **Orchestration:** Azure Synapse Analytics (Pipelines)

-   **Compute:** Serverless Spark Pools (PySpark)

-   **Storage:** ADLS Gen2, Delta Tables

-   **Database:** Azure SQL Database (MSSQL)

-   **Automation:** Azure Logic Apps (Email Notifications)

-   **Version Control:** Git

* * * * *

📂 Project Structure
--------------------

Bash

```
fintech-azure-data-pipeline/
├── Fintech_Data_Migration_Project/
│   ├── spark_notebooks/        # PySpark ETL scripts (Bronze to Silver, Silver to Gold)
│   ├── sql_database_tables/    # SQL DDL for source system setup
│   └── sql_queries/            # Metadata and lookup queries
├── synapse-artifacts/          # Exported Synapse JSON templates (Pipelines, Datasets)
├── Images/                     # Screenshots of pipeline execution & resources
└── sql_database_tables/        # Table-specific SQL scripts (Accounts, Loans, etc.)

```

* * * * *

🚀 Key Features
---------------

### 1\. Dynamic Ingestion

Instead of hardcoding table names, the pipeline uses a **Lookup Activity** coupled with a **ForEach** loop. It queries the `INFORMATION_SCHEMA.TABLES`to dynamically ingest all Fintech tables (Accounts, Customers, Loans, Payments, Transactions) in a single execution.

### 2\. Medallion Processing logic

-   **Bronze to Silver:** PySpark scripts handle null values, standardize date formats, and convert data into **Delta format** for ACID compliance.

-   **Silver to Gold:** Performs complex joins and aggregations (eg, total loan amounts per customer, payment success rates) to create analytics-ready views.

### 3\. Automated Alerting System

A custom** Azure Logic App **is integrated via Synapse **Web Activity **.

-   On **Success** : Triggers an HTTP POST request to the Logic App to send a "SUCCESS" email with run details.

-   On **Failure** : Captures error logs and notifies the operations team immediately.

* * * * *

📊 Pipeline Execution Details
-----------------------------

The pipeline is designed with built-in error handling and sequential dependencies to ensure data integrity.

| **Activity Name** | **Type** | **Purpose** |
| --- | --- | --- |
| `lkp_GetTableList` | Lookup | Fetches the list of tables from the SQL source. |
| `fe_IterateOnEachTable` | ForEach | Iterates through tables for parallel ingestion. |
| `nb_BronzeToSilver` | Spark Notebook | Raw to Cleansed transformation. |
| `nb_SilverToGold` | Spark Notebook | Business logic and aggregations. |
| `web_SuccessEmail` | Web Activity | Triggers Logic App on successful completion. |

* * * * *

📸 Proof of Concept
-------------------

### Pipeline Graph

### Data Storage (ADLS Gen2)

### Execution Result

The pipeline has been validated with successful runs, migrating all fintech records and delivering automated email confirmations upon completion.

* * * * *

🛠️ Setup and Installation
--------------------------

1.  **Database:** Execute the scripts in `/sql_database_tables`on your Azure SQL instance.

2.  **Storage:** Create an ADLS Gen2 account with containers: `bronze`, `silver`, `gold`.

3.  **Synapse:** Import the artifacts from `/synapse-artifacts`and link your Spark Pool.

4.  **Logic App:** Create a Logic App with an HTTP Request trigger and an Office 365 Outlook "Send an email" action.

* * * * *

*Developed by Manish Rai - 2026*
