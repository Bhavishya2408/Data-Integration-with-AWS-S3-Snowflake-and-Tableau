# ⚡ Renewable Energy Analytics: AWS + Snowflake + Tableau Integration

## 📌 Project Overview

This project demonstrates how to integrate **Amazon Web Services (AWS)**, **Snowflake**, and **Tableau** to analyze renewable energy usage and cost savings across different regions and energy sources. The goal is to streamline data ingestion, transformation, and visualization to uncover actionable insights.

---

## 🧰 Tech Stack

- **AWS S3**: Cloud storage for raw CSV data  
- **Snowflake**: Data warehouse for SQL-based data transformation  
- **Tableau**: Visualization tool for interactive dashboards

---

## 🔄 Workflow

### Step 1: S3 Bucket Setup
- Created a bucket: `tableauproject`
- Uploaded: `Renewable_Energy_Usage_Sampled.csv`

### Step 2: IAM Role Configuration
- Created roles: `Snowflake-Test-Role` and `Tableau-Role`
- Assigned `AmazonS3FullAccess` policy and configured trust relationships

### Step 3: Snowflake Integration Setup
- Created a storage integration object: `tableau_Integration`
- Verified with:  
  ```sql
  DESC INTEGRATION tableau_Integration;
  ```

### Step 4: Load Data into Snowflake
```sql
CREATE DATABASE tableau;
CREATE SCHEMA tableau_Data;

CREATE TABLE tableau_dataset (
    Household_ID STRING,
    Region STRING,
    Country STRING,
    Energy_Source STRING,
    Monthly_Usage_kWh FLOAT,
    Year INT,
    Household_Size INT,
    Income_Level STRING,
    Urban_Rural STRING,
    Adoption_Year INT,
    Subsidy_Received STRING,
    Cost_Savings_USD FLOAT
);

CREATE STAGE tableau_Data.tableau_stage
  URL = 's3://tableauproject'
  STORAGE_INTEGRATION = tableau_Integration;

COPY INTO tableau_dataset
FROM @tableau_stage
FILE_FORMAT = (TYPE=CSV FIELD_DELIMITER=',' SKIP_HEADER=1)
ON_ERROR = 'CONTINUE';
```

### Step 5: Data Understanding & Transformation
- Performed SQL queries in Snowflake to assess completeness and analyze metrics (e.g., usage by region, source, etc.)

### Step 6: Tableau Connection
- Connected Snowflake live to Tableau using:
  - **Database**: `tableau`
  - **Schema**: `tableau_data`
  - **Table**: `energy_consumption`

### Step 7: Visualizations Created in Tableau
- Monthly Usage by Region, Country, Energy Source
- Cost Savings by Region, Country, Energy Source

### Step 8: Dashboard Creation
- Built an interactive dashboard with filters for:
  - Region
  - Country
  - Energy Source

---

## 📊 Key Insights

- **High Usage Countries**: Australia, New Zealand, Canada  
- **Top Energy Sources**: Wind and Hydro for usage, Solar and Biomass for savings  
- **Regional Patterns**: South America and Africa lead in cost savings

---

## 📦 Key Deliverables

- ✅ AWS S3 bucket with uploaded dataset  
- ✅ IAM roles for Snowflake and Tableau access  
- ✅ Snowflake table and stage integration  
- ✅ Transformed, queryable dataset  
- ✅ Tableau dashboard for stakeholder insights

---

## ✅ Conclusion

This project demonstrates a successful end-to-end data pipeline using **AWS**, **Snowflake**, and **Tableau** to analyze renewable energy patterns. The resulting dashboard empowers stakeholders with real-time, interactive insights to make informed energy decisions.
