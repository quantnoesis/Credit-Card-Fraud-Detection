# Credit Card Fraud Detection — Exploratory Data Analysis

## Overview
This project explores the [Credit Card Fraud Detection dataset](https://www.kaggle.com/mlg-ulb/creditcardfraud) from Kaggle, which contains 284,807 anonymized European credit card transactions made over two days in September 2013. Only 492 of these transactions (0.17%) are fraudulent, making this a highly imbalanced, real-world dataset.

The goal isn't to build a fraud-detection model — it's to explore the data through a finance and risk-analysis lens: understanding *how rare* fraud is, *what patterns* separate fraudulent transactions from normal ones, and *what this means* for accounting, audit, and risk teams.

This is part of an ongoing portfolio of data analytics projects combining a finance/accounting background with hands-on Python skills.

## Dataset
- **Source:** [Kaggle — mlg-ulb/creditcardfraud](https://www.kaggle.com/mlg-ulb/creditcardfraud)
- **Size:** 284,807 rows, 31 columns
- **Key columns:**
  - `Time` — seconds elapsed since the first transaction in the dataset
  - `Amount` — transaction amount
  - `V1`–`V28` — anonymized features (result of a PCA transformation, original details withheld for confidentiality)
  - `Class` — target label (0 = normal, 1 = fraud)
- **Note:** The raw CSV is not included in this repo due to size and licensing. Download it directly from the Kaggle link above and place it in the project root as `creditcard.csv`.

## Tools & Environment
- **Language:** Python 3
- **Environment:** Google Colab
- **Libraries:** pandas, matplotlib

## Project Structure
```
credit-card-fraud-eda/
├── README.md
├── credit_card_fraud_eda.ipynb   # Main Colab notebook
├── charts/
│   ├── fraud_vs_normal_count.png
│   ├── fraud_by_amount.png
│   └── fraud_by_hour.png
└── .gitignore                    # excludes creditcard.csv
```

## Analysis Steps
1. **Data loading & inspection** — checked structure, types, and null values with `df.info()` and `df.describe()`.
2. **Class imbalance check** — quantified how rare fraud is (~0.17% of all transactions).
3. **Fraud vs. normal transaction counts** — bar chart comparing class frequencies.
4. **Transaction amount distribution** — histogram comparing fraud vs. normal amounts (log scale, since fraud counts are tiny).
5. **Time-of-day pattern (raw count)** — converted `Time` into hour-of-day and checked whether fraud clusters at certain hours.
6. **Time-of-day pattern (normalized rate)** — divided fraud count by total transaction volume per hour, since raw counts conflate risk with volume. This is what confirmed hour 2 as a genuine high-risk window rather than just a high-traffic one.
7. **Correlation analysis** — identified which anonymized features (`V1`–`V28`) correlate most strongly with the fraud label.

## Key Findings
- **Class imbalance:** Fraud accounted for just **0.173%** of all transactions (492 fraud vs. 284,315 normal, out of 284,807 total) — a textbook needle-in-a-haystack problem.
- **Amount pattern:** Fraudulent transactions cluster tightly at low dollar amounts and essentially disappear above a few thousand dollars, while normal transactions spread across the full range up to $25,000+. Fraud in this dataset favors small, easy-to-miss amounts rather than big-ticket theft.
- **Time-of-day pattern:** Raw fraud counts looked highest around hours 2 and 11, but that can just reflect transaction volume — so we normalized by computing the fraud **rate** per hour instead. That correction changed the story: **hour 2 has a fraud rate of 1.71%**, roughly **10x the overall average** of 0.173%, confirming it as a genuinely high-risk window rather than a volume artifact. **Hour 4** is also elevated at **1.04%** (~6x average). Hour 11, despite its high raw count, drops to a much more modest **0.31% rate** once normalized — still slightly above average, but far less dramatic than the count chart suggested. The safest hours are **hour 10** (0.048%) and **hour 22** (0.058%), both well below average.
- **Correlation with fraud:** The anonymized features most positively correlated with the `Class` label are **V11** (0.155), **V4** (0.133), and **V2** (0.091). The strongest negative correlations are **V17** (−0.326), **V14** (−0.303), and **V12** (−0.261) — meaning low values on these three features are the strongest signal of fraud in this dataset.

## Why This Matters (Finance/Accounting Angle)
Even without knowing what the anonymized `V1`–`V28` features represent, the patterns in this data mirror real audit and risk-management concerns: rare-event detection, transaction-amount profiling, and rate-normalized time-based anomaly monitoring are all techniques used in fraud audits and internal controls testing. The hour-2 finding is a good illustration of a common analyst pitfall: a raw count spike can be misleading if it isn't checked against volume — the same caution that applies when interpreting a ratio like ROE without checking the denominator behind it.

## How to Reproduce
1. Download `creditcard.csv` from the [Kaggle dataset page](https://www.kaggle.com/mlg-ulb/creditcardfraud).
2. Open `credit_card_fraud_eda.ipynb` in Google Colab.
3. Upload `creditcard.csv` when prompted (or mount Google Drive).
4. Run all cells top to bottom.

## Related Write-Up
- 📝 Medium: *(link once published)*
- 📝 Dev.to: *(link once published)*

## Author
Neha — building a data analytics portfolio at the intersection of finance/accounting and Python.
