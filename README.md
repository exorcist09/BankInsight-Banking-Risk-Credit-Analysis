# BankInsight — Banking Risk & Creditworthiness Analysis

**BankInsight** is a data-driven risk assessment system designed to analyze customer profiles and lending behaviours to evaluate creditworthiness and support safer lending decisions for financial institutions. The project performs exploratory data analysis (EDA), feature engineering, predictive-ready preparation, and dashboard-ready KPI calculations (Power BI). The deliverable is a reproducible Python notebook + Power BI-ready measures to help risk managers and business stakeholders.

---

## Table of Contents

* [Overview](#overview)
* [Goals & Problem Statement](#goals--problem-statement)
* [Dataset](#dataset)
* [Project Structure](#project-structure)
* [Tools & Technologies](#tools--technologies)
* [Key Steps & Methodology](#key-steps--methodology)

  * [Data Loading & Summary](#data-loading--summary)
  * [Data Cleaning & Feature Engineering](#data-cleaning--feature-engineering)
  * [Univariate & Bivariate Analysis](#univariate--bivariate-analysis)
  * [Correlation Analysis](#correlation-analysis)
  * [Power BI Measures & KPIs](#power-bi-measures--kpis)
* [Example Code Snippets](#example-code-snippets)
* [How to run](#how-to-run)
* [Notebook & Files](#notebook--files)
* [Insights & Business Impact](#insights--business-impact)
* [Future Work](#future-work)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)

---

## Overview

This project focuses on building a foundational risk analytics workflow for banks:

* Explore customer demographics and banking behaviour
* Create features relevant to credit risk (income band, engagement length, fees)
* Identify patterns and correlations that indicate default risk
* Produce Power BI-ready KPI measures for management and risk teams

The analyses are intentionally modular so they can feed into scoring models (logistic regression, tree-based models) later.

---

## Goals & Problem Statement

Design and develop a comprehensive risk assessment system to analyze customer profiles and lending behaviours to evaluate creditworthiness. The system should:

* Detect customers who are likely to default
* Quantify risk using interpretable KPIs
* Provide visual dashboards for informed lending decisions
* Support continuous monitoring via derived features and measures

---

## Dataset

The dataset contains client banking information across several interlinked tables (or a single denormalized table depending on your export):

* `Clients - Banking` (main transactional/profile table)
* `Banking Relationship`
* `Client-Banking`
* `Gender` (lookup)
* `Investment Advisor` (lookup)
* `Period` (time dimensions)

Common fields include: `Client ID`, `Estimated Income`, `Age`, `Nationality`, `Occupation`, `Bank Deposits`, `Checking Accounts`, `Saving Accounts`, `Credit Card Balance`, `Bank Loans`, `Business Lending`, `Joined Bank`, `Fee Structure`, `Properties Owned`, `Risk Weighting`.

---

## Project Structure (suggested)

```
BankInsight/
├─ data/
│  ├─ bank_clients.csv
│  └─ README_dataset.md
├─ notebooks/
│  ├─ 01_data_loading_and_eda.ipynb
│  └─ 02_feature_engineering_and_kpis.ipynb
├─ src/
│  ├─ features.py
│  └─ viz.py
├─ powerbi/
│  └─ BankInsight_PowerBI.pbix  # placeholder
├─ requirements.txt
└─ README.md
```

---

## Tools & Technologies

* Python 3.8+
* pandas, numpy
* matplotlib, seaborn
* Jupyter Notebook / VSCode
* Power BI Desktop (for dashboard & DAX measures)

---

## Key Steps & Methodology

### Data Loading & Summary

* Load CSV with `pd.read_csv()`
* Examine shape, `df.info()`, and `df.describe()`
* Check missingness and data types

### Data Cleaning & Feature Engineering

* Convert date columns to `datetime` and create `Engagement Days` (time since joining)
* Bin `Estimated Income` into `Income Band` (`Low`, `Medium`, `High`)
* Map `Fee Structure` to a numeric `Processing Fees` rate for KPI calculations
* Create aggregated metrics such as `Total Loan`, `Total Deposit`, `Total Fees` for dashboard consumption

### Univariate & Bivariate Analysis

* Bar plots for categorical distributions (e.g., `Income Band`, `Nationality`, `Occupation`)
* Histograms / KDE for numerical distributions (`Age`, `Estimated Income`, `Credit Card Balance`)
* Countplots with `hue` to explore relationships between categories

### Correlation Analysis

* Compute Pearson correlation matrix for numerical features: `Age`, `Estimated Income`, `Bank Deposits`, `Credit Card Balance`, `Bank Loans`, `Business Lending`, `Saving Accounts`, `Checking Accounts`.
* Visualize with Seaborn heatmap to identify collinearity and candidate predictors for risk models.

### Power BI Measures & KPIs

Example KPIs (DAX formulas used in Power BI):

* **Total Clients**

  ```dax
  Total Clients = DISTINCTCOUNT('Clients - Banking'[Client ID])
  ```

* **Bank Deposit**

  ```dax
  Bank Deposit = SUM('Clients - Banking'[Bank Deposits])
  ```

* **Total Loan** (bank loan + business lending + credit card balance)

  ```dax
  Total Loan = [Bank Loan] + [Business Lending] + [Credit Cards Balance]
  ```

* **Processing Fees** (example mapping)

  ```dax
  // Set Processing Fees in Power Query or as a calculated column
  Processing Fee = SWITCH('Clients - Banking'[Fee Structure],
      "High", 0.05,
      "Medium", 0.03,
      "Low", 0.01,
      0.02)

  Total Fees = SUMX('Clients - Banking', [Total Loan] * 'Clients - Banking'[Processing Fee])
  ```

* **Engagement Days**

  ```dax
  Engagement Days = DATEDIFF('Clients - Banking'[Joined Bank], TODAY(), DAY)
  ```

---

## Example Python Code Snippets

**Load data & basic summary**

```python
import pandas as pd

df = pd.read_csv('data/bank_clients.csv')
print(df.shape)
print(df.info())
print(df.describe())
```

**Feature engineering: Income Band & Engagement Days**

```python
# Income banding
bins = [0, 100000, 300000, float('inf')]
labels = ['Low', 'Medium', 'High']
df['Income Band'] = pd.cut(df['Estimated Income'], bins=bins, labels=labels, include_lowest=True)

# Convert join date and create engagement days
df['Joined Bank'] = pd.to_datetime(df['Joined Bank'], errors='coerce')
df['Engagement Days'] = (pd.Timestamp('today') - df['Joined Bank']).dt.days

# Map processing fees
fee_map = {'High': 0.05, 'Medium': 0.03, 'Low': 0.01}
df['Processing Fees'] = df['Fee Structure'].map(fee_map).fillna(0.02)
```

**Correlation heatmap**

```python
import seaborn as sns
import matplotlib.pyplot as plt

num_cols = ['Age','Estimated Income','Bank Deposits','Saving Accounts',
            'Checking Accounts','Credit Card Balance','Bank Loans','Business Lending']

corr = df[num_cols].corr()
plt.figure(figsize=(10,8))
sns.heatmap(corr, annot=True, fmt='.2f', cmap='coolwarm')
plt.title('Correlation matrix — numerical features')
plt.show()
```

---

## How to run

1. Create a virtual environment and install dependencies:

```bash
python -m venv venv
source venv/bin/activate   # macOS / Linux
venv\Scripts\activate     # Windows
pip install -r requirements.txt
```

2. Start Jupyter and open the notebooks:

```bash
jupyter notebook notebooks/01_data_loading_and_eda.ipynb
```

3. Open `powerbi/BankInsight_PowerBI.pbix` in Power BI Desktop to view example dashboards and DAX measures (file is a placeholder).

---

## Notebook & Files

* `01_data_loading_and_eda.ipynb` — data ingestion, cleaning, EDA visualizations, correlation heatmap
* `02_feature_engineering_and_kpis.ipynb` — engineered features, KPI computation, export-ready tables
* `powerbi/` — folder for Power BI report and exported datasets
* `data/` — raw CSV source files (sensitive data removed/anonymized)

---

## Insights & Business Impact (Key Findings)

* **Deposits and saving behaviour**: strong positive correlations between deposit-related columns — customers with high balances often hold multiple account types.
* **Income & age**: moderate correlation with savings and loan balances — older, higher-income customers hold larger balances and borrowing.
* **Property ownership**: weak direct correlation with banking variables; this suggests property data alone is insufficient to explain lending risk.
* **Business lending**: shows moderate relationship with bank loans — useful when profiling customers with business exposure.

These insights help prioritize features for predictive modelling and guide the BI dashboards that risk managers use for triage.


