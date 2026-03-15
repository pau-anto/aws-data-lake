# Analytics & Reporting — Amazon Athena

## Objective

The objective of this step was to perform **data analysis on the Data Lake** using **Amazon Athena**.  
Athena allows running **SQL queries directly on data stored in Amazon S3**, without requiring a traditional database system.

The tables used for the analysis were previously detected by **AWS Glue Crawler** and registered in the **Glue Data Catalog**, making them accessible from Athena.

---

## Analytical Queries

Several **Key Performance Indicators (KPIs)** were calculated in order to explore the mental health datasets.

### KPIs generated

- Total number of participants in the survey dataset
- Distribution of mental health treatments
- Impact of mental health on work performance
- Distribution of mental health diagnoses
- Average stress level depending on mental health condition

These analyses demonstrate how Athena can be used to perform **aggregations, counts and statistical calculations directly on S3 datasets**.

The SQL queries used for these analyses are available in: sql/kpis.sql

---

## Athena Query Execution

The queries were executed in the **Amazon Athena console**.

Athena reads the schema from the **Glue Data Catalog** and executes SQL queries on the datasets stored in the **Curated layer of the Data Lake**.

The results of the queries are automatically stored in the S3 bucket configured for Athena query results.

---

## Screenshots

Screenshots showing the execution of the queries and their results are available in: screenshots/04-analytics

These screenshots illustrate the KPIs generated using Athena.

---

## Visualization

The architecture includes **Amazon QuickSight** to create dashboards based on Athena queries.

However, due to permission restrictions in the **AWS Academy sandbox environment**, QuickSight could not be fully configured.

Despite this limitation, the analytical layer of the data lake remains compatible with visualization tools such as:

- Amazon QuickSight
- Power BI
- Excel

These tools can easily connect to Athena results to create dashboards and visualizations.

---

## Role in the Data Lake Architecture

This step represents the **Analytics layer** of the Data Lake architecture.

Pipeline overview:
S3 Curated Layer
↓
Glue Data Catalog
↓
Amazon Athena
↓
KPIs & Analytics

This layer transforms the stored data into **actionable insights through SQL-based analysis**.
