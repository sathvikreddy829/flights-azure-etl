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
