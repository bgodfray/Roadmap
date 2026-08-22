# ✈️ British Airways: Heathrow T3 Lounge Capacity Pipeline (Phase 1)

**Part of the British Airways Enterprise Analytics Portfolio**

## 🎯 Executive Summary & Business Impact
British Airways requires accurate forecasting for lounge capacity at Heathrow Terminal 3 to optimize staffing, manage costs, and ensure a premium customer experience. Passenger eligibility is determined by a complex matrix of cabin class, frequent flyer tiers, and partner airline status.

This project solves this capacity planning challenge by building a scalable, cloud-native data pipeline. It ingests raw flight schedules and passenger manifests, translates complex business rules into a dynamic SQL lookup table, and outputs automated capacity forecasts via Power BI.

## 🏗️ Cloud Architecture
This pipeline utilizes a serverless AWS architecture to minimize compute costs while maintaining enterprise-grade security and scalability.

`Raw Flight/Passenger Data (CSV)` ➡️ `AWS S3 (Secure Data Lake)` ➡️ `AWS Athena (Serverless SQL)` ➡️ `Power BI (Star Schema / DAX)`

## 🗂️ Repository Structure
*   📄 `athena_eligibility_lookup.sql`: The core SQL script executing the business logic to determine lounge access.
*   📊 `BA_Lounge_Capacity_Forecast.pbix`: The Power BI dashboard containing the semantic data model and executive visualizations.
*   📝 `data_dictionary_and_assumptions.md`: Documentation of the business rules, transit time assumptions, and data quality checks applied.
*   🖼️ `/docs/architecture_diagram.png`: Visual map of the AWS infrastructure.

## 🧠 Business Logic & Assumptions applied
Translating raw data into actionable capacity metrics required strict business logic implementation:
1.  **Eligibility Matrix:** Access is granted to First/Business class passengers and Oneworld Emerald/Sapphire tier members flying on eligible carriers.
2.  **Capacity Forecasting:** Modeled peak utilization assumes a standard 90-minute dwell time prior to boarding, adjusted for historical no-show rates. 
3.  **Data Quality:** Implemented robust checks in AWS Athena to handle null frequent-flyer fields and inconsistent flight codes before passing data to Power BI.

## 🚀 Next Steps (Phase 2 - Late 2027)
This repository establishes the foundational data infrastructure. Phase 2 will utilize this cleaned dataset to train a predictive machine learning model (AWS SageMaker / XGBoost) to predict customer buying behavior and proactive upgrade targeting.

## Task
