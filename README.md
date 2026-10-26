# 🎮 Video Game Analytics — End-to-End EDA & Statistical Project

---

## 📌 Project Overview & Domain Context

**Project Title:** Video Game Analytics  
**Domain:** Gaming & Entertainment Industry  
**Course:** Data Analytics using Python  

### 🎯 Business Problem
In a hit-driven entertainment market, video game publishers and platform holders face high capital risks when financing game development and regional marketing. This project performs an end-to-end data analytics lifecycle on historical global video game sales (`vgsales.csv`) to identify regional consumer preferences, publisher efficiency models, platform dynamics, and statistical factors governing commercial success.

---

## 📂 Repository Structure

```text
├── data/
│   ├── raw_vgsales.csv                   # Raw dataset (16,598 rows x 11 cols)
│   └── cleaned_videogame_sales.csv       # Preprocessed dataset (16,291 rows x 11 cols)
├── notebooks/
│   ├── Sprint1_Data_Understanding_Preparation.ipynb
│   ├── Sprint2_Exploratory_Data_Analysis.ipynb
│   └── Sprint3_Advanced_Statistics_Feature_Engineering.ipynb
├── reports/
│   ├── Business_Insights_Report.pdf
│   ├── Statistical_Analysis_Report.pdf
│   └── Feature_Engineering_Report.pdf
└── README.md                             # Project Documentation
```

---

## ⚙️ Tech Stack & Dependencies

* **Language:** Python 3.10+
* **Data Wrangling:** Pandas, NumPy
* **Data Visualization:** Matplotlib, Seaborn
* **Statistical Inference:** SciPy (`scipy.stats`)

---

## 🚀 Execution Lifecycle & Work Done

The project strictly follows a **3-Sprint Development Approach**, transforming raw sales data into actionable strategic recommendations.

```text
Raw Data (vgsales.csv)
   │
   ▼
Sprint 1: Data Understanding & Preparation  ──────►  Cleaned Data (cleaned_videogame_sales.csv)
   │
   ▼
Sprint 2: Exploratory Data Analysis (EDA)    ──────►  Visual Insights & Regional Profiles
   │
   ▼
Sprint 3: Advanced Stats & Feature Eng.     ──────►  Hypothesis Tests, CIs & Business Strategy
```

---

### 🟢 Sprint 1: Data Understanding & Preparation

#### 1. Dataset Inspection
* **Initial Shape:** 16,598 rows × 11 columns.
* **Schema Attributes:** 
  * *Categorical:* `Name`, `Platform`, `Genre`, `Publisher`
  * *Numerical:* `Rank`, `Year`, `NA_Sales`, `EU_Sales`, `JP_Sales`, `Other_Sales`, `Global_Sales`

#### 2. Missing Value Analysis & Cleaning Strategy
* **Identified Issues:** Missing values detected in `Year` (271 missing) and `Publisher` (58 missing). Zero duplicate rows found.
* **Execution:** Dropped incomplete rows in `Year` and `Publisher` subsets. Converted `Year` from `float64` to `int64` for chronological consistency.
* **Final Cleaned Dataset:** 16,291 rows × 11 columns (`cleaned_videogame_sales.csv`).

#### 3. Outlier Handling & Baseline Metrics
* **Outlier Strategy:** Visualized `Global_Sales` via boxplots. High-end blockbusters (e.g., *Wii Sports* at 82.74M, *Super Mario Bros.* at 40.24M) were **retained**, as they represent genuine market blockbusters rather than erroneous data entry.
* **Cumulative Global Sales:** 8,811.97 Million units across all titles.

| Top 5 Genres by Global Volume | Revenue (Million Units) |
| :--- | :---: |
| 🎮 **Action** | 1,722.84 |
| ⚽ **Sports** | 1,309.24 |
| 🎯 **Shooter** | 1,026.20 |
| 🧙 **Role-Playing (RPG)** | 923.83 |
| 🍄 **Platform** | 829.13 |

---

### 🟡 Sprint 2: Exploratory Data Analysis & Visualization

#### 1. Descriptive Statistics & Skewness
Calculated metrics across all regional revenue markets (in millions of units):

| Metric | Global_Sales | NA_Sales | EU_Sales | JP_Sales | Other_Sales |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Mean** | 0.54M | 0.27M | 0.15M | 0.08M | 0.05M |
| **Median (50%)** | 0.17M | 0.08M | 0.02M | 0.00M | 0.01M |
| **Std Dev** | 1.57M | 0.82M | 0.51M | 0.31M | 0.19M |
| **IQR** | 0.42M | 0.24M | 0.11M | 0.04M | 0.04M |
| **Range** | 82.73M | 41.49M | 29.02M | 10.22M | 10.57M |
| **Skewness** | **17.30** | **18.74** | **18.77** | **11.12** | **24.10** |

