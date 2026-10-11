# CreditLens 360 — System Architecture

## 1. Overview

CreditLens 360 is a BFSI analytics project focused on lending portfolio monitoring, customer prioritization, credit-risk indicators, collections analytics, and business intelligence.

The project combines Python-based data processing, SQL analytics, and BI reporting assets.

## 2. High-Level Workflow

```text
Raw BFSI Datasets
       |
       v
Data Loading and Preparation
       |
       v
Python Analytics and Transformations
       |
       +---------------------------+
       |                           |
       v                           v
Portfolio Aggregation       Customer Prioritization
       |                           |
       +-------------+-------------+
                     |
                     v
             Processed Outputs
                     |
                     v
           SQL Analytics Layer
                     |
                     v
       Power BI / Tableau / Excel
                     |
                     v
       Business Insights and Reporting
```

This diagram represents the intended analytical workflow. Individual connections must be verified against the actual scripts and dashboard implementations.

## 3. Repository Components

### `data/`

Contains raw and processed datasets.

- `raw/`: Source datasets used by the project.
- `processed/`: Analytical outputs generated during processing.
- Dataset provenance, file formats, and privacy restrictions should be documented.

### `python/`

Contains Python analytics scripts.

- `generate_data.py`: Dataset generation logic.
- `analysis.py`: Portfolio aggregation and customer-priority calculations.
- `utils.py`: Reserved for shared utilities; verify implementation before relying on it.

### `sql/`

Contains SQL analytics scripts, including BigQuery-related queries.

The SQL layer supports analytical transformations and KPI queries. Confirm the execution order and dependencies from the individual SQL files.

### `app/`

Contains the application entry point and related application code.

Document the application's actual framework, input files, and execution instructions after inspecting the application source.

### BI and Reporting

- `powerbi/`: Power BI assets and documentation.
- `tableau/`: Tableau assets and documentation.
- `excel/`: Excel-based analysis and reporting assets.

### `tests/`

Contains automated tests. Test coverage and validation scope should be determined from the test implementations.

## 4. Customer Priority Score

The current Python analysis calculates a heuristic priority score using:

- Assets under management (AUM), represented by the project’s customer-level exposure measure.
- Maximum days past due (DPD).
- Average interest rate.

The score uses a weighted formula with a maximum contribution of 50 points from AUM, 35 points from DPD, and 15 points from the average interest rate component, subject to the current formula's normalization assumptions.

This score is a prioritization heuristic. It is not a calibrated probability of default or a validated credit-risk model.

## 5. Data and Calculation Controls

The following controls should be implemented and verified:

- Required-column validation.
- Primary-key and duplicate checks.
- Null and invalid-value handling.
- Referential-integrity checks across related datasets.
- Reconciliation of equivalent KPIs across Python, SQL, and BI reports.
- Reproducible execution and documented dependencies.

## 6. Security and Privacy

- Do not commit credentials, API keys, or passwords.
- Verify that public datasets contain no confidential customer information.
- Document the source and permitted usage of each dataset.
- Use synthetic or properly anonymized data for public demonstrations.

## 7. Known Limitations

- The exact data-generation process and dataset provenance must be documented from the source code.
- The priority score should not be interpreted as default probability.
- Dashboard-to-SQL and Python-to-SQL calculation consistency must be tested.
- Production readiness, model validation, and automated deployment must not be claimed unless implemented and verified.

## 8. Maintenance

Update this document whenever the pipeline, source datasets, processing logic, or reporting architecture changes.