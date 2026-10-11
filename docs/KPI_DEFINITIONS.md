# CreditLens 360 — KPI Definitions

## 1. Purpose

This document defines the business meaning and calculation rules for portfolio analytics.

The formulas marked as current reflect the visible Python implementation. Other metrics must be implemented and reconciled before being presented as validated project outputs.

## 2. Assets Under Management (AUM)

**Current status:** Implemented in Python.

**Definition:** Total outstanding principal exposure for the selected portfolio.

\[
AUM = \sum_{i=1}^{N} OutstandingPrincipal_i
\]

**Reporting dimensions:** State, product, customer, and overall portfolio where supported.

**Validation:** Confirm that each loan contributes exactly once and that outstanding principal uses a consistent currency and reporting date.

## 3. Delinquency Rate

**Current status:** Implemented in Python as a DPD-based indicator.

A loan is classified as delinquent when its DPD is greater than zero.

\[
DelinquencyRate =
\frac{\text{Loans with DPD} > 0}
{\text{Loans with valid DPD}}
\times 100
\]

The current Python aggregation takes the mean of a binary delinquency indicator. If that indicator is stored as 0 and 1, the mean is a proportion; multiply by 100 for a percentage display.

**Limitation:** This is a snapshot indicator unless the dataset contains valid observation dates for historical reporting.

## 4. NPL Rate Proxy

**Current status:** Implemented as a DPD threshold indicator.

The current Python logic flags a loan when DPD is greater than or equal to 90.

\[
NPLRateProxy =
\frac{\text{Loans with DPD} \geq 90}
{\text{Loans with valid DPD}}
\times 100
\]

**Important limitation:** This is a project-defined DPD-based proxy. It must not be described as an officially classified non-performing loan ratio unless the relevant regulatory and accounting definition is applied and verified.

## 5. Average Interest Rate

**Current status:** Implemented in Python.

\[
AverageInterestRate =
\frac{\sum_{i=1}^{N} InterestRate_i}{N}
\]

The current implementation calculates the arithmetic mean at the portfolio grouping level.

**Validation:** Confirm whether rates are expressed as percentages or decimals and whether a simple or balance-weighted average is appropriate for the business question.

## 6. Collection Efficiency

**Current status:** Implemented as the mean of loan-level ratios in the visible Python logic.

For each eligible loan:

\[
LoanCollectionEfficiency_i =
\frac{AmountPaid_i}{AmountDue_i}
\]

The current code replaces zero amounts due with missing values before division and then takes the mean of the resulting loan-level ratios.

**Important distinction:** This is not necessarily the same as aggregate collection efficiency.

An alternative portfolio-level measure is:

\[
AggregateCollectionEfficiency =
\frac{\sum AmountPaid_i}{\sum AmountDue_i}
\times 100
\]

Use the aggregate formula only after defining the reporting period, eligibility rules, and handling of overpayments. Reconcile Python, SQL, and BI calculations before comparing their results.

## 7. Customer Priority Score

**Current status:** Implemented as a heuristic.

The current formula is:

\[
PriorityScore =
50\left(\frac{AUM}{Max(AUM)}\right)
+35\left(\frac{Clip(MaxDPD,0,120)}{120}\right)
+15\left(\frac{AverageRate}{25}\right)
\]

The implementation rounds the score to two decimal places.

### Components

- AUM contributes up to 50 points under the current normalization.
- Maximum DPD contributes up to 35 points after clipping to the 0–120 range.
- Average interest rate contributes 15 points when the rate is within the intended 0–25 range.

**Limitations:**

- The score is a heuristic, not a probability of default.
- The AUM component depends on the maximum AUM in the evaluated population.
- The interest-rate component assumes a meaningful upper reference value of 25; confirm the rate unit and range.
- The score weights are business rules, not statistically calibrated parameters.
- A high score does not establish that a customer will default.

## 8. Default Rate

**Status:** Not verified as implemented.

A true default rate requires a documented default event or label, a defined observation period, and an appropriate denominator.

Do not label the existing DPD-based NPL proxy as default rate without validating the event definition and implementation.

## 9. KPI Reconciliation Requirements

Before publishing results:

1. Use the same date range and population across tools.
2. Confirm the grain of each source table.
3. Check duplicate joins and missing records.
4. Standardize percentage scaling and currency units.
5. Reconcile Python and SQL values against a common test dataset.
6. Confirm Power BI and Tableau calculations against the approved definitions.

## 10. Change Control

Every KPI change must include its definition, formula, denominator, exclusions, reporting grain, implementation location, and validation evidence.