> 💡 **Observation:** The extreme gap between mean (0.54M) and median (0.17M) along with a positive skewness of 17.30 proves that video game commercial success is heavily right-skewed and blockbuster-driven.

#### 2. Key Exploratory Findings
* **Regional Consumer Divergence:**
  * **NA & EU (Western Markets):** Strong preference for **Action** and **Shooter** genres with nearly identical demand structures. North America serves as the primary anchor market.
  * **Japan (JP Market):** Distinct consumer preference where **Role-Playing Games (RPGs)** dominate sales, while Shooters hold negligible market share.
* **Volume vs. Unit Efficiency:**
  * While *Action* titles lead in total release volume, **Platform** and **Shooter** genres generate the highest *average sales per game*.
* **Publisher Strategy (Quality vs. Quantity):**
  * **Nintendo:** 696 titles released → **1,784.43M** total sales (**2.56M** avg/game).
  * **Electronic Arts (EA):** 1,339 titles released → **1,093.39M** total sales (**0.82M** avg/game).
  * *Insight:* Focus on high-quality flagship IPs yields significantly higher returns than high-volume catalog publishing.

---

### 🔴 Sprint 3: Advanced Statistics & Feature Engineering

#### 1. Higher-Order Distribution & Covariance Metrics
* **Kurtosis (Global Sales):** `596.00` (Extreme leptokurtic distribution confirming heavy fat tails).
* **Covariance Matrix:** Covariance(NA_Sales, EU_Sales) = 0.322, proving a strong co-movement between North American and European markets.

#### 2. Hypothesis Testing (Action vs. Sports Revenue)
To test whether genre selection guarantees significantly higher average sales:
* **Null Hypothesis (H₀):** μ_Action = μ_Sports (No significant difference in mean global sales).
* **Alternative Hypothesis (H_A):** μ_Action ≠ μ_Sports (Significant difference exists).
* **Test Applied:** Welch’s Independent Two-Sample t-Test (`scipy.stats.ttest_ind(..., equal_var=False)`).

**Test Results:**
* **Test Statistic (t):** -0.7917
* **p-value:** 0.4286

* **Statistical Decision:** Fail to reject H₀ (p = 0.4286 > 0.05).
* **Business Takeaway:** Average sales per title between Action and Sports games are not statistically different. Genre volume alone does not guarantee superior individual title sales.

#### 3. Confidence Interval Analysis
Constructed a 95% Confidence Interval for True Mean Global Sales using Student's t-distribution:
* **Margin of Error:** h = Standard Error × t_critical
* **95% Confidence Interval:** **[0.517M, 0.565M] units**
* Provides baseline financial modeling expectations for prospective game releases.

#### 4. Feature Engineering (`Hit_Status`)
Engineered a strategic segmentation metric based on revenue thresholds:
```python
df['Hit_Status'] = df['Global_Sales'].apply(
    lambda x: 'Mega-Hit' if x >= 1.0 else 'Standard'
)
```
* **Mega-Hit (≥ 1.0M units):** 2,058 games (~12.63%)
* **Standard (< 1.0M units):** 14,233 games (~87.37%)

---

## 📈 Strategic Business Recommendations

1. **Adopt a High-Impact IP Strategy:** Emulate Nintendo's high-efficiency model by investing in high-quality core franchises rather than flooding the market with mid-tier releases.
2. **Localization & Market Targeting:** Tailor portfolio distribution geographically—market Shooters and Action titles aggressively in NA/EU, while prioritizing RPG investments in the Japanese market.
3. **Risk Management via Baseline Modeling:** Utilize the 95% CI baseline ([0.517M, 0.565M] units) for initial project greenlighting and budget allocation.

---

## 📋 Project Submission Evaluation Checklist

- [x] **Sprint 1:** Data Preprocessing, Null/Duplicate Cleaning, Outlier Boxplots (`cleaned_videogame_sales.csv`)
- [x] **Sprint 2:** Univariate, Bivariate, Multivariate Visualizations, GroupBy & Pivot Tables
- [x] **Sprint 3:** Statistical Metrics, Welch's T-Test, 95% Confidence Intervals, Feature Engineering (`Hit_Status`)
- [x] **Documentation:** Developer-Grade Markdown README, Visuals, Metrics & Strategic Recommendations

---

<div align="center">
  <sub>Built as part of the <b>EDA Data Analytics Project using Python</b> Data Science Course.</sub>
</div>
