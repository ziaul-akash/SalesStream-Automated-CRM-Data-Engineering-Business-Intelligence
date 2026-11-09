# 🚀 CRM Sales Data Engineering Pipeline | Azure UK South

**An end-to-end Azure Data Engineering solution for automated CRM sales data ingestion, transformation, secure storage, optimised analytics, and interactive business intelligence.**

---

## 📌 Project Overview

This project demonstrates the development of a modern, end-to-end Azure Data Engineering pipeline using CRM sales data.

The solution integrates multiple Azure services to build a scalable and secure data platform that ingests data from external and on-premises sources, transforms raw data into analytics-ready datasets, and delivers business insights through Power BI dashboards.

The pipeline follows a structured Raw-to-Curated Data Lake architecture, leveraging Azure Data Factory for orchestration, Azure Databricks for PySpark transformations, Azure Data Lake Storage Gen2 for storage, and Azure Synapse Analytics for serverless SQL analytics.

The project also implements automated monitoring, email notifications, Git integration, and centralised secrets management to improve reliability, maintainability, and security.

## 🎯 Business Problem

Organisations often maintain CRM sales data across multiple systems and file formats. Extracting meaningful business insights from this data can become challenging due to:

- Manual and time-consuming data ingestion processes.
- Inconsistent data formats, schemas, and data quality.
- Large volumes of raw data increasing storage requirements.
- Complex transformation logic required before analytics.
- Slow reporting caused by querying unoptimised datasets.
- Security risks associated with hardcoded credentials.
- Lack of automated pipeline monitoring and failure notifications.
- Difficulty maintaining and collaborating on data pipelines.

These challenges can delay reporting, increase operational overhead, and make it difficult for business stakeholders to access reliable and timely sales insights.

## 💡 Solution

I designed and implemented an Azure-based data engineering pipeline to automate the complete data lifecycle.

The solution:

- Automates ingestion of CRM sales data using Azure Data Factory.
- Uses Self-Hosted Integration Runtime for secure on-premises connectivity.
- Implements a Raw and Curated Data Lake architecture using ADLS Gen2.
- Uses Azure Databricks and PySpark for data cleaning, standardisation, and transformation.
- Applies efficient storage formats such as Parquet/Delta for analytics-ready data.
- Uses Azure Synapse Analytics Serverless SQL to query curated data directly from the Data Lake.
- Integrates Power BI for interactive sales reporting and business insights.
- Uses Azure Key Vault and Databricks Secret Scope for secure credential management.
- Implements Logic Apps email alerts for pipeline success and failure.
- Uses Azure Monitor for operational monitoring and troubleshooting.
- Integrates Azure Data Factory with Git for version control and collaborative development.

The overall objective is to reduce manual effort, improve data processing efficiency, optimise storage, and make business reporting faster and more reliable.

## 🏗️ Architecture

![Azure CRM Sales Data Engineering Architecture](architecture_diagram.png)

### Architecture Components

| Azure Service | Purpose |
|---|---|
| External Data Sources | Source CRM sales data and CSV files |
| Azure Data Factory | Data ingestion and pipeline orchestration |
| Self-Hosted Integration Runtime | Secure connectivity to on-premises data |
| Git Repository | Version control and collaboration |
| Azure Data Lake Storage Gen2 | Raw and curated data storage |
| Azure Databricks | Data transformation using PySpark |
| Azure Key Vault | Secure storage of credentials and secrets |
| Databricks Secret Scope | Secure access to stored secrets |
| Azure Synapse Analytics | Serverless SQL analytics on curated data |
| Power BI | Interactive dashboards and visualisation |
| Azure Logic Apps | Automated email notifications |
| Azure Monitor | Pipeline monitoring and operational tracking |

## 🔄 End-to-End Data Pipeline

### 1. Data Ingestion — Azure Data Factory

- Ingest CRM sales data from external and on-premises sources.
- Use Self-Hosted Integration Runtime for secure connectivity.
- Automate data movement into Azure Data Lake Storage Gen2.
- Configure pipeline dependencies and execution activities.

