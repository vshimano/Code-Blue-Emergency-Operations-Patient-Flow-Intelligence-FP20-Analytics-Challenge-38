# Code Blue: Emergency Operations & Patient Flow Intelligence
**FP20 Analytics Challenge 38**

## Overview

This Power BI report analyzes emergency care performance across an 11-hospital NHS network in England. Built for FP20 Analytics Challenge 38, the report uses ZoomCharts Drill Down Pro visuals for interactive exploration across three analytical pages: an executive overview, a patient journey deep-dive, and an operational pressure analysis.

---

## Report Pages

### Page 1: Overview
High-level view of patient volume, wait times, and mortality across all 11 hospitals. Includes year-over-year comparisons and a mortality risk heatmap by hospital and department.

### Page 2: Patient Journey and Outcomes
Explores the clinical patient pathway: triage and treatment timing by severity, length of stay by diagnosis, patient outcomes by diagnosis and severity, and the relationship between wait time and patient satisfaction.

### Page 3: Operational Pressure
Examines the operational drivers of performance: financial health by hospital, staff burnout and overtime vs patient satisfaction, ED demand by admission type, socioeconomic context by region, and the correlation between staff burnout and mortality rates.

---

## Data Engineering & Quality

Several data quality issues were identified and corrected before analysis:

- **Hospital_ID mapping error:** Hospital_ID values in all three fact tables (Fact_Patient_Visits, Fact_Staffing, Fact_Financials) were found to be inconsistent and did not reliably identify individual hospitals. All fact-to-dimension relationships were rebuilt using Hospital_Name as the join key to ensure accurate hospital-level analysis.

- **Recalculated metrics:** Wait_Time_Minutes and Length_of_Stay_Hours columns were found to be inconsistent with the source datetime columns. This was confirmed by the challenge organizer. Both metrics were recalculated directly from raw timestamps using DAX calculated columns:
  - `Arrival_to_Treatment_Minutes` = DATEDIFF(Arrival_DateTime, Treatment_Start_DateTime, MINUTE)
  - `Arrival_to_Discharge_Hours` = DATEDIFF(Arrival_DateTime, Discharge_DateTime, HOUR)

- **Missing relationship:** Fact_Patient_Visits joined to Dim_Diagnosis via Diagnosis_Category (containing codes like DX01) while Dim_Diagnosis used Diagnosis_ID as the key - a column name mismatch that prevented auto-detection. Relationship was manually configured.

---

## Data Model

Star schema with 3 fact tables and 7 dimension tables, 11 active relationships:

**Fact Tables:**
- Fact_Patient_Visits (9,994 rows) - core patient visit data
- Fact_Staffing (9,919 rows) - shift-level staffing metrics
- Fact_Financials (264 rows) - monthly hospital financial data

**Dimension Tables:**
- Dim_Hospital, Dim_Patient, Dim_Doctor, Dim_Diagnosis, Dim_Department, Dim_Region, Dim_Date

---

## Key Findings

- Average arrival-to-treatment time rose 4.25% in 2025 to 101 minutes, with a renewed upward trend after the January surge - suggesting worsening operational pressure beyond seasonal variation
- Severity 2 patients show unexpectedly higher mortality than Severity 3 across multiple diagnoses including Mental Health, Oncology and Infectious Disease - a counterintuitive pattern that warrants further investigation
- Staff burnout correlates with higher mortality rates and lower patient satisfaction, most severely at Sandwell General Hospital which stands out across all pressure metrics
- 21.4% of ED visits are elective or non-urgent, consuming critical capacity that could otherwise serve urgent cases
- Royal Cornwall is the only hospital where costs consistently exceed revenue - a pattern that may be linked to serving England's highest elderly population, though this requires further investigation with more granular data
- South East England consistently outperforms other regions across financial, staffing and patient experience metrics, correlating with the region's lower poverty rate and higher average income

---

## Tools & Technologies

- **Power BI Desktop**
- **ZoomCharts Drill Down Pro Visuals** (Combo Bar PRO, TimeSeries PRO, Scatter PRO, Donut PRO)
- **DAX** - calculated columns, measures, time intelligence
- **Power Query (M)** - data cleansing, custom columns, type transformations

---

## Data Source

FP20 Analytics Challenge 38 - provided by Federico Pastor / FP20 Analytics

---

## Author

Victoria Shimanovich

[LinkedIn](https://www.linkedin.com/in/victoria-shimanovich-a7309764/)
