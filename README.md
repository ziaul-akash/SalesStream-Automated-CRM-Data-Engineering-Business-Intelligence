::: {align="center"}
# 🚀 CRM Sales Data Engineering Pipeline --- Azure UK South

`<a href="#project-overview">`{=html}
`<img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=22&pause=1000&color=0078D4&center=true&vCenter=true&width=760&lines=End-to-End+Azure+Data+Engineering;ADF+%7C+ADLS+Gen2+%7C+Databricks+%7C+Synapse;Secure+Pipelines+%7C+Analytics+%7C+Power+BI" alt="Animated project title" />`{=html}
`</a>`{=html}

```{=html}
<p>
```
`<img src="https://img.shields.io/badge/Cloud-Microsoft%20Azure-0078D4?logo=microsoftazure&logoColor=white" alt="Azure badge" />`{=html}
`<img src="https://img.shields.io/badge/Region-UK%20South-00A4EF" alt="UK South badge" />`{=html}
`<img src="https://img.shields.io/badge/Language-PySpark%20%7C%20SQL-F2C811" alt="PySpark and SQL badge" />`{=html}
`<img src="https://img.shields.io/badge/BI-Power%20BI-F2C811?logo=powerbi&logoColor=black" alt="Power BI badge" />`{=html}
`<img src="https://img.shields.io/badge/Status-Portfolio%20Project-2EA44F" alt="Portfolio project badge" />`{=html}
```{=html}
</p>
```
**A production-inspired, end-to-end data platform for ingesting,
securing, transforming, querying, and visualising CRM sales data on
Azure.**

