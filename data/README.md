# CreditLens 360 — Data Documentation

## 1. Purpose

This directory contains the datasets used by CreditLens 360 for lending portfolio analytics, customer prioritization, and related reporting.

## 2. Directory Structure

```text
data/
├── raw/
│   ├── customers.csv
│   ├── loans.csv
│   ├── repayments.csv
│   ├── transactions.csv
│   └── insurance_policies.csv
└── processed/
    ├── portfolio_by_state_product.csv
    └── customer_priority.csv
```

The filenames above describe the expected logical datasets. Verify their actual extensions and generated outputs before relying on this listing.

## 3. Raw Datasets

| Dataset | Purpose |
|---|---|
| Customers | Customer-level attributes and identifiers |
| Loans | Loan details, balances, interest rates, and DPD |
| Repayments | Amounts due and paid |
| Transactions | Transaction-related analytical data |
| Insurance policies | Policy status and premium measures |

The authoritative source, license, generation method, and permitted usage of each dataset must be documented after verification.

## 4. Processed Datasets

### `portfolio_by_state_product.csv`

Contains aggregated portfolio metrics grouped by state and product, including loan counts, outstanding principal, original principal, delinquency indicators, NPL proxy, average rate, and collection-efficiency measure.

### `customer_priority.csv`

Contains customer-level aggregates and the heuristic priority score used to order customers for analytical review.

The score is not a calibrated probability of default.

## 5. Data Processing

The visible Python analysis reads customer, loan, repayment, and insurance-policy data, performs joins and aggregations, and writes processed portfolio and customer-priority outputs.

The exact dependency order, file formats, and refresh procedure must be confirmed against `python/analysis.py` and `python/generate_data.py`.

## 6. Data Quality

Before processing, verify:

- Required columns and data types.
- Unique customer and loan identifiers at the appropriate grain.
- Referential integrity between loans, customers, and repayments.
- Null values and invalid amounts.
- Valid date formats and reporting periods.
- Consistent currency and interest-rate units.
- Reconciliation of processed outputs against their source data.

## 7. Privacy and Security

- Do not publish confidential customer or financial records.
- Do not commit credentials, tokens, or secrets.
- Use synthetic or properly anonymized datasets for public demonstrations.
- Confirm licensing and redistribution permissions before publishing datasets.
- Document whether the datasets are synthetic, anonymized, or externally sourced.

## 8. Reproducibility

Document the command used to generate each dataset, its random seed where applicable, its expected output path, and its dependencies.

Do not overwrite source data unless the workflow explicitly requires it.

## 9. Known Limitations

Dataset provenance, exact source schemas, file formats, and historical observation coverage must be verified before this documentation is treated as final.

Update this document whenever datasets or transformation logic change.