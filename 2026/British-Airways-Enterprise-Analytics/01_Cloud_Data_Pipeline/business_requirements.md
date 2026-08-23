# 📋 Business Requirements: Heathrow T3 Lounge Capacity 

## 📖 Scenario Context
British Airways needs to plan for future lounge demand using strategic data modeling at Heathrow Terminal 3. The objective is to review lounge eligibility criteria, explore how customer groupings inform demand, and build a reusable capacity forecasting model.

Need to understanding lounge eligibility
To begin modeling lounge demand, it’s important to understand who is typically eligible for lounge access. Lounge eligibility is generally based on customer loyalty status and travel class, with different access tiers offering varying levels of amenities.

Each tier supports a different group of travelers, and lounge capacity planning depends on forecasting how many eligible passengers fall into each of these categories.

<img width="1112" height="405" alt="image" src="https://github.com/user-attachments/assets/70f50a74-8827-4b24-8f2f-ac404d30e421" />

Creating eligibility assumptions
Now that you understand the lounge tiers, it’s time to think about how you’ll estimate the percentage of customers eligible for each tier across a flight schedule. Since BA is planning far into the future, your model needs to be flexible and based on high-level groupings—not specific flight numbers or aircraft types. 

Your goal is to create a lookup table that estimates lounge eligibility using clear, scalable categories. To do this, you’ll need to decide how to group flights and make logical assumptions.

Common groups include: 

Time of day: Early morning, mid-day, evening departures.
Type of route: Short-haul vs. long-haul
Region or destination group: Europe, North America, Asia, etc.
 
You’ll estimate what proportion of passengers in each group are likely to be eligible for:

Tier 1: Concorde Room
Tier 2: First Lounge
Tier 3: Club Lounge

There is no single correct approach—what matters most is that your assumptions are logical, justifiable, and easy to apply to future schedules. 

<img width="1920" height="400" alt="image" src="https://github.com/user-attachments/assets/3cd6c577-0f56-4a47-af23-573ba33e0ea8" />

## 🎯 Task Instructions & Scope

1. **Flight Schedule Analysis:** Review the provided raw flight schedule dataset.
2. **Categorization:** Assign flights to defined categories (e.g., by time of day, route type, or destination region) rather than modeling on a per-flight basis.
3. **Eligibility Application:** Apply estimated eligibility percentages to each category to calculate the number of passengers likely to utilize each lounge.
4. **Data Sampling:** Select a representative sample of flights (e.g., a specific time window or set of destinations) to test groupings and apply assumptions meaningfully.

## ⚠️ Key Constraints & Edge Cases
* **The "Concorde Room" Exception:** While there is currently no Concorde Room (Tier 1) at Terminal 3, estimates should still reflect passengers who would qualify for this level of service. This data acts as a hypothetical/potential space analysis to inform future capital expenditure, not a confirmed development.

## 📦 Expected Output
A reusable, generalized SQL lookup table/matrix that can be systematically applied to any future flight schedules to generate capacity estimates.
