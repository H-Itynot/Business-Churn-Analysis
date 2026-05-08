# 🏦 Savanna Bank Kenya — Customer Churn Analysis

A full end-to-end data analytics project investigating customer churn for a mid-tier Kenyan commercial bank. Covers data cleaning, exploratory analysis, feature engineering, churn prediction modelling, and actionable business recommendations.

---

## Project Structure

```
business-data-project/
│
├── .data/
│   ├── raw/
│   │   └── bank_customer_churn_dirty.csv       # Original raw dataset (5,020 rows)
│   └── cleaned/
│       └── bank_customer_churn_clean.csv        # Cleaned & feature-engineered dataset (5,000 rows)
│
├── notebooks/
│   ├── 01_data_quality_and_cleaning.ipynb
│   ├── 02_exploratory_data_analysis.ipynb
│   ├── 03_feature_engineering.ipynb
│   └── 04_churn_modeling_and_recommendations.ipynb
│
├── outputs/
│   └── visuals/                                 # 14 saved figures + Power BI dashboard
│
├── README.md
└── requirements.txt
```

---

## Business Context

**Savanna Bank Kenya** operates across 7 regions with over 120,000 customers. The Retail Banking division has seen accelerating churn, with customer acquisition costing KSH 8,400 per head. This project identifies who is leaving, why, and what the bank can do about it.

---

## Data Quality Issues Found & Fixed

| # | Issue | Fix |
|---|-------|-----|
| 1 | Inconsistent `gender` encoding (M, male, Male) | Standardised to Male/Female |
| 2 | Whitespace in `region` values | `.strip()` applied |
| 3 | Sentinel value 999 in `credit_score` | Replaced with median |
| 4 | Null values in `credit_score` | Imputed with median |
| 5 | Negative `account_balance_ksh` | Clipped to 0 |
| 6 | Outliers in `monthly_income_ksh` | Capped at 99th percentile |
| 7 | Future `last_transaction_date` values | Replaced with median date |
| 8 | Missing `marital_status` values | Imputed with mode |
| 9 | Missing `employment_status` values | Imputed with mode |
| 10 | Duplicate `customer_id` rows (20) | Dropped, keeping first occurrence |

---

## Engineered Features

| Feature | Formula | Rationale |
|---------|---------|-----------|
| `days_since_last_txn` | today − last_transaction_date | Direct inactivity signal |
| `account_age_months` | months since account opening | Tenure loyalty proxy |
| `has_loan` | 1 if loan_type ≠ "No Loan" | Product tie-in flag |
| `loan_to_income_ratio` | loan_amount / monthly_income | Debt burden relative to income |
| `repayment_burden` | monthly_repayment / monthly_income | Monthly cash pressure |
| `balance_to_income_ratio` | balance / monthly_income | Savings buffer signal |

---

## Key Findings

- **Overall churn rate:** 13.4% (668 of 5,000 customers)
- **Highest-risk regions:** Nyeri (15.7%) and Eldoret (15.3%)
- **Income gap:** Churners earn ~52% less per month than active customers
- **Repayment burden:** Churners carry a burden of 0.74 vs 0.41 for active customers
- **Top 5 predictors:** monthly income, account balance, credit score, transaction frequency, repayment burden

---

## Model Performance

| Model | Accuracy | Recall (Churn) | AUC-ROC | CV AUC |
|-------|----------|----------------|---------|--------|
| Logistic Regression | 54% | 51% | 0.532 | 0.527 |
| **Decision Tree (depth=3)** | **72%** | **43%** | **0.593** | **0.608** |

The Decision Tree is the recommended model — interpretable, deployable without ML infrastructure, and best used as a **risk ranking tool** to surface the top 20% most at-risk customers for proactive outreach.

---

## Business Recommendations

1. **Repayment Burden Relief Programme** — Offer loan restructuring to customers where `repayment_burden > 0.6`. ~420 customers currently in this band.
2. **Income-Segmented Retention Offers** — Launch tiered "Hifadhi" savings incentives for customers earning below KSH 50,000/month.
3. **Regional Campaigns in Nyeri & Eldoret** — Deploy targeted branch-level retention campaigns; investigate local competitive activity.

**Estimated saving from 20% churn reduction:** KSH 1,117,200 (133 customers × KSH 8,400 acquisition cost).

---

## Getting Started

```bash
pip install -r requirements.txt
jupyter lab
# Run notebooks in order: 01 → 02 → 03 → 04
```