[Project Overview](#project-overview) · [Architecture](#architecture) ·
[Pipeline Walkthrough](#pipeline-walkthrough) ·
[Security](#security-and-governance) · [Power
BI](#analytics-and-visualisation) · [Getting Started](#getting-started)
:::

------------------------------------------------------------------------

## Project Overview

This portfolio project demonstrates a modern cloud data engineering
workflow using CRM sales data. It moves data from on-premises or local
CSV sources into Azure Data Lake Storage Gen2, processes it with Azure
Databricks, exposes curated datasets through Azure Synapse Analytics
serverless SQL, and presents business insights in Power BI.

The design also incorporates source control, secret management,
monitoring, and automated pipeline notifications---key concerns in
maintainable data platforms.

### Goals

-   Build a reliable ingestion workflow from local/on-premises data
    sources to Azure.
-   Organise lake data into **raw** and **curated** zones.
-   Clean, standardise, and enrich data using PySpark.
-   Keep credentials out of notebooks and source code.
-   Query curated lake files with serverless SQL.
-   Deliver stakeholder-friendly sales reporting in Power BI.
-   Track execution and notify operators about pipeline outcomes.

## Architecture

::: {align="center"}
![CRM Sales Azure Data Engineering
Architecture](architecture_diagram.png)

`<sub>`{=html}Architecture overview --- save the supplied diagram in the
repository root as
`<code>`{=html}architecture_diagram.png`</code>`{=html}.`</sub>`{=html}
:::

### Architecture components

  -----------------------------------------------------------------------
  Component                           Responsibility
  ----------------------------------- -----------------------------------
  External / CRM data sources         Provide source sales data, such as
                                      CSV extracts

  Azure Data Factory (ADF)            Orchestrate ingestion and pipeline
                                      activities

  Self-Hosted Integration Runtime     Secure connectivity to on-premises
  (SHIR)                              data sources

  Git repository                      Version control and collaborative
                                      ADF development

  Azure Data Lake Storage Gen2        Store raw and curated data zones

  Azure Databricks                    Transform and enrich data using
                                      PySpark

  Azure Key Vault                     Centralise credentials, keys, and
                                      connection secrets

  Databricks Secret Scope             Allow notebooks to retrieve secrets
                                      securely

  Azure Logic Apps                    Send email notifications for
                                      pipeline success or failure

  Azure Monitor                       Monitor pipeline execution and
                                      operational health

  Azure Synapse Analytics (serverless Query curated Parquet/Delta data in
  SQL)                                the lake

  Power BI                            Build interactive sales dashboards
                                      and reports
  -----------------------------------------------------------------------

## Pipeline Walkthrough

1.  **Ingest** --- ADF copies source CRM/CSV data into the ADLS Gen2 raw
    zone. SHIR is used where connectivity to an on-premises environment
    is required.
2.  **Orchestrate** --- ADF pipelines coordinate copy and transformation
    activities. ADF is integrated with Git for version history and
    controlled deployment.
3.  **Land raw data** --- Preserve source extracts in the raw zone,
    ideally with source, ingestion-date, and load-run folder
    conventions.
4.  **Transform** --- Databricks notebooks use PySpark to validate,
    clean, standardise, deduplicate, and enrich the raw datasets.
5.  **Publish curated data** --- Write analytics-ready data to the
    curated zone in Parquet or Delta format, using a consistent schema
    and partition strategy where appropriate.
6.  **Protect secrets** --- Store credentials in Azure Key Vault and
    access them through a Databricks-backed secret scope. Do not
    hard-code secrets in notebooks, pipeline JSON, or Git.
7.  **Query** --- Synapse serverless SQL reads curated files directly
    from ADLS Gen2, avoiding a dedicated data warehouse for this use
    case.
8.  **Visualise** --- Power BI connects to Synapse SQL views or queries
    to present KPIs, trends, and drill-down analysis.
9.  **Observe and alert** --- Azure Monitor tracks operational signals;
    Logic Apps sends pipeline outcome notifications to the configured
    recipients.

## Data Lake Design

A simple two-zone layout is used to separate source preservation from
analytical data.

``` text
<storage-account>/
└── <container>/
    ├── raw/
    │   └── crm_sales/
    │       └── load_date=YYYY-MM-DD/
    └── curated/
        └── crm_sales/
            └── load_date=YYYY-MM-DD/
```

-   **Raw zone:** source-aligned data, retained for replay and
    traceability.
-   **Curated zone:** cleaned, conformed, analytics-ready datasets for
    SQL and BI consumption.

> The two-zone layout is often described as a simplified medallion
> architecture (raw/bronze and curated/silver or gold, depending on the
> transformations and business readiness). Avoid calling it a full
> bronze--silver--gold implementation unless all three layers are
> actually built.

## Data Quality and Transformation

Databricks transformation notebooks can implement checks such as:

-   Required-column and schema validation
-   Null handling for mandatory fields
-   Duplicate detection using business keys
-   Date, currency, and numeric type standardisation
-   Reconciliation of input and output row counts
-   Logging rejected or invalid records for investigation

Suggested curated entities include sales transactions, customers,
products, and sales territories---adjust these to match the actual
source schema.

## Security and Governance

-   Use Azure Key Vault as the central secret store.
-   Access Key Vault secrets through an appropriately configured
    Databricks Secret Scope.
-   Apply least-privilege Azure RBAC and ADLS ACLs to identities and
    users.
-   Prefer managed identities where supported; avoid storage account
    keys when possible.
-   Keep secrets, credentials, and sensitive source data out of Git.
-   Restrict Power BI and Synapse access to approved users and datasets.

## Reliability and Monitoring

-   Configure ADF activity retries and timeouts intentionally.
-   Route success/failure outcomes to Logic Apps for email notification.
-   Use Azure Monitor and ADF monitoring views to investigate run
    history, duration, and failures.
-   Include run identifiers and ingestion timestamps for traceability.
-   Consider idempotent loads so a rerun does not create unintended
    duplicates.

## Analytics and Visualisation

Power BI dashboards can be designed around business questions such as:

-   What are total sales and sales trends over time?
-   Which products or categories generate the most revenue?
-   How do sales compare across territories or regions?
-   Which customers contribute the most to sales?
-   How do monthly, quarterly, or year-over-year results change?

**Potential KPI cards:** Total Revenue, Order Count, Average Order
Value, Top Product, and Sales Growth. These are suggested metrics;
implement them according to the available fields and business
definitions.

## Technology Stack

  -----------------------------------------------------------------------
  Area                                Technology
  ----------------------------------- -----------------------------------
  Cloud platform                      Microsoft Azure --- UK South
                                      (project deployment region)

  Ingestion and orchestration         Azure Data Factory

  Hybrid connectivity                 Self-Hosted Integration Runtime

  Data storage                        Azure Data Lake Storage Gen2

  Processing                          Azure Databricks, PySpark

  Secret management                   Azure Key Vault, Databricks Secret
                                      Scope

  SQL analytics                       Azure Synapse Analytics serverless
                                      SQL

  Reporting                           Power BI

  Notifications                       Azure Logic Apps

  Monitoring                          Azure Monitor

  Version control                     Git
  -----------------------------------------------------------------------

## Getting Started

### Prerequisites

-   An Azure subscription with permissions to create or access the
    services listed above.
-   Azure Data Factory, ADLS Gen2, Azure Databricks, Azure Key Vault,
    and Synapse workspace resources.
-   Power BI Desktop (for report development).
-   A CRM sales dataset in CSV format (use appropriately licensed or
    anonymised data).
-   Git repository access.
-   Self-Hosted Integration Runtime installed on a machine with access
    to the source, if the data is on-premises.

### Setup outline

1.  Clone this repository.
2.  Create or configure the Azure resources in the target region.
3.  Create ADLS Gen2 raw and curated containers/folders.
4.  Configure ADF linked services and datasets; use SHIR for on-premises
    connectivity when needed.
5.  Connect ADF to Git and publish pipeline artifacts through your
    chosen deployment workflow.
6.  Configure Key Vault and the Databricks secret scope with
    least-privilege access.
7.  Import and configure Databricks notebooks; parameterise storage
    paths and input locations.
8.  Run ingestion into raw, then execute transformations and publish
    curated output.
9.  Define Synapse serverless SQL views over curated files.
10. Connect Power BI to Synapse and build/report the agreed metrics.
11. Configure Logic Apps notifications and validate monitoring by
    testing both successful and failed runs.

> Resource names, linked-service settings, secret names, and notebook
> paths are environment-specific. Do not commit credentials or live
> connection strings.

## Repository Structure

``` text
CRM_SALES_UKSOUTH_AZURE_PROJECT/
├── README.md
├── architecture_diagram.png
├── adf/
│   ├── pipelines/
│   ├── datasets/
│   └── linked_services/       # Sanitised templates only
├── databricks/
│   └── notebooks/
│       ├── 01_ingest_validation.py
│       └── 02_transform_curate.py
├── synapse/
│   └── sql/
│       └── curated_views.sql
├── powerbi/
│   └── reports/               # Report files / screenshots
└── docs/
    └── data_dictionary.md
```

Update this tree to reflect the artifacts actually present in your
repository.

## Project Status and Future Enhancements

This README describes the intended project workflow. Mark components as
completed only after they have been implemented and tested in your Azure
environment.

Potential next steps:

-   Add a third gold/business layer for dimensional models and
    aggregated reporting.
-   Add automated data quality tests and pipeline validation.
-   Introduce CI/CD deployment for ADF and notebook assets.
-   Add incremental ingestion using watermarks.
-   Implement a dimensional model (fact sales and dimension tables) if
    the reporting requirements call for it.
-   Document data lineage, schema, and KPI definitions.

## Author

**Akash** · Azure Data Engineering Portfolio Project

------------------------------------------------------------------------

::: {align="center"}
[⬆ Back to top](#-crm-sales-data-engineering-pipeline--azure-uk-south)
:::
