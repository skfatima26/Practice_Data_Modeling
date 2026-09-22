# Databricks Data Modeling & Medallion Architecture

## Project Overview

This repository contains my hands-on practice and implementation of core **Data Engineering, Data Modeling, ETL, and Databricks Lakehouse concepts**.

The project was developed using **Databricks Free Edition** and focuses on understanding how raw data can be progressively transformed into clean, structured, and analytics-ready data using modern data engineering patterns.

Rather than focusing only on theoretical concepts, this project applies the concepts through practical SQL/PySpark implementations and follows data engineering best practices wherever applicable.

---

## Objectives

The main objectives of this project are to:

* Understand the fundamentals of Data Modeling.
* Differentiate between OLTP and OLAP systems.
* Understand ETL architecture and data flow.
* Explore the Databricks Lakehouse environment.
* Implement the **Medallion Architecture**.
* Perform incremental data loading.
* Build and transform Bronze, Silver, and Gold layers.
* Implement `MERGE` / `UPSERT` operations.
* Understand dimensional data modeling.
* Build Fact and Dimension tables.
* Implement Star and Snowflake schemas.
* Understand different types of Facts and Dimensions.
* Implement Slowly Changing Dimensions (SCD).
* Understand the practical differences between SCD Type 1 and Type 2.
* Apply scalable and maintainable data engineering practices.

---

# Concepts Covered

## 1. Data Modeling Fundamentals

Data Modeling is the process of designing how data is structured, stored, related, and accessed within a data system.

The project covers:

* Data modeling fundamentals
* Entities and attributes
* Relationships
* Primary Keys
* Foreign Keys
* Normalization
* Denormalization
* Analytical data modeling
* Transactional vs analytical workloads

---

## 2. OLTP vs OLAP

Understanding the difference between transactional and analytical database systems is an important foundation for Data Engineering.

### OLTP — Online Transaction Processing

Designed for frequent transactional operations such as:

* INSERT
* UPDATE
* DELETE
* Point lookups
* High-volume transactional workloads

Examples include:

* Banking systems
* E-commerce transactions
* Order management systems
* Customer applications

### OLAP — Online Analytical Processing

Designed for:

* Large-scale analytical queries
* Aggregations
* Reporting
* Business Intelligence
* Historical analysis

The project explores why analytical workloads commonly use dimensional models containing **Fact and Dimension tables**.

---

# 3. ETL Fundamentals & Architecture

The project covers the fundamentals of:

**Extract → Transform → Load**

### Extract

Data is collected from source systems such as:

* Files
* Databases
* Applications
* External systems

### Transform

Data is:

* Cleaned
* Standardized
* Validated
* Deduplicated
* Enriched
* Restructured

### Load

The transformed data is loaded into analytical storage for downstream consumption.

The project also explores how modern lakehouse architectures organize these transformations into multiple layers.

---

# 4. Databricks Free Edition

The project includes hands-on exploration of **Databricks Free Edition**.

Areas explored include:

* Databricks Workspace
* Notebooks
* SQL
* PySpark
* Tables
* Delta Lake concepts
* Data transformation workflows
* Data engineering development patterns

The goal was to understand how Databricks can be used as a modern data engineering and lakehouse platform.

---

# 5. Medallion Architecture

A major focus of this project is the **Medallion Architecture**.

The data flows through three primary layers:

```text
Source Data
    ↓
┌──────────────┐
│ Bronze Layer │
│ Raw Data     │
└──────────────┘
        ↓
┌──────────────┐
│ Silver Layer │
│ Cleaned Data │
└──────────────┘
        ↓
┌──────────────┐
│ Gold Layer   │
│ Business     │
│ Ready Data   │
└──────────────┘
```

### Bronze Layer

The Bronze layer stores data in a raw or minimally transformed form.

Practiced concepts include:

* Initial ingestion
* Incremental data loading
* Raw data preservation
* Loading new records
* Maintaining source-level information

---

### Silver Layer

The Silver layer contains cleaned and refined data.

