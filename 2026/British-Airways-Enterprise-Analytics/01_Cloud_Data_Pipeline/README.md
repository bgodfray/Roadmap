# ✈️ British Airways: Heathrow T3 Lounge Capacity Pipeline (Phase 1)

**Part of the British Airways Enterprise Analytics Portfolio**

## Executive Summary & Business Impact
British Airways requires accurate forecasting for lounge capacity at Heathrow Terminal 3 to optimize staffing, manage costs, and ensure a premium customer experience. Passenger eligibility is determined by a complex matrix of cabin class, frequent flyer tiers, and partner airline status.

This project solves this capacity planning challenge by building a scalable, cloud-native data pipeline. It ingests raw flight schedules and passenger manifests, translates complex business rules into a dynamic SQL lookup table, and outputs automated capacity forecasts via Power BI.

## Cloud Architecture
This pipeline utilizes a serverless AWS architecture to minimize compute costs while maintaining enterprise-grade security and scalability.

## Raw Flight/Passenger Data (CSV)
Copy of the task file from BA on Forage -> [file](https://github.com/bgodfray/LearningRoadmap/blob/main/2026/British-Airways-Enterprise-Analytics/01_Cloud_Data_Pipeline/British%20Airways%20Summer%20Schedule%20Dataset%20-%20Forage%20Data%20Science%20Task%201.xlsx) 

File changed to CSV -> [file](https://github.com/bgodfray/LearningRoadmap/blob/main/2026/British-Airways-Enterprise-Analytics/01_Cloud_Data_Pipeline/ba_summer_schedule.csv)

## Infrastructure & Data Ingestion (AWS S3 & Athena)

To ensure a production-grade schema-on-read architecture, the raw data ingestion was strictly separated from the analytical output. 

1. **Storage (AWS S3):** Raw CSV schedules were ingested into a secure S3 bucket, isolated within a dedicated `raw_data/` directory to prevent Athena from recursively scanning its own output metadata.
2. **Schema Definition (AWS Athena):** The table was instantiated using an external DDL script. 
3. **Data Quality Handling:** The raw CSV extract contained inconsistent double-quote formatting. Rather than cleaning this manually in Python or Excel, I implemented the `OpenCSVSerde` within the table properties to dynamically strip quotes and escape characters during the read process, ensuring clean downstream aggregations.

<details>
<summary><b>Click to view the DDL (Create Table) Script</b></summary>

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS ba_summer_schedule (
  `FLIGHT_DATE` string,
  `FLIGHT_TIME` string,
  `TIME_OF_DAY` string,
  `AIRLINE_CD` string,
  `FLIGHT_NO` string,
  `DEPARTURE_STATION_CD` string,
  `ARRIVAL_STATION_CD` string,
  `ARRIVAL_COUNTRY` string,
  `ARRIVAL_REGION` string,
  `HAUL` string,
  `AIRCRAFT_TYPE` string,
  `FIRST_CLASS_SEATS` string,
  `BUSINESS_CLASS_SEATS` string,
  `ECONOMY_SEATS` string,
  `TIER1_ELIGIBLE_PAX` string,
  `TIER2_ELIGIBLE_PAX` string,
  `TIER3_ELIGIBLE_PAX` string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   'separatorChar' = ',',
   'quoteChar' = '\"',
   'escapeChar' = '\\'
)
STORED AS TEXTFILE
LOCATION 's3://ba-tasks-bg/raw_data/'
TBLPROPERTIES ('skip.header.line.count'='1');
```
</details>

## Repository Structure
*   **[business_requirements.md](./business_requirements.md):** The original business prompt, scope, and scenario constraints.
*   `athena_eligibility_lookup.sql`: The core SQL script executing the business logic to determine lounge access.
*   `BA_Lounge_Capacity_Forecast.pbix`: The Power BI dashboard containing the semantic data model and executive visualizations.
*   **[data_dictionary_and_assumptions.md](./data_dictionary_and_assumptions.md):** Documentation of the transit time assumptions and data quality checks applied.
*   `/docs/architecture_diagram.png`: Visual map of the AWS infrastructure.
*   
## Business Logic & Assumptions applied
Translating raw data into actionable capacity metrics required strict business logic implementation:
1.  **Eligibility Matrix:** Access is granted to First/Business class passengers and Oneworld Emerald/Sapphire tier members flying on eligible carriers.
2.  **Capacity Forecasting:** Modeled peak utilization assumes a standard 90-minute dwell time prior to boarding, adjusted for historical no-show rates. 
3.  **Data Quality:** Implemented robust checks in AWS Athena to handle null frequent-flyer fields and inconsistent flight codes before passing data to Power BI.

## Next Steps (Phase 2 - Late 2027)
This repository establishes the foundational data infrastructure. Phase 2 will utilize this cleaned dataset to train a predictive machine learning model (AWS SageMaker / XGBoost) to predict customer buying behavior and proactive upgrade targeting.
