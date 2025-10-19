✈️ Databricks Flights End-to-End Data Engineering Project
🔹 Overview

The Flights End-to-End Project demonstrates a complete data engineering workflow using Databricks and the medallion architecture — consisting of Bronze, Silver, and Gold layers. The goal is to build a scalable, parameterized, and fully automated ETL pipeline that ingests raw flight-related datasets, applies transformations and validations, and produces analytics-ready tables.

🥉 Bronze Layer — Incremental Ingestion (Raw to Bronze)

The Bronze layer ingests raw source files (bookings, flights, airports, and customers) stored in workspace.raw volumes.

It uses Databricks Autoloader (cloudFiles) for incremental ingestion with schema inference, checkpointing, and schema evolution.

A parameterized notebook allows ingestion of multiple sources dynamically using Lakeflow Jobs and dbutils.widgets.

Each source writes Delta-formatted data into the workspace.bronze volume, ensuring incremental, append-only ingestion.

🥈 Silver Layer — Cleansing and Standardization

The Silver layer applies data validation, type casting, and business logic transformations using Delta Live Tables (DLT).

DLT pipelines define transformation stages (stage, trans, and silver tables) for each domain like bookings or flights.

Expectations are applied to enforce data quality rules (e.g., booking_id IS NOT NULL).

The layer ensures clean, standardized, and deduplicated data ready for analytics or fact/dimension modeling.

🥇 Gold Layer — Aggregation, Facts & Dimensions (Dynamic Modeling)

The Gold layer represents the final stage of the data pipeline, where analytical models — both Fact and Dimension tables — are built dynamically from the cleansed Silver data.

The process begins by combining and aggregating data from Silver tables such as silver_bookings, silver_flights, silver_airports, and silver_customers.

A dynamic dimension builder automatically detects each domain and creates dimension tables (e.g., dim_flights, dim_airports, dim_customers) without hardcoding logic for every source.

Each dimension table is generated using reusable PySpark logic that identifies key columns, assigns surrogate keys, and maintains create_date and update_date audit fields.

The SCD Type-1 merge strategy (using Delta MERGE) ensures that any data updates are reflected immediately while maintaining historical consistency.

A Fact_Bookings table is also created dynamically by joining the dimension tables with the booking details.

It aggregates booking-level data such as revenue, passenger counts, and flight information, linking them through surrogate keys.

This entire process is metadata-driven, meaning new tables or columns can be added with minimal manual configuration — the code adapts automatically to schema or data changes.

The resulting Fact and Dimension tables in the workspace.gold schema form a well-modeled star schema, optimized for analytics, BI dashboards, and advanced reporting in Power BI or Synapse Analytics.
