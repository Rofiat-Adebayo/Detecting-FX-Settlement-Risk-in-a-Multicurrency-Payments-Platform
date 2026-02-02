# Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform
Analysis of multicurrency transaction data to validate FX conversion accuracy, settlement integrity, and financial controls. Identifies data quality issues, reconciliation gaps, and governance risks in a fintech payments context.


## Table of Contents

- [Project Snapshot](#project-snapshot)
- [Project Background](#project-background)
- [Key Insight Areas](#key-insight-areas)
- [Data Structure & Initial Checks](#data-structure--initial-checks)
  - [Table Descriptions](#table-descriptions)
  - [Initial Data Quality Checks](#initial-data-quality-checks)
- [Executive Summary](#executive-summary)
- [Insights Deep Dive](#insights-deep-dive)
  - [Data Quality & Integrity](#data-quality--integrity)
  - [Settlement Accuracy & FX Validation](#settlement-accuracy--fx-validation)
  - [Operational Performance & Completion Trends](#operational-performance--completion-trends)
  - [Financial Controls, Governance & Risk](#financial-controls-governance--risk)
-  [Recommendations](#recommendations)
-  [Assumptions and Caveats](#assumptions-and-caveats)
- [Author](#Author)



## Project Snapshot
- Analyzed **1,000+ multi-currency transactions** to assess FX conversion accuracy and settlement integrity  
- Joined transaction and FX rate datasets using Excel (XLOOKUP) to compute expected USD settlement values, and applied data quality and reconciliation validation checks        covering duplicates, FX coverage, invalid amounts, and deviation thresholds 
- Identified a **critical settlement data governance gap** that prevents ±0.5% deviation monitoring, with direct financial and audit risk implications  


**Tools & Skills:**Tools & Skills: Excel (XLOOKUP, calculated fields), data quality analysis, FX rate validation, reconciliation logic, financial controls analysis, BI reporting




## Project Background

This project was conducted from the perspective of a **Data Analyst working within a multinational payments and settlement platform** in the fintech industry. The company enables **cross-border, multi-currency transactions** for businesses via web, mobile, and API-based channels.


The platform’s business model centers on:
- High-volume transaction processing
- FX-based currency conversion
- USD settlement for global merchants
- Reliability, auditability, and regulatory compliance


Key business metrics monitored include:
- Total USD settlement value
- Transaction and customer counts
- Settlement success, failure, and pending rates
- FX conversion accuracy and reconciliation gaps


Given the financial and regulatory exposure associated with FX discrepancies, this analysis focuses on **transaction integrity, settlement validation logic, and governance controls**.



### Key Insight Areas
 Data Quality & Integrity  
 Settlement Accuracy & FX Validation  
 Operational Performance & Completion Trends  
 Financial Controls, Governance & Risk  


[Dataset used for this analysis can be found here](<https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/ex1_fx_rates.csv>)


[Dataset 2](<https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/ex1_transactions.csv>)


[Interactive dashboard with full analysis and reporting](<https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/Multi_currency%20Analysis.xlsx>)

---

## Data Structure & Initial Checks

The analysis is based on **two core tables**, representing transactional activity and FX reference data. The sample dataset contains approximately **1,000 transaction records**.

### Table Descriptions

**transactions**
- transaction_id  
- customer_id  
- timestamp  
- amount  
- currency  
- status (Completed, Pending, Failed)  
- platform (Web, Mobile, API)

**fx_rates**
- date  
- from_currency  
- to_currency (USD)  
- rate


Transaction and FX rate tables were joined at the transaction-date and currency level using Excel XLOOKUP to enable expected settlement calculations.

<img width="314" height="381" alt="!relationship" src="https://github.com/user-attachments/assets/c7df023a-bf7f-4254-9295-9851f5ada7e3" />


### Initial Data Quality Checks
- No duplicate transaction IDs detected  
- No missing FX rates for transaction dates or currencies  
- No negative or invalid transaction amounts  
- Fully aligned and valid currency codes  

---


## Executive Summary

From a finance and operations leadership perspective, three key insights emerge. First, **core transaction and FX reference data quality is strong**, reducing the risk of systemic reconciliation errors. Second, **settlement accuracy cannot be fully validated due to missing settlement_amount data**, representing a material governance and control gap. Third, **a meaningful portion of transaction value remains tied up in failed and pending states**, introducing liquidity and operational risk.

*Dashboard snapshot: overall settlement value, status distribution, and completion trends*

<img width="622" height="398" alt="!multi currency" src="https://github.com/user-attachments/assets/f2d5b35a-782f-4040-b5b5-39b91e091939" />




## Insights Deep Dive


### Data Quality & Integrity

- Duplicate transaction checks returned zero duplicates, indicating strong ingestion and identifier controls.
- Insight 2:** FX rate coverage was complete across all transaction dates and currencies.
- No negative or zero-value transaction amounts were observed.
- Currency codes were fully consistent across transaction and FX reference tables.

*Visualization: Data quality validation summary*

---

### Settlement Accuracy & FX Validation

- The dataset does not include a `settlement_amount` field, preventing direct reconciliation testing.
- Transactions were joined to daily FX rates using Excel (XLOOKUP) to calculate expected USD settlement values (transaction_amount × FX_rate), providing a         concrete     baseline for reconciliation analysis.
- Without actual settlement values, ±0.5% deviation thresholds cannot be enforced.
- This creates a blind spot for financial reconciliation, audit assurance, and regulatory reporting.

*Visualization: Expected USD settlement by currency*

---

### Operational Performance & Completion Trends

-  Transaction volumes are broadly distributed across customers, reducing concentration risk.
-  Failed transactions represent a disproportionate share of settlement value, not just transaction count.
-  Pending settlements indicate temporary liquidity lock-up and delayed customer value delivery.
-  Completion performance varies over time, suggesting operational or external dependency effects.

*Visualization: Settlement status distribution and trend*

---

### Financial Controls, Governance & Risk

-  Missing settlement_amount logging represents a critical internal control weakness.
-  Current monitoring focuses on transaction outcomes rather than reconciliation accuracy.
-  FX-related discrepancies could remain undetected until downstream audits.
- Automated reconciliation controls would materially improve governance maturity.
 
*Visualization: Control gaps and risk exposure overview*

---

## Recommendations

Based on the findings, the **Finance, Risk, and Engineering teams** should consider the following actions:

- **Settlement data completeness:** Enforce mandatory capture of settlement_amount at transaction finalization.
- **Reconciliation controls:** Implement automated deviation checks with ±0.5% tolerance thresholds.
- **Operational monitoring:** Track failed and pending settlement *values*, not just transaction counts.
- **Governance maturity:** Introduce daily reconciliation dashboards and exception alerts.
- **Stakeholder communication:** Clearly communicate data gaps and estimated financial exposure when limitations exist.

---

## Assumptions and Caveats

Several assumptions were required due to data limitations:

- Expected settlement values were derived using transaction amount × FX rate.
- FX rates were assumed to be accurate and authoritative for each transaction date.
- The sample dataset is representative of typical platform activity.

These caveats should be resolved before using this analysis for financial reporting, audit sign-off, or regulatory submissions.


## Author
Rofiat Adebayo
Data Analyst | Business Intelligence | Analytics & Insights

Connect with me: (<https://www.linkedin.com/in/rofiat-adebayo/>)

Contact: 07066609682


                                                                                                                                                 
 [⬆ Back to top](#top)

