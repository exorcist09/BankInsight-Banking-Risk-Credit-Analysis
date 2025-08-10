# BankInsight - Banking Business Analysis 

Problem statement -> Design and develop a comprehensive risk assessment system aimed at analyzing customer profiles and lending behaviors to accurately evaluate creditworthiness. The system will utilize data-driven techniques to identify potential risks associated with lending, thereby enabling financial institutions to minimize the likelihood of monetary loss. Through predictive analytics and continuous monitoring, the solution will support informed lending decisions

Types of customer --> high earnign , middle, low earning --> on thier careatersitcs the  loan is given to them accordingly
--> few of them they do not pay back which causes loss to the banking system


Our work over here is to do the risk analysis, predictive analysis

END goal is to show important KPI's to managers and risk anaylis 


- Dataset contains customer banking data including demographics, account balances, loans, and other financial indicators.
- Goal: Understand data distribution, detect patterns, and explore relationships to generate actionable insights.
- Tools used: Python (Pandas, Matplotlib, Seaborn), Jupyter/VSCode notebooks.

---

## Contents

- Data Loading and Summary  
- Data Cleaning & Feature Engineering  
- Univariate Analysis of Categorical and Numerical Variables  
- Bivariate Analysis with Countplots and Histograms  
- Correlation Heatmap for Numerical Variables  
- Insights & Interpretation  
- [Placeholder for Power BI Dashboard]  

---

## 1. Data Loading and Initial Summary

- Loaded dataset using `pandas.read_csv()`.
- Checked data shape and info summary.
- Examined basic statistics using `describe()`.

---

## 2. Feature Engineering

- Created a new categorical feature `Income Band` by binning the `Estimated Income` into **Low**, **Medium**, and **High** groups using pandas `cut()`.

---

## 3. Univariate Analysis

- Visualized distribution of **Income Band** using bar charts.
- Analyzed unique values and counts for categorical columns such as:
  - Nationality
  - Occupation
  - Fee Structure
  - Loyalty Classification
  - Properties Owned
  - Risk Weighting
- Checked for missing values and converted date columns to datetime format.

---

## 4. Bivariate Analysis

- Used `countplot` with hue to explore relationships between categorical variables and `Nationality`.
- Plotted histograms for categorical variables excluding `Occupation`.
- Visualized distributions of numerical columns with histograms and KDE plots.

---

## 5. Correlation Analysis

- Calculated Pearson correlation matrix for key numerical features such as:
  - Age, Estimated Income, Superannuation Savings
  - Credit Card Balance, Bank Loans, Bank Deposits
  - Checking & Saving Accounts, Foreign Currency Account, Business Lending
- Displayed correlations with a heatmap using Seaborn for easy interpretation.

---

## 6. Key Insights from EDA

- **Deposits and Saving Behavior:**  
  Strong positive correlation between different types of bank deposits and accounts, indicating customers with high balances tend to have multiple account types.

- **Income, Age, and Accumulation:**  
  Moderate correlations suggest older and higher-income customers tend to accumulate more savings, loans, and credit balances.

- **Property Ownership:**  
  Weak correlation with banking variables, possibly influenced by external factors not captured in dataset.

- **Business Lending:**  
  Moderate relation to bank loans, indicating some overlap in personal and business financial behavior.

---

## 7. Power BI Dashboard

<!--  
Insert your Power BI dashboard screenshots here or embed your Power BI report links.  
You can add visuals such as interactive bar charts, slicers, and trend analyses to complement this analysis.

Example:

![Power BI Dashboard Screenshot](path_to_screenshot.png)

Or link:

[View Interactive Power BI Dashboard](https://app.powerbi.com/view?r=example-link)  
-->

---

## How to Run

1. Install required Python libraries:  
   `pip install pandas matplotlib seaborn numpy`

2. Run the Jupyter notebook or Python script.

3. Visualizations and analyses will display inline.

---