### 2. Data Lake Storage — ADLS Gen2

Implement a structured Data Lake architecture:

- **Raw Zone:** Stores source data in its original form for traceability and reprocessing.
- **Curated Zone:** Stores cleaned, transformed, and analytics-ready data.

This separation improves data organisation, maintainability, and downstream analytics.

### 3. Data Transformation — Azure Databricks

Use PySpark notebooks to perform:

- Data cleansing and standardisation.
- Schema validation and data type conversion.
- Duplicate record handling.
- Null value management.
- Data enrichment and business transformations.
- Efficient writing of curated datasets.

### 4. Data Security — Azure Key Vault

- Store sensitive credentials securely.
- Integrate Azure Key Vault with Databricks Secret Scope.
- Retrieve secrets securely during pipeline execution.
- Avoid hardcoding storage keys and connection strings.
- Apply appropriate access controls.

### 5. SQL Analytics — Azure Synapse Analytics

Use Synapse Serverless SQL Pool to query curated datasets directly from ADLS Gen2.

- Query Parquet/Delta files without loading them into a dedicated warehouse.
- Create SQL views for reporting.
- Enable efficient analytical access to transformed datasets.
- Reduce unnecessary data movement.

### 6. Business Intelligence — Power BI

Connect Power BI to Synapse Analytics to build interactive dashboards.

Potential business KPIs include:

- Total Sales Revenue
- Total Orders
- Average Order Value
- Monthly Sales Trends
- Product Performance
- Customer Sales Analysis
- Regional Sales Performance
- Sales Growth

### 7. Monitoring and Notifications

- Configure Azure Monitor for pipeline execution tracking.
- Integrate Azure Logic Apps for email notifications.
- Send automated alerts on pipeline success or failure.
- Improve operational visibility and troubleshooting.

---

## ⚡ Problem Solving, Performance Optimisation & Efficiency

One of the key objectives of this project is to simplify complex data engineering processes while improving storage efficiency and analytical performance.

### Challenges Addressed

| Challenge | Solution | Expected Improvement |
|---|---|---|
| Manual data ingestion | Automated ADF pipelines | Reduced manual intervention |
| Large raw CSV datasets | Parquet/Delta-based curated storage | More efficient storage and analytics |
| Complex data cleaning | PySpark transformation notebooks | Reusable and scalable processing |
| Slow analytical queries | Synapse Serverless SQL on curated files | Reduced unnecessary data processing |
| Repeated data processing | Structured Raw and Curated zones | Improved data organisation and reprocessing |
| Hardcoded credentials | Azure Key Vault integration | Improved security and maintainability |
| Pipeline failures without visibility | Logic Apps email alerts | Faster issue identification |
| Difficult pipeline maintenance | Git-integrated ADF development | Improved collaboration and version control |

### Storage Optimisation

Raw CSV files are useful for ingestion, but they can be inefficient for repeated analytical workloads.

By transforming data into columnar formats such as Parquet or Delta, the pipeline can benefit from:

- Compression to reduce storage footprint.
- Columnar storage for analytical workloads.
- Efficient column pruning.
- Reduced data scanning when queries select only required columns.
- Improved compatibility with Spark and serverless SQL analytics.

**Note:** Actual storage reduction depends on the dataset, compression settings, schema, and file sizes. Quantitative improvements should be measured against the original CSV data.

### Query Performance Optimisation

The curated layer is designed to support efficient downstream analytics.

Potential optimisation strategies include:

- Selecting only the required columns in SQL queries.
- Applying partitioning based on suitable business or date columns.
- Avoiding unnecessary full dataset scans.
- Reducing small-file overhead during Spark writes.
- Using appropriate Parquet compression.
- Creating reusable Synapse SQL views for reporting.

These techniques can reduce data scanned and improve query response times when implemented appropriately.

### Processing Efficiency

Azure Databricks provides distributed processing capabilities using Apache Spark.

