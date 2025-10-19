✈️ Databricks Flights End-to-End Data Engineering Project

The Flights End-to-End Project demonstrates a complete data engineering workflow using Databricks and the medallion architecture — consisting of Bronze, Silver, and Gold layers. The goal is to build a scalable, parameterized, and fully automated ETL pipeline that ingests raw flight-related datasets, applies transformations and validations, and produces analytics-ready tables.

🥉 Bronze Layer — Incremental Ingestion (Raw to Bronze)

The Bronze layer ingests raw source files (bookings, flights, airports, and customers) stored in workspace.raw volumes.
It uses Databricks Autoloader (cloudFiles) for incremental ingestion with schema inference, checkpointing, and schema evolution.

A parameterized notebook allows ingestion of multiple sources dynamically using Lakeflow Jobs and dbutils.widgets.
Each source writes Delta-formatted data into the workspace.bronze volume, ensuring incremental, append-only ingestion.

![image alt](https://github.com/sathvikreddy829/flights-azure-etl/blob/9930465b318a6f38eca6c76703579eabdb022dbc/Screenshot%202025-10-19%20135848.png)

🥈 Silver Layer — Cleansing and Standardization

The Silver layer applies data validation, type casting, and business logic transformations using Delta Live Tables (DLT).
DLT pipelines define transformation stages for each domain like bookings or flights.

Expectations are applied to enforce data quality rules (e.g., booking_id IS NOT NULL).
The layer ensures clean, standardized, and deduplicated data ready for analytics or fact/dimension modeling.

![image alt](https://github.com/sathvikreddy829/flights-azure-etl/blob/402989c1cd42fe8283b0d7b26b0321a7ca883051/Screenshot%202025-10-19%20143841.png)

🥇 Gold Layer — Aggregation, Facts & Dimensions (Dynamic Modeling)

The Gold layer is the final stage that creates ready-to-use analytical tables.
It combines data from Silver tables like bookings, flights, customers, and airports.

A dynamic dimension builder automatically creates dimension tables (e.g., dimension flights, dimension customers) without hardcoding.
Each dimension gets surrogate keys, audit columns (create_date, update_date) and uses SCD Type-1 merges with Delta MERGE to update data consistently.

A Fact table is created dynamically by joining all dimension tables.
The fact table shows key metrics like revenue, passenger count, and flight details.

The resulting Fact and Dimension tables in the workspace.gold schema form a well-modeled star schema, optimized for analytics, BI dashboards, and advanced reporting in Power BI or Synapse Analytics.
