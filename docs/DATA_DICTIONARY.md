# CreditLens 360 — Data Dictionary

## 1. Purpose

This document describes the principal entities and fields used in CreditLens 360.

The data types and definitions below are provisional where the source schema has not yet been independently verified.

## 2. Customers Dataset

| Field | Expected Type | Business Meaning |
|---|---|---|
| `customer_id` | String | Customer identifier |
| `state` | String | Customer or portfolio state; confirm the source definition |

Additional customer attributes must be documented from the actual source file.

**Key expectation:** `customer_id` should uniquely identify a customer if the source dataset follows a one-row-per-customer design.

## 3. Loans Dataset

| Field | Expected Type | Business Meaning |
|---|---|---|
| `loan_id` | String | Loan identifier |
| `customer_id` | String | Associated customer identifier |
| `disbursement_date` | Date | Loan disbursement date |
| `dpd` | Integer | Days past due |
| `original_principal` | Numeric | Original principal amount |
| `outstanding_principal` | Numeric | Remaining principal exposure |
| `interest_rate` | Numeric | Loan interest rate; confirm whether stored as a percentage |
| `state` | String | Geographic classification used for portfolio reporting |
| `product` | String | Loan or financial product classification |

**Expected keys:** `loan_id` should be unique if one row represents one loan. `customer_id` should reference a valid customer.

## 4. Repayments Dataset

| Field | Expected Type | Business Meaning |
|---|---|---|
| `loan_id` | String | Associated loan identifier |
| `amount_due` | Numeric | Amount due for the relevant repayment record |
| `amount_paid` | Numeric | Amount paid for the relevant repayment record |

The source schema must establish whether the dataset contains one record per loan, installment, or payment event.

For installment-level data, aggregate paid and due amounts at the appropriate reporting grain before calculating portfolio collection efficiency.

## 5. Insurance Policies Dataset

| Field | Expected Type | Business Meaning |
|---|---|---|
| `customer_id` | String | Associated customer identifier |
| `policy_active` | Boolean | Whether a policy is active |
| `annual_premium` | Numeric | Annual premium amount |

Confirm whether multiple policies can exist for a customer and whether `annual_premium` is stored at policy or customer level.

## 6. Transactions Dataset

The raw folder contains a transactions dataset. Its exact schema has not yet been verified.

Document each transaction field, data type, primary key, transaction timestamp, amount definition, and relationship to customers or loans after inspecting the source file.

## 7. Processed Portfolio Dataset

Expected output: `portfolio_by_state_product.csv`

| Field | Expected Type | Business Meaning |
|---|---|---|
| `state` | String | Portfolio grouping dimension |
| `product` | String | Product grouping dimension |
| `loans` | Integer | Number of loans in the group |
| `aum` | Numeric | Sum of outstanding principal |
| `original_amount` | Numeric | Sum of original principal |
| `delinquency_rate` | Numeric | Mean of the delinquent indicator |
| `npl_rate` | Numeric | Mean of the project's NPL indicator |
| `avg_rate` | Numeric | Mean interest rate |
| `collection_efficiency` | Numeric | Mean of the current loan-level collection-efficiency calculation |

These names reflect the Python aggregation logic. Confirm the actual exported column names and downstream SQL mappings.

## 8. Processed Customer Priority Dataset

Expected output: `customer_priority.csv`

| Field | Expected Type | Business Meaning |
|---|---|---|
| `customer_id` | String | Customer identifier |
| `active_loans` | Integer | Number of loans associated with the customer |
| `aum` | Numeric | Aggregated outstanding principal exposure |
| `avg_rate` | Numeric | Average loan interest rate |
| `max_dpd` | Integer | Maximum observed DPD in the customer's loan records |
| `policy_active` | Boolean | Policy activity indicator |
| `annual_premium` | Numeric | Premium measure used by the analysis |
| `cross_sell_eligible` | Integer / Boolean | Rule-based cross-sell eligibility indicator |
| `priority_score` | Numeric | Weighted heuristic prioritization score |

## 9. Data Quality Rules

Implement and verify the following:

1. Required identifiers must not be null.
2. Loan identifiers should be unique in the loan-level table.
3. Repayment records must reference valid loans.
4. Numeric amounts must follow the source's documented sign conventions.
5. DPD must be non-negative unless the source explicitly defines otherwise.
6. Boolean values must use a consistent representation.
7. Missing values must be handled explicitly rather than silently converted to zero.
8. Aggregations must preserve the correct grain and avoid double-counting.

## 10. Schema Maintenance

Update this document when source columns, data types, relationships, or transformation logic change.