PySpark allows the pipeline to process larger datasets efficiently compared with purely manual or single-machine processing approaches.

The transformation layer also centralises business logic, making data preparation more consistent, reusable, and maintainable.

---

## 🔐 Security & Governance

Security is implemented across the data pipeline using Azure-native services.

- Azure Key Vault for centralised secret management.
- Databricks Secret Scope for secure credential retrieval.
- Azure RBAC for access control.
- ADLS Gen2 ACLs for data access permissions.
- Git-based version control without committing secrets.
- Secure connectivity through Self-Hosted Integration Runtime.

The design follows the principle of least privilege wherever applicable.

## 📊 Analytics & Business Insights

The curated CRM sales data can be used to support business decisions through Power BI.

Example dashboard analysis:

- Identify top-performing products.
- Analyse sales trends over time.
- Compare regional performance.
- Identify high-value customers.
- Track revenue growth.
- Monitor order volumes and average order values.

The final objective is to transform raw operational data into meaningful and accessible business insights.

## 🛠️ Technology Stack

| Category | Technology |
|---|---|
| Cloud Platform | Microsoft Azure |
| Cloud Region | UK South |
| Data Ingestion | Azure Data Factory |
| Hybrid Connectivity | Self-Hosted Integration Runtime |
| Storage | Azure Data Lake Storage Gen2 |
| Data Processing | Azure Databricks |
| Programming | Python, PySpark |
| Data Formats | CSV, Parquet, Delta |
| SQL Analytics | Azure Synapse Analytics Serverless SQL |
| Security | Azure Key Vault, Databricks Secret Scope |
| Monitoring | Azure Monitor |
| Notifications | Azure Logic Apps |
| Visualisation | Power BI |
| Version Control | Git |

## 📁 Repository Structure

```text
CRM_SALES_UKSOUTH_AZURE_PROJECT/
│
├── README.md
│
├── architecture_diagram.png
│
├── adf/
│   ├── pipelines/
│   ├── datasets/
│   └── linked_services/
│
├── databricks/
│   └── notebooks/
│       ├── 01_data_ingestion.py
│       ├── 02_data_cleaning.py
│       └── 03_data_transformation.py
│
├── synapse/
│   └── sql/
│       └── curated_views.sql
│
├── powerbi/
│   └── dashboards/
│
└── docs/
    └── data_dictionary.md
```

*Repository structure is illustrative and should be adjusted to match the actual project files.*

## 🚀 Project Implementation Steps

1. Create Azure resources in the UK South region.
2. Configure Azure Data Factory and Git integration.
3. Set up Self-Hosted Integration Runtime.
4. Create ADLS Gen2 storage and Raw/Curated zones.
5. Configure ADF ingestion pipelines.
6. Integrate Azure Key Vault with Databricks.
7. Develop PySpark transformation notebooks.
8. Transform and publish curated datasets.
9. Configure Synapse Serverless SQL access.
10. Build Power BI dashboards.
11. Configure Azure Monitor and Logic Apps email alerts.
12. Test pipeline execution, data quality, and failure handling.

## 📈 Future Enhancements

- Implement incremental data ingestion using watermarks.
- Introduce a full Bronze, Silver, and Gold Medallion Architecture.
- Add automated data quality validation.
- Implement CI/CD using Azure DevOps.
- Introduce dimensional modelling for advanced reporting.
- Implement automated performance benchmarking.
- Add pipeline execution metrics and data reconciliation.
- Optimise Spark partitioning and file sizes based on workload.

## 👨‍💻 Author

**Ziaul Haq Akash**

Azure Data Engineering Portfolio Project

Skills Demonstrated:

Azure Data Factory | Azure Databricks | PySpark | ADLS Gen2 | Azure Synapse Analytics | Azure Key Vault | Power BI | SQL | Data Engineering | ETL | Cloud Data Architecture

---

<div align="center">

⭐ If you find this project useful, feel free to star the repository.

</div>
