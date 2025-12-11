# Hospital Readmissions Analysis Using SQL & Exploratory Data Analysis

A full-stack healthcare analytics project demonstrating SQL, EDA, and clinical quality metrics.

## Project Overview

Hospital readmissions—especially within 7 and 30 days—are a core CMS quality metric and one of the most expensive and preventable healthcare events.
This project performs an end-to-end analysis of inpatient encounters and readmissions using:

Microsoft SQL Server (T-SQL)

Healthcare quality metric logic (CMS 30-day readmissions)

Exploratory Data Analysis (EDA)

Clinical insights for quality improvement and utilization management

The goal of this analysis is to understand:

Which patients are at highest risk of readmission

Which medical conditions contribute the most to readmissions

How readmission rates trend over time

Early (0–7 day) vs. standard (0–30 day) readmission patterns

Which patients experience multiple readmissions (“frequent flyers”)

Operational and clinical gaps based on readmission behavior

This project is structured exactly like a real Quality Analyst / Healthcare Data Analyst portfolio.

## 📂 Repository Structure
```sql
healthcare_readmissions_analysis/
│
├── data/
│   ├── hospital_readmissions.csv
│   ├── patient_encounters.csv
│   └── members.csv
│
├── sql/
│   ├── week1_utilization_metrics.sql
│   ├── week2_readmissions_analysis.sql
│   └── advanced_readmission_queries.sql
│
├── notebooks/
│   └── eda_hospital_readmissions.ipynb
│
├── images/
│   └── visuals_sample.png
│
└── README.md
```
## Key Questions Answered in This Project

✔ What is the hospital’s 30-day readmission rate?

✔ How does readmission rate vary by medical condition?

✔ Are certain months associated with higher readmissions?

✔ Who are the “frequent flyer” patients?

✔ How many patients are readmitted within 7 days (an early bounce-back)?


✔ What are the operational patterns behind readmission behavior?

## Dataset Description

This project uses three synthetic but realistic healthcare datasets:

### 1. hospital_readmissions.csv

    Encounter-level data including:

    encounter_id

    patient_name

    DOB

    date_of_admission

    date_of_discharge

    date_of_readmission

    primary & secondary medical conditions

    measure_name

### 2. patient_encounters.csv

    General utilization metrics:

    billing amount

    length of stay

    payor type

    admission/discharge dates

### 3. members.csv

    Member-level demographics:

    risk scores

    line of business (Medicaid/Medicare)

    SDoH flag

    geographic indicators

## Tools & Technologies Used
SQL Server / T-SQL

CTEs

GROUP BY + HAVING

Window functions

DATEDIFF logic

Dataset creation + transformations

Readmission rate calculation

Healthcare Analytics Concepts

CMS 30-day readmission definition

7-day early readmission identification

High-risk condition detection

Patient-level utilization patterns

Quality improvement metrics

## Core Analyses & SQL Logic

Below are examples of the types of metrics calculated in this project.

### 🔹 1. Identify All 30-Day Readmissions
```sql
SELECT
    COUNT(*) AS total_readmissions
FROM hospital_readmissions
WHERE date_of_readmission IS NOT NULL
  AND DATEDIFF(DAY, date_of_discharge, date_of_readmission) BETWEEN 0 AND 30;
```
### 🔹 2. Readmission Rate by Primary Medical Condition
```sql
SELECT
    primary_medical_condition,
    COUNT(*) AS total_discharges,
    SUM(CASE 
            WHEN DATEDIFF(DAY, date_of_discharge, date_of_readmission) BETWEEN 0 AND 30 
            THEN 1 ELSE 0 
        END) AS readmitted_30_days,
    CAST(SUM(CASE 
                WHEN DATEDIFF(DAY, date_of_discharge, date_of_readmission) BETWEEN 0 AND 30 
                THEN 1 ELSE 0 
             END) AS DECIMAL(10,2)) 
        / NULLIF(COUNT(*), 0) AS readmission_rate
FROM hospital_readmissions
GROUP BY primary_medical_condition
ORDER BY readmission_rate DESC;
```
### 🔹 3. Monthly Readmission Trend
```sql
SELECT
    FORMAT(date_of_discharge, 'yyyy-MM') AS discharge_month,
    COUNT(*) AS total_discharges,
    SUM(CASE 
            WHEN DATEDIFF(DAY, date_of_discharge, date_of_readmission) BETWEEN 0 AND 30 
            THEN 1 ELSE 0 
        END) AS readmitted_30_days,
    CAST(SUM(CASE 
                WHEN DATEDIFF(DAY, date_of_discharge, date_of_readmission) BETWEEN 0 AND 30 
             END) AS DECIMAL(10,2))
        / NULLIF(COUNT(*), 0) AS readmission_rate
FROM hospital_readmissions
GROUP BY FORMAT(date_of_discharge, 'yyyy-MM')
ORDER BY discharge_month;
```
### 🔹 4. Early Readmissions (0–7 Days)
```sql
SELECT
    encounter_id,
    patient_name,
    date_of_discharge,
    date_of_readmission,
    DATEDIFF(DAY, date_of_discharge, date_of_readmission) AS days_to_readmission
FROM hospital_readmissions
WHERE date_of_readmission IS NOT NULL
  AND DATEDIFF(DAY, date_of_discharge, date_of_readmission) BETWEEN 0 AND 7;

### 🔹 5. Frequent Flyers (Patients With >1 Readmission)
```sql
WITH frequent_patients AS (
    SELECT
        patient_name,
        COUNT(*) AS num_readmissions
    FROM hospital_readmissions
    WHERE date_of_readmission IS NOT NULL
    GROUP BY patient_name
    HAVING COUNT(*) > 1
)
SELECT
    hr.patient_name,
    hr.date_of_discharge,
    hr.date_of_readmission,
    DATEDIFF(DAY, hr.date_of_discharge, hr.date_of_readmission) AS days_to_readmission,
    fp.num_readmissions
FROM hospital_readmissions AS hr
JOIN frequent_patients AS fp
    ON hr.patient_name = fp.patient_name
WHERE hr.date_of_readmission IS NOT NULL
ORDER BY hr.patient_name, hr.date_of_discharge;
```
## Skills Demonstrated

### SQL / Data Engineering

Data modeling

Aggregation logic

Quality metric calculation

Error-proof analytics (NULL handling, DIV0 protection)

Window functions & CTEs

### Healthcare Domain Knowledge

CMS readmission definitions

Condition-based stratification

High-utilization patient identification

Trend analysis for quality improvement

### Business Intelligence

KPI development

Readmission dashboards

Performance insights for care management

Clinical operations reporting

## Why This Project Matters

Hospital readmissions represent:

Higher costs

Lower CMS star scores

Penalties and reimbursement reductions

Indicators of care coordination gaps

This project simulates exactly what a Quality Analyst / Population Health Analyst / Healthcare Data Analyst does in real roles.

It is a perfect portfolio example because it shows:

Real SQL skills

Real healthcare metrics

Real analytical reasoning

Real problem solving
