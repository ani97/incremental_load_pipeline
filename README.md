# 📌 Data Loading with Incremental Loading

## 🚀 Overview
This project demonstrates **incremental data loading** using Azure Synapse, Azure Data Factory, and SQL. The pipeline loads data from five tables while tracking changes using a **Watermark Table**.

## 🏗️ Schema Definition
We have created five tables:
- **Customer** (Delta Column: `Customer_ID`)
- **Employee** (Delta Column: `Updated_At`)
- **Product** (Delta Column: `Product_ID`)
- **Orders** (Delta Column: `Order_Date`)
- **Sales** (Delta Column: `Transaction_Date`)

## 📊 Watermark Table
The Watermark Table helps manage incremental loads:
- **SchemaName** - Stores schema names (e.g., `dbo`).
- **TableName** - Stores table names (e.g., `Customer`).
- **FolderPath** - Defines the storage location for processed data.
- **LPV (Last Processed Value)** - Stores the latest processed value for tracking.
- **DeltaColumn** - Specifies the column used for incremental checks.

## 🔄 Pipeline Steps
1. **Lookup Activity** - Reads the Watermark Table.
2. **Foreach Activity** - Iterates through tables.
3. **Lookup Max Value** - Gets the latest delta column value.
4. **IF Condition** - Compares LPV with Max Value to determine if new data exists.
5. **Copy Activity** - Loads new data to ADLS in **Parquet format**.
6. **Stored Procedure Activity** - Updates LPV in the Watermark Table.

## 📂 Folder Structure
```
📁 GitHub-Repo
│── 📄 README.md
│── 📂 Data
│   ├── customer_data.csv
│   ├── product_data.csv
│   ├── orders_data.csv
│   ├── sales_data.csv
│── 📂 SQL
│   ├── schema.sql
│   ├── insert_data.sql
│   ├── watermark_table.sql
│   ├── update_watermark_proc.sql
│── 📂 Pipelines
│   ├── incremental_pipeline.json
│── 📂 Docs
│   ├── Project_Documentation.pdf
```

## ⚡ Running the Pipeline
1. Open **Azure Synapse Workspace** and create a new pipeline.
2. Use Lookup Activity to read the Watermark Table.
3. Configure parameters (`schema_name`, `table_name`).
4. Run **debug mode** and validate outputs in ADLS.

## 📝 Additional Notes
- Ensure the SQL schema matches the expected data.
- Use **Synapse Linked Service** to connect to Azure SQL.
- The pipeline prevents empty file creation by checking `IF LPV < MAX(DeltaColumn)`.
