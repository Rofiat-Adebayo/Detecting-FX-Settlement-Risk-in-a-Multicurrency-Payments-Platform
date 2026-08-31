# Detecting FX Settlement Risk in a Multicurrency Payments Platform

### Clean transaction data. Complete FX rates. But could the business actually prove every payment was settled correctly?

**An analytical investigation into FX conversion accuracy, settlement integrity, operational exposure, and financial controls across 1,000+ multicurrency transactions.**

<br>

![EXCEL](https://img.shields.io/badge/EXCEL-444444?style=flat-square&logo=microsoftexcel&logoColor=white)
![DATA ANALYSIS](https://img.shields.io/badge/DATA%20ANALYSIS-666666?style=flat-square)
![FINTECH](https://img.shields.io/badge/FINTECH-555555?style=flat-square)
![FX ANALYSIS](https://img.shields.io/badge/FX%20ANALYSIS-777777?style=flat-square)
![RECONCILIATION](https://img.shields.io/badge/RECONCILIATION-444444?style=flat-square)
![FINANCIAL CONTROLS](https://img.shields.io/badge/FINANCIAL%20CONTROLS-666666?style=flat-square)
![RISK ANALYSIS](https://img.shields.io/badge/RISK%20ANALYSIS-555555?style=flat-square)

---

<img width="622" height="398" alt="!multi currency" src="https://github.com/user-attachments/assets/f2d5b35a-782f-4040-b5b5-39b91e091939" />


[View the interactive Excel analysis and reporting](https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/Multi_currency%20Analysis.xlsx)

---

## Table of Contents

- [Executive Summary](#executive-summary)
- [Business Context](#business-context)
- [Business Problem](#business-problem)
- [Analytical Questions](#analytical-questions)
- [Dataset & Analytical Foundation](#dataset--analytical-foundation)
- [Data Quality Investigation](#data-quality-investigation)
- [Analytical Approach](#analytical-approach)
- [Settlement Validation Logic](#settlement-validation-logic)
- [Key Findings](#key-findings)
  - [1. Transaction Data Was Not the Primary Risk](#1-transaction-data-was-not-the-primary-risk)
  - [2. Expected Settlement Could Be Calculated—but Actual Settlement Could Not Be Verified](#2-expected-settlement-could-be-calculatedbut-actual-settlement-could-not-be-verified)
  - [3. Failed and Pending Transactions Create Operational Exposure](#3-failed-and-pending-transactions-create-operational-exposure)
  - [4. The Larger Issue Is a Financial Control Gap](#4-the-larger-issue-is-a-financial-control-gap)
- [Dashboard & Decision Support](#dashboard--decision-support)
- [Business Implications](#business-implications)
- [Recommendations](#recommendations)
- [Potential Business Value](#potential-business-value)
- [Assumptions & Limitations](#assumptions--limitations)
- [Tools & Analytical Skills](#tools--analytical-skills)
- [Reflection](#reflection)
- [Author](#author)

---

## Executive Summary

At first glance, the payment data appeared reliable. Across approximately **1,000 multicurrency transactions**, validation checks found no duplicate transaction IDs, missing FX rates, invalid transaction amounts, or inconsistent currency codes.

But good source data did not mean settlement risk was under control.

The deeper investigation exposed a more important issue: **the platform could calculate what each transaction should have settled for in USD, but could not verify whether that amount was actually settled.**

Transaction records were matched to the corresponding daily FX rates to establish an **expected USD settlement value**. However, the transaction data contained no `settlement_amount` field representing the actual amount ultimately settled.

That missing field creates a significant control blind spot.

Without actual settlement values, the business cannot perform a complete:

**Expected Settlement → Actual Settlement → Variance → Exception**

reconciliation process.

It also prevents the proposed **±0.5% settlement deviation threshold** from being applied to identify potential over-settlement or under-settlement.

The analysis therefore shifted the problem from:

> **“Is the transaction data clean?”**

to the more commercially important question:

> **“Can the business prove that every transaction was settled for the correct amount?”**

The available data could not provide that assurance.

The investigation also found that failed transactions account for a disproportionate share of settlement value, while pending transactions represent value that has not yet completed the settlement process.

Together, these findings point to a broader requirement for stronger **settlement observability, automated reconciliation, exception monitoring, and financial controls**.

<img width="622" height="398" alt="!multi currency" src="https://github.com/user-attachments/assets/f2d5b35a-782f-4040-b5b5-39b91e091939" />


---

## Business Context

This project approaches the problem from the perspective of a **Data Analyst working within a multinational payments and settlement platform** operating across multiple currencies.

The platform enables businesses to process cross-border transactions through **web, mobile, and API channels**, with transactions converted using FX rates and ultimately settled in USD.

For this type of business, successful transaction processing is only part of the picture.

The organisation also needs confidence that:

- FX rates are correctly applied;
- expected settlement amounts can be independently calculated;
- actual settlements agree with those expectations;
- failed and pending transactions are visible by financial value;
- discrepancies can be identified before they become reconciliation, customer, audit, or regulatory issues.

This makes **settlement integrity** a financial control problem as much as a transaction-processing problem.

---

## Business Problem

A multicurrency payment can appear operationally successful while still carrying financial risk if the business cannot verify the amount ultimately settled.

The investigation therefore focused on a fundamental control question:

> **Did we settle the correct amount for every transaction, at the correct FX rate?**

The existing data presented several challenges:

- **No end-to-end settlement validation** because `settlement_amount` was unavailable.
- **No ability to compare expected and actual settlement amounts.**
- **No ability to operationalise a ±0.5% FX deviation threshold.**
- **Failed and pending transactions creating potential liquidity and operational exposure.**
- **Reconciliation depending on downstream/manual review rather than an automated exception-control process.**

The objective was therefore not simply to summarise transactions.

It was to determine **how much settlement assurance the available data could actually provide—and where the control framework stopped.**

---

## Analytical Questions

I broke the investigation into questions that move from **data reliability → settlement accuracy → operational exposure → financial control**:

1. **Data Integrity:** Can the underlying transaction and FX datasets be trusted enough to support reconciliation?

2. **FX Coverage:** Does every transaction have the appropriate FX rate for its currency and transaction date?

3. **Expected Settlement:** What should each transaction settle for in USD based on the available FX rate?

4. **Settlement Accuracy:** Can expected settlement be compared with the amount actually settled?

5. **Exception Detection:** Can transactions outside a ±0.5% tolerance be identified?

6. **Operational Exposure:** How much transaction value remains in failed or pending states?

7. **Control Effectiveness:** What information is missing that prevents Finance and Risk teams from independently validating settlement integrity?

This structure prevented the analysis from treating a clean dataset as proof of a well-controlled settlement process.

---

## Dataset & Analytical Foundation

The analysis uses two related datasets containing approximately **1,000 transaction records**.

### Transactions

The transaction data contains:

- `transaction_id`
- `customer_id`
- `timestamp`
- `amount`
- `currency`
- `status` — Completed, Pending, Failed
- `platform` — Web, Mobile, API

### FX Rates

The FX reference data contains:

- `date`
- `from_currency`
- `to_currency` — USD
- `rate`

The datasets were connected at the **transaction-date and currency level**, allowing each transaction to be evaluated using the appropriate daily FX rate.

### Dataset

- [FX Rates Dataset](https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/ex1_fx_rates.csv)
- [Transactions Dataset](https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/ex1_transactions.csv)

### Data Relationship

<img width="314" height="381" alt="Transaction and FX Rate Relationship" src="https://github.com/user-attachments/assets/c7df023a-bf7f-4254-9295-9851f5ada7e3" />

---

## Data Quality Investigation

Before investigating settlement risk, I first needed to establish whether apparent discrepancies could simply be explained by poor source data.

The validation checks found:

| Validation | Result | Why It Mattered |
|---|---|---|
| Duplicate transaction IDs | None detected | Reduced the risk of double-counting settlement exposure |
| FX-rate coverage | Complete | Allowed transactions to be matched to applicable reference rates |
| Transaction amounts | No negative or invalid values | Supported reliable value-based analysis |
| Currency codes | Fully aligned | Reduced the risk of failed or incorrect FX matching |

These results changed the direction of the investigation.

The obvious data-quality problems were **not** where the principal risk appeared to sit.

Instead, the analysis could move beyond data cleanliness and investigate whether the available information was sufficient to validate the settlement process itself.

---

## Analytical Approach

With the source data validated, the analysis progressed through four stages.

### 1. Establish Data Reliability

Transaction identifiers, amounts, currencies, and FX coverage were checked first so that downstream calculations would not be built on obvious data-quality defects.

This established whether the source data was sufficiently reliable to support settlement analysis.

### 2. Establish an Expected Settlement Baseline

Transactions were matched to the appropriate FX reference data using **Excel XLOOKUP** at the transaction-date and currency level.

Expected USD settlement was then derived as:

```text
Expected USD Settlement = Transaction Amount × FX Rate
```

This provided an independent expectation of what each transaction should settle for.

### 3. Test Whether Settlement Accuracy Could Be Measured

The next logical control was to compare:

```text
Expected Settlement
        ↓
Actual Settlement
        ↓
Settlement Variance
        ↓
±0.5% Tolerance Check
        ↓
Exception / No Exception
```

This is where the investigation exposed the central limitation:

**Actual `settlement_amount` data was unavailable.**

### 4. Assess the Remaining Operational and Governance Exposure

Because complete reconciliation was not possible, the analysis examined transaction status, settlement value, completion patterns, and the control implications of the missing settlement information.

This allowed the analysis to distinguish between what could be measured confidently and what remained an unresolved control risk.

---

## Settlement Validation Logic

The analytical design was intended to move beyond transaction monitoring into **reconciliation monitoring**.

For each transaction, the desired control framework would calculate:

```text
Expected Settlement = Transaction Amount × FX Rate

Settlement Variance = Actual Settlement − Expected Settlement

Deviation % = Settlement Variance / Expected Settlement
```

Transactions exceeding the proposed **±0.5% tolerance** could then be surfaced for investigation.

However, because `settlement_amount` was absent, only the **expected settlement baseline** could be calculated.

This distinction is important:

> **The analysis did not identify actual FX settlement discrepancies. It identified that the available data prevents the business from reliably detecting them.**

That limitation became one of the most important findings of the investigation.

---

## Key Findings

### 1. Transaction Data Was Not the Primary Risk

The first stage of the investigation produced a positive result.

No duplicate transaction IDs were detected, FX coverage was complete, transaction amounts were valid, and currencies were consistently represented.

That provided a sufficiently reliable foundation for calculating expected settlement values.

More importantly, it meant the investigation needed to look beyond conventional data-quality issues.

---

### 2. Expected Settlement Could Be Calculated—but Actual Settlement Could Not Be Verified

Matching transactions to daily FX rates established the expected USD settlement for each transaction.

But the absence of `settlement_amount` broke the reconciliation chain.

The business could answer:

> **“What should this transaction have settled for?”**

but not:

> **“Is that what we actually paid?”**

Without that comparison, neither settlement variance nor ±0.5% exceptions can be reliably calculated.

This was the most important risk identified in the analysis.

---

### 3. Failed and Pending Transactions Create Operational Exposure

Transaction status analysis added another dimension to the investigation.

Failed transactions represented a disproportionate share of settlement value rather than simply transaction count, while pending transactions represented value still awaiting completion.

Completion performance also varied over time.

This suggests that operational monitoring should not rely solely on the **number** of failed or pending transactions.

The **financial value associated with those states** is equally important when prioritising operational investigation.

---

### 4. The Larger Issue Is a Financial Control Gap

The missing settlement field is more than a reporting limitation.

Current monitoring can describe transaction outcomes, but the available data cannot independently confirm whether the amount settled agrees with the expected FX-derived value.

This creates the possibility that FX discrepancies could remain unidentified until later reconciliation or audit processes.

The analytical problem therefore becomes one of **control design and data governance**, not simply dashboard reporting.

---

## Dashboard & Decision Support

The reporting layer brings the investigation together by presenting:

- overall expected settlement value;
- transaction status distribution;
- completion trends;
- FX-derived settlement values;
- operational exposure across failed and pending transactions;
- the identified reconciliation-control limitation.

### Interactive Analysis

[View the interactive Excel analysis and reporting](https://github.com/Rofiat-Adebayo/Detecting-FX-Settlement-Risk-in-a-Multicurrency-Payments-Platform/blob/main/Multi_currency%20Analysis.xlsx)

### Dashboard Preview

<img width="622" height="398" alt="Multicurrency Settlement Analysis" src="https://github.com/user-attachments/assets/f2d5b35a-782f-4040-b5b5-39b91e091939" />

The dashboard should therefore be interpreted as a **decision-support and risk-monitoring layer**, rather than proof that settlement accuracy has been fully validated.

---

## Business Implications

The findings have implications across several business functions.

### Finance

Finance needs actual settlement values to independently reconcile **expected versus realised settlement amounts**.

Without that information, the organisation cannot fully quantify settlement variances or confirm whether every transaction settled at the expected value.

### Risk & Compliance

Risk and Compliance teams need traceable settlement information to support **exception monitoring, auditability, and control assurance**.

A missing settlement value limits the evidence available for demonstrating that FX conversion and settlement controls are operating as intended.

### Operations

Operations teams need visibility into the **value**, not simply the volume, of failed and pending transactions.

A small number of high-value exceptions may represent greater operational exposure than a large number of low-value transactions.

### Engineering

Settlement data capture needs to become part of the transaction lifecycle rather than an unavailable downstream attribute.

Capturing the actual settlement amount would allow the organisation to move from transaction-status monitoring to transaction-level financial reconciliation.

### Overall Business Implication

The broader lesson is that:

> **Clean transactional data does not automatically mean strong financial controls.**

A platform can have accurate transaction IDs, valid currencies, and complete FX reference data while still lacking the information required to prove settlement accuracy.

---

## Recommendations

Based on the investigation, the **Finance, Risk, Operations, and Engineering teams** should consider the following actions:

### 1. Capture Actual Settlement Amounts

Make `settlement_amount` a mandatory field when settlement is finalised so expected and actual values can be independently reconciled.

### 2. Automate Expected-vs-Actual Reconciliation

Compare the calculated expected USD settlement against the actual settlement amount at transaction level.

This would establish a repeatable reconciliation control rather than relying solely on downstream manual review.

### 3. Introduce Tolerance-Based Exception Monitoring

Apply the proposed **±0.5% deviation threshold** and route transactions outside tolerance for investigation.

This would allow teams to focus attention on exceptions rather than manually reviewing every transaction.

### 4. Prioritise Exposure by Value

Monitor the financial value associated with failed and pending transactions alongside transaction counts.

This would help operational teams prioritise exceptions according to potential financial exposure.

### 5. Introduce Daily Reconciliation Monitoring

Provide Finance and Risk teams with visibility into:

- settlement variances;
- unresolved exceptions;
- failed transaction values;
- pending settlement exposure;
- tolerance breaches.

### 6. Strengthen Settlement Traceability

Preserve the following information as part of the settlement audit trail:

```text
Transaction Amount
        ↓
FX Rate Applied
        ↓
Expected Settlement
        ↓
Actual Settlement
        ↓
Settlement Variance
        ↓
Exception Status
```

These recommendations represent **proposed controls based on the analysis** and are not presented as implemented business outcomes.

---

## Potential Business Value

Because actual settlement data was unavailable, this project does **not** claim realised financial savings or reduced settlement losses.

Instead, the analysis demonstrates how stronger settlement data and reconciliation controls could enable the business to:

- identify potential over- or under-settlement earlier;
- reduce reliance on reactive manual reconciliation;
- prioritise failed and pending transactions according to financial exposure;
- strengthen settlement traceability;
- improve Finance and Risk exception monitoring;
- provide stronger evidence for audit and regulatory review;
- support faster investigation of settlement exceptions.

The measurable financial value of these improvements should only be quantified after actual settlement data and exception outcomes become available.

---

## Assumptions & Limitations

The analysis is subject to several important limitations:

- Expected settlement was calculated as `transaction amount × FX rate`.
- FX reference rates were assumed to be accurate and authoritative for the relevant transaction date.
- The sample dataset was assumed to represent typical platform activity.
- Actual `settlement_amount` values were unavailable.
- Therefore, actual settlement variance and ±0.5% deviation breaches could **not** be measured.

These limitations should be resolved before the analysis is used for **financial reporting, audit sign-off, or regulatory submissions**.

---

## Tools & Analytical Skills

| Tool / Skill | Application |
|---|---|
| **Excel** | XLOOKUP, calculated fields, analytical reporting, and settlement calculations |
| **Data Quality Analysis** | Duplicate, transaction amount, currency, and FX coverage validation |
| **FX Analysis** | Daily FX-rate matching and expected USD settlement calculations |
| **Reconciliation Logic** | Expected-versus-actual control design and tolerance framework |
| **Financial Controls Analysis** | Identification of settlement-data, monitoring, and governance gaps |
| **Business Intelligence** | Translating transaction-level analysis into decision-support reporting |

The tools supported the investigation, but the primary focus of the project was determining **what could be trusted, what could be measured, what could not be validated, and what that meant for the business.**

---

## Reflection

This project reinforced an important part of how I approach analytics:

> **A clean dataset is not necessarily the same as a controlled business process.**

The initial checks suggested that the underlying transaction and FX data were reliable.

Rather than stopping there, I followed the business process further and asked whether the available data could actually answer the question that mattered most:

> **Was every transaction settled correctly?**

That investigation exposed the missing link between expected and actual settlement.

For me, that is the strongest part of this project.

It demonstrates an analytical approach that moves beyond reporting what is available in the data to identifying **what is missing, why that gap matters, what risk it creates, and what additional control would allow the business to make a better decision.**

---

## Author

**Rofiat Adebayo**  
Data Analyst | Business Intelligence | Analytics & Insights

[Connect with me on LinkedIn](https://www.linkedin.com/in/rofiat-adebayo/)

---

[⬆ Back to top](#detecting-fx-settlement-risk-in-a-multicurrency-payments-platform)
