# BankInsight: Credit Risk Analysis

**BankInsight** is a data-driven risk assessment system that analyzes customer profiles and lending behaviours to evaluate creditworthiness. The project performs exploratory data analysis (EDA), feature engineering, and prepares KPIs for Power BI dashboards. The deliverable is a reproducible Python notebook plus Power BI-ready measures to support informed lending decisions.

---
## Dashboards

![Dashboard](https://github.com/exorcist09/BankInsight-Credit-Risk-Analysis/blob/main/PowerBI-Dashboard/DashBoard.png)

![Loan Analysis](https://github.com/exorcist09/BankInsight-Credit-Risk-Analysis/blob/main/PowerBI-Dashboard/Loan-Analysis.png)

![Deposit Analysis](https://github.com/exorcist09/BankInsight-Credit-Risk-Analysis/blob/main/PowerBI-Dashboard/Deposit-Analysis.png)

---
## Overview

This project builds a foundational risk analytics workflow for banks:

- Explore customer demographics and banking behaviour  
- Create features relevant to credit risk (income band, engagement length, fees)  
- Identify patterns and correlations that indicate default risk  
- Produce Power BI-ready KPI measures for management and risk teams  

The analyses are modular to support predictive scoring models later.  

---

## Tools & Technologies

- EXCEL
- SQL
- Python 3.8+  
- Pandas, NumPy, Matplotlib, Seaborn   
- Power BI Desktop (for dashboards & DAX measures)  

---

## Dataset

The dataset contains client banking information across several tables or a single denormalized table:  

- `Clients - Banking` (main profile/transaction table)  
- `Banking Relationship`  
- `Client-Banking`  
- Lookup tables: `Gender`, `Investment Advisor`, `Period`  

Common fields include: `Client ID`, `Estimated Income`, `Age`, `Nationality`, `Occupation`, `Bank Deposits`, `Checking Accounts`, `Saving Accounts`, `Credit Card Balance`, `Bank Loans`, `Business Lending`, `Joined Bank`, `Fee Structure`, `Properties Owned`, `Risk Weighting`.


---


---

## Methodology

### Data Loading & Summary

- Load CSV using `pd.read_csv()`  
- Inspect shape, columns, missing values, and summary statistics  

### Data Cleaning & Feature Engineering

- Convert `Joined Bank` to `datetime` and calculate `Engagement Days`  
- Bin `Estimated Income` into `Income Band` (`Low`, `Medium`, `High`)  
- Map `Fee Structure` to numeric `Processing Fees`  
- Aggregate metrics for dashboard KPIs (`Total Loan`, `Total Deposit`, `Total Fees`)  

### Univariate & Bivariate Analysis

- Bar plots for categorical features  
- Histograms/KDE for numerical features  
- Countplots with `hue` to explore relationships  

### Correlation Analysis

- Pearson correlation for numerical features (`Age`, `Estimated Income`, `Bank Deposits`, `Credit Card Balance`, `Bank Loans`, `Business Lending`, `Saving Accounts`, `Checking Accounts`)  
- Visualize with Seaborn heatmap to identify collinearity  

### Power BI KPIs

- **Total Clients**: `DISTINCTCOUNT('Clients - Banking'[Client ID])`  
- **Bank Deposit**: `SUM('Clients - Banking'[Bank Deposits])`  
- **Total Loan**: `[Bank Loan] + [Business Lending] + [Credit Cards Balance]`  
- **Processing Fees**: Mapped via `Fee Structure`  
- **Engagement Days**: `DATEDIFF('Clients - Banking'[Joined Bank], TODAY(), DAY)`  

---


## How to Run
```bash 
python3 -m venv venv 
```

```bash 
source venv/bin/activate   # macOS / Linux
```

```bash 
venv\Scripts\activate      # Windows 
```

```bash 
pip install pandas numpy, matplotlib seaborn
```

```bash
jupyter notebook notebooks/eda.ipynb
```


## Insights & Business Impact

* Deposits and savings behaviour are strongly correlated — high-balance clients often hold multiple account types.

* Income and age moderately correlate with savings and loans — older, higher-income clients maintain larger balances.

* Property ownership shows weak direct correlation with banking variables.

* Business lending moderately relates to bank loans — useful for profiling clients with business exposure.

* These insights guide KPI selection and predictive modeling for credit risk assessment.
