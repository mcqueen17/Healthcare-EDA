# Case Study: Reducing 30-Day Hospital Readmissions

## Business Problem
High readmission rates lead to financial penalties and indicate gaps in care transitions.  
The goal is to analyze 2,000 encounters to identify clinical and operational drivers.

## Key Questions
1. What is the overall 30-day readmission rate?
2. Which medical conditions have the highest risk?
3. Are readmissions clustered in the first week after discharge?
4. Which patients have multiple repeat readmissions (“frequent flyers”)?
5. How do readmission patterns change month-to-month?

## Methods
- SQL-based cohort creation  
- KPI development (30-day rate, 7-day rate, condition-specific KPIs)  
- Python-based EDA and visualizations  
- Feature engineering: days_to_readmission, readmit_bucket, frequent_flyer_flag  
- Trend and condition segmentation  

## Key Findings
- The overall 30-day readmission rate was **X%**.
- Conditions with the highest readmission risk: **A, B, C**.
- A large share of readmissions occurred within **0–7 days**, indicating preventable patterns.
- **XX%** of total readmissions were driven by frequent-flyer patients.
- Readmissions spiked in **Month X**, suggesting operational factors.

## Recommended Interventions
- Post-discharge follow-up calls within 48 hours  
- Standardized medication reconciliation  
- Condition-specific discharge education  
- Care management referral for high-risk conditions  
- Additional touchpoints for 0–7 day high-risk windows

## Deliverables
- SQL logic for KPI development  
- EDA notebook  
- Visualization suite  
- Clean dataset with engineered features  

