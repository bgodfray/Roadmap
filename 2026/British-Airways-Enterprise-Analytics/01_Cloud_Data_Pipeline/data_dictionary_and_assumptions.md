# 📖 Data Dictionary & Modeling Assumptions

## 1. Data Quality & Structure Assessment
An initial Exploratory Data Analysis (EDA) was conducted on the raw British Airways summer schedule dataset prior to cloud ingestion. 
*   **Fill Rate:** The dataset demonstrated exceptional data quality with a 100% fill rate across all 10,000 rows (zero `NULL` values). 
*   **Granularity:** The data is aggregated at the flight level, not the individual passenger level. It contains the total seat configuration and the exact count of eligible passengers per tier for each historical flight.

## 2. Categorization Strategy (The Grouping Logic)
To create a reusable forecasting model for future schedules, relying on specific flight numbers (e.g., BA123) is too rigid. Instead, the SQL model will need to use more broad categories. My recommendation is:
*   **Route Type:** `HAUL` (Short-Haul vs. Long-Haul)
*   **Time of Day:** `TIME_OF_DAY` (Morning, Afternoon, Evening)
*   *Example Output Category:* `Short-haul Morning`, `Long-haul Evening`.

## 3. Eligibility Calculation Methodology
To generate the probability matrix, the following logic is to be applied within AWS Athena to calculate the percentage of eligible passengers per category:
1.  **Total Capacity:** Calculated by adding `FIRST_CLASS_SEATS` + `BUSINESS_CLASS_SEATS` + `ECONOMY_SEATS`.
2.  **Tier Percentage Formulation:** 
    *   `Tier 1 %` = `(SUM(TIER1_ELIGIBLE_PAX) / SUM(Total Capacity)) * 100`
    *   `Tier 2 %` = `(SUM(TIER2_ELIGIBLE_PAX) / SUM(Total Capacity)) * 100`
    *   `Tier 3 %` = `(SUM(TIER3_ELIGIBLE_PAX) / SUM(Total Capacity)) * 100`

*Note: 
These percentages represent the proportion of the total aircraft capacity that is eligible for a specific lounge, not just the fill-rate of the premium cabins.
**Early Morning Utilization Penalty:** British Airways Terminal 3 lounges open at 5:00 AM. For flights departing at or before 6:30 AM, boarding commences shortly after lounge opening, restricting potential dwell time to under 30 minutes. Therefore, while total eligibility remains static, the assumed utilization rate for the 'Early Morning' category is modeled significantly lower than mid-day peaks to reflect real-world passenger behavior.
**Late Evening Utilization Penalty:** The British Airways Terminal 3 lounge facilities close at 22:00 (10:00 PM). For flights departing post-22:45, eligible passengers experience a forced cap on dwell time, and late-arriving passengers will bypass the lounge entirely. Consequently, the assumed utilization rate for the 'Late Evening' category is adjusted downward to account for facility closure constraints.

## 4. Edge Cases & Strategic Assumptions
*   **The Tier 1 (Concorde Room) Exception:** Terminal 3 does not currently feature a Concorde Room. However, per business requirements, Tier 1 eligibility percentages are still calculated. This serves as a hypothetical capacity forecast to inform future capital expenditure and lounge development strategy, rather than current operational staffing.
*   **Lounge Utilization Rate (Dwell Time):** While the SQL model calculates *eligibility*, the final Power BI visualizations assume a 100% utilization rate of eligible passengers for baseline peak modeling. In a production environment, this would be adjusted downward using historical scan-in data (no-show rates).