Practiced transformations include:

* Data cleansing
* Standardization
* Data type transformations
* Deduplication
* Incremental processing
* `MERGE`
* `UPSERT`
* Updating existing records
* Inserting new records

Example pattern:

```sql
MERGE INTO target_table AS target
USING source_table AS source
ON target.id = source.id

WHEN MATCHED THEN
    UPDATE SET *

WHEN NOT MATCHED THEN
    INSERT *
```

The exact implementation may vary depending on the table and business requirement.

---

### Gold Layer

The Gold layer focuses on business-ready and analytics-ready datasets.

This layer can contain:

* Fact tables
* Dimension tables
* Aggregated datasets
* Analytical views
* Reporting-ready structures

The project uses dimensional modeling concepts to organize data for analytical workloads.

---

# 6. Incremental Data Loading

The project explores incremental processing instead of repeatedly processing the entire dataset.

Key concepts practiced:

* Identifying new records
* Processing only changed/new data
* Incremental ingestion
* Avoiding unnecessary full reloads
* Maintaining target tables
* Using merge/upsert patterns

Incremental processing is particularly important when dealing with growing datasets because repeatedly reprocessing the entire dataset can increase processing time and resource consumption.

---

# 7. MERGE / UPSERT Operations

The project includes practical implementation of `MERGE` / `UPSERT` patterns.

The basic concept is:

```text
Source Data
     ↓
Compare with Target
     ↓
 ┌───────────────┐
 │ Existing Key? │
 └───────────────┘
       ↓     ↓
      YES    NO
       ↓     ↓
    UPDATE  INSERT
```

This pattern is useful when maintaining incrementally updated tables.

---

# 8. Dimensional Data Modeling

The project explores dimensional modeling for analytical systems.

The major components include:

### Dimension Tables

Dimension tables describe business entities.

Examples:

* Customer
* Product
* Date
* Location
* Employee

Typical attributes may include:

```text
customer_id
customer_name
city
country
customer_type
```

---

### Fact Tables

Fact tables contain measurable business events.

Examples:

* Sales
* Orders
* Transactions
* Payments
* Bookings

Typical columns may include:

```text
order_id
customer_key
product_key
date_key
quantity
sales_amount
```

---

# 9. Star Schema

The project covers the **Star Schema** design.

A typical structure looks like:

```text
             Dim Customer
                  |
                  |
Dim Product ── Fact Sales ── Dim Date
                  |
                  |
             Dim Location
```

The Fact table is positioned at the center and connects to multiple Dimension tables.

Star schemas are commonly used for analytical workloads because they provide a straightforward structure for querying and aggregating business data.

---

# 10. Snowflake Schema

The project also explores the **Snowflake Schema**.

Unlike a Star Schema, dimensions can be further normalized into related tables.

Example:

```text
                 Dim Country
                      |
                 Dim Customer
                      |
Dim Product ─── Fact Sales ─── Dim Date
```

The project compares the structure and trade-offs between Star and Snowflake schemas.

---

# 11. Types of Facts

The project covers different types of fact tables, including concepts such as:

* Transaction Fact Tables
* Periodic Snapshot Fact Tables
* Accumulating Snapshot Fact Tables
* Factless Fact Tables

These different fact types are useful depending on the business process and analytical requirements.

---

# 12. Types of Dimensions

The project explores different dimension concepts, including:

* Conformed Dimensions
* Role-Playing Dimensions
* Degenerate Dimensions
* Junk Dimensions
* Slowly Changing Dimensions

Understanding these patterns helps design reusable and scalable analytical data models.

---

# 13. Slowly Changing Dimensions

Slowly Changing Dimensions (SCD) are techniques used to manage changes in dimension data over time.

The project focuses particularly on:

### SCD Type 1

SCD Type 1 overwrites the existing value.

Example:

```text
Before:
Customer → Mumbai

After:
Customer → Pune
```

The previous value is not retained.

This approach is useful when historical changes are not required.

---

### SCD Type 2

