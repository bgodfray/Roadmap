# 📋 Business Requirements: Heathrow T3 Lounge Capacity 

## 📖 Scenario Context
British Airways needs to plan for future lounge demand using strategic data modeling at Heathrow Terminal 3. The objective is to review lounge eligibility criteria, explore how customer groupings inform demand, and build a reusable capacity forecasting model.

## 🎯 Task Instructions & Scope

1. **Flight Schedule Analysis:** Review the provided raw flight schedule dataset.
2. **Categorization:** Assign flights to defined categories (e.g., by time of day, route type, or destination region) rather than modeling on a per-flight basis.
3. **Eligibility Application:** Apply estimated eligibility percentages to each category to calculate the number of passengers likely to utilize each lounge.
4. **Data Sampling:** Select a representative sample of flights (e.g., a specific time window or set of destinations) to test groupings and apply assumptions meaningfully.

## ⚠️ Key Constraints & Edge Cases
* **The "Concorde Room" Exception:** While there is currently no Concorde Room (Tier 1) at Terminal 3, estimates should still reflect passengers who would qualify for this level of service. This data acts as a hypothetical/potential space analysis to inform future capital expenditure, not a confirmed development.

## 📦 Expected Output
A reusable, generalized SQL lookup table/matrix that can be systematically applied to any future flight schedules to generate capacity estimates.