SCD Type 2 preserves historical versions of records.

A typical implementation can contain:

```text
customer_id
customer_name
city
effective_date
end_date
is_current
```

Example:

```text
Customer 101
Mumbai
2025-01-01
2026-03-15
False

Customer 101
Pune
2026-03-15
NULL
True
```

Instead of overwriting the original record, a new version is created.

This allows historical analysis of how dimensional attributes changed over time.

---

# 14. Best Practices Applied

Throughout the implementation, the project focuses on practical data engineering practices such as:

* Layered data architecture
* Incremental processing
* Reusable transformation logic
* Clear separation of raw, refined, and analytical data
* Appropriate use of Fact and Dimension tables
* Meaningful table and column naming
* Proper key design
* Handling changing dimension records
* Avoiding unnecessary full-table processing
* Maintaining historical information where required
* Designing data structures according to analytical requirements

---

# Technology Stack

| Technology       | Usage                                      |
| ---------------- | ------------------------------------------ |
| **Databricks**   | Data engineering and lakehouse environment |
| **Apache Spark** | Distributed data processing                |
| **PySpark**      | Data transformation and processing         |
| **Spark SQL**    | SQL-based transformations                  |
| **Delta Lake**   | Reliable analytical table storage          |
| **SQL**          | Data querying and transformation           |
| **Git & GitHub** | Version control and project documentation  |

---

# Learning & Implementation Flow

```text
Data Engineering Fundamentals
            ↓
      Data Modeling
            ↓
       OLTP vs OLAP
            ↓
     ETL Architecture
            ↓
       Databricks
            ↓
   Medallion Architecture
            ↓
   Bronze → Silver → Gold
            ↓
 Incremental Data Loading
            ↓
      MERGE / UPSERT
            ↓
 Dimensional Data Modeling
            ↓
 Fact + Dimension Tables
            ↓
 Star vs Snowflake Schema
            ↓
 Slowly Changing Dimensions
            ↓
      SCD Type 1 & 2
```

---

#  Project Structure

The repository is organized to keep the different concepts and implementations easy to understand.

```text
Databricks-Data-Modeling/
│
├── Data_Modeling/
│   ├── OLTP_vs_OLAP/
│   ├── Dimensional_Modeling/
│   ├── Fact_Tables/
│   ├── Dimension_Tables/
│   └── Star_vs_Snowflake/
│
├── Medallion_Architecture/
│   ├── Bronze/
│   ├── Silver/
│   └── Gold/
│
├── Incremental_Loading/
│
├── MERGE_UPSERT/
│
├── SCD/
│   ├── SCD_Type_1/
│   └── SCD_Type_2/
│
└── README.md
```

> The final folder structure may be adjusted according to the notebooks and implementations exported from Databricks.

---

# Key Takeaways

Through this hands-on practice, I strengthened my understanding of:

* Modern Data Engineering architecture
* Data Modeling
* Data Warehousing concepts
* ETL fundamentals
* Databricks
* Lakehouse architecture
* Medallion Architecture
* Incremental data processing
* Delta-based merge/upsert patterns
* Dimensional modeling
* Fact and Dimension design
* Star and Snowflake schemas
* Slowly Changing Dimensions
* SCD Type 1 and Type 2

The project helped bridge the gap between **data engineering theory and practical implementation** by applying these concepts in a Databricks environment.

---

## Future Improvements

Potential extensions to this project include:

* Automated Databricks Workflows
* Data quality checks
* Pipeline monitoring
* Data validation frameworks
* Advanced PySpark transformations
* Structured Streaming
* Change Data Capture (CDC)
* Unity Catalog governance
* Databricks CI/CD
* Production-style orchestration
* End-to-end analytics pipelines

---

## Author

**Fatima Shaikh**

Aspiring Data Engineer | Python | SQL | Databricks | Apache Spark | Data Modeling | ETL | Cloud Data Engineering

---
This repository represents my hands-on practice in modern Data Engineering and Data Modeling using Databricks.
