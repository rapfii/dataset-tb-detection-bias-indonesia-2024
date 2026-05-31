# 📊 Tuberculosis Case Notification in Indonesia (2024): When More Hospitals Mean More Disease (On Paper)

> **A small-sample regression benchmark exploring how detection systems can reshape observed disease patterns, without assuming a fixed causal direction.**

[![Kaggle Dataset](https://img.shields.io/badge/Kaggle-Dataset-blue?logo=kaggle&style=for-the-badge)](https://www.kaggle.com/)
[![Cleanliness](https://img.shields.io/badge/Data_Cleanliness-Validated-brightgreen?style=for-the-badge)](https://github.com/)
[![Model Difficulty](https://img.shields.io/badge/Modeling-Challenging-red?style=for-the-badge)](https://github.com/)

This dataset explores a counterintuitive pattern where higher reported disease may coincide with stronger healthcare systems, which may reflect differences in detection, reporting systems, or underlying disease burden. 

*There is no single "correct" model — only more robust ones under different assumptions.*

---

## 🎯 The Challenge
*Can you build a model where poverty becomes statistically significant, or will municipal leverage points and healthcare surveillance intensity dominate your parameters?*

**Difficulty Level:** Intermediate $\rightarrow$ Advanced (Statistical Modeling)

Indonesia ranks among the countries with the highest tuberculosis (TB) burden globally. However, public health records do not capture true incidence—they capture *reported* cases. This dataset serves as a case study in **Tuberculosis Case Notification Bias** and a **Small-Sample Regression Benchmark** across all **38 provinces in Indonesia for the year 2024**, exploring the gap between reported and underlying disease burden. Combining officially published records from the **Ministry of Health (Kemenkes)** and the **Central Bureau of Statistics (BPS)**, it challenges standard modeling assumptions.

What makes this dataset unique is that it challenges the assumption that better infrastructure always reduces observed disease.

> [!WARNING]
> This dataset contains realistic modeling challenges such as high leverage outliers and multicollinearity. Interpret results cautiously — statistical significance in this dataset does not imply real-world causality.

---

## 🧪 Benchmark Identity
This dataset behaves similarly to classical small-N regression benchmarks with high sensitivity to outliers and model specification. Similar to canonical small-sample regression benchmarks used in statistical learning, this package serves as a testbed for regularized regression, outlier diagnostics, and model interpretability under real-world constraint anomalies.

---

## 🎓 What This Dataset Teaches
* **Small-N Instability:** Demonstrates how regression coefficients in small samples ($N=38$) are highly vulnerable to specifications and features.
* **Detection vs. Reality Gap:** Highlights the gap between measured disease surveillance and true underlying disease patterns.
* **Single-Observation Influence:** Explores how a single data point (DKI Jakarta) can dictate global model parameters.

---

## 👥 Audience & Use Cases

### Who This Dataset Is For
* **Statistics Learners & Educators:** An ideal real-world teaching tool to demonstrate multivariate linear regression, check assumptions (VIF, residual diagnostics), and deal with extreme leverage points.
* **Data Science Practitioners:** A challenging regression package containing natural variability and severe outliers to practice robust scaling, regularization, and model validation on small sample sizes.
* **Public Health Analysts:** Suitable for exploring structural correlates of infectious disease detection and simulating regional health policy adjustments.

### Who Should NOT Use This Dataset
* **Time-Series Modeler:** This dataset is strictly cross-sectional (constant year `2024`) and lacks temporal observations.
* **Causal Inference Researchers:** Regression coefficients cannot be interpreted causally without instrumental variables due to strong endogeneity (e.g., healthcare facility placement vs. regional case detection).

---

## 💡 Key Research Questions

1. **Healthcare Access vs. Reported Burden:** Does regional healthcare access (facility ratio) systematically inflate case notification rates (CNR), representing detection bias?
2. **Socioeconomic Significance:** Is regional poverty still a statistically significant predictor of tuberculosis notifications once environmental housing conditions are controlled?
3. **The Crowding Effect:** To what extent does population density explain TB notification rates when extreme municipal hubs are treated as leverage points?

---

## 📖 Data Dictionary & Hypothesized Effects

### Schema

| Variable | Type | Unit | Description | Role in Modeling |
| :--- | :--- | :--- | :--- | :--- |
| **`province`** | Nominal String | - | Name of the province in Indonesia | Observation Identifier |
| **`tb_case_notification_rate`** | Continuous Int | cases per 100k pop | Cases detected & reported per 100,000 residents | **Target Variable (Y)** |
| **`population_density`** | Continuous Int | people/km² | Average residents per square kilometer | Feature ($X_1$) |
| **`log_population_density`** | Continuous Float | log(people/km²) | Natural logarithm of population density | Feature ($X_{1\_log}$) |
| **`proper_housing_percent`** | Continuous Float | % | Households living in decent/proper housing | Feature ($X_2$) |
| **`health_facilities_ratio`** | Continuous Float | facilities per 100k pop | Puskesmas + Hospitals per 100,000 residents | Feature ($X_3$) |
| **`poverty_rate`** | Continuous Float | % | Population living below the poverty line | Feature ($X_4$) |
| **`year`** | Integer | year | Calendar year of data (constant `2024`) | Metadata / Filter |

### Common Theoretical Expectations
* **`population_density`**: Common theoretical expectation: positive correlation (increased transmission in crowded areas), though may not hold in this dataset due to extreme urban centers (see Outlier Profile).
* **`proper_housing_percent`**: Common theoretical expectation: negative correlation (better housing quality reduces indoor transmission risk), though may not hold in this dataset due to structural detection capacity masking.
* **`health_facilities_ratio`**: Common theoretical expectation: positive correlation (higher clinic density improves diagnostics and reporting), though may not hold in this dataset depending on spatial distribution.
* **`poverty_rate`**: Common theoretical expectation: positive correlation (nutritional/disparity risks), though may not hold in this dataset due to diagnostic gaps in underserved regions.

---

## 📈 Exploratory Data Analysis (EDA) Preview

### Descriptive Statistics ($N=38$)

| Variable | Mean | Std Dev | Min | Max |
| :--- | :---: | :---: | :---: | :---: |
| **`tb_case_notification_rate`** | 303.84 | 151.51 | 104.00 | 872.00 |
| **`population_density`** | 678.24 | 2609.45 | 5.00 | 16,165.00 |
| **`log_population_density`** | 4.71 | 1.66 | 1.61 | 9.69 |
| **`proper_housing_percent`** | 61.66 | 16.18 | 4.44 | 86.68 |
| **`health_facilities_ratio`** | 8.01 | 4.12 | 2.19 | 18.50 |
| **`poverty_rate`** | 11.15 | 6.75 | 4.00 | 32.97 |

### Correlation Matrix

| | `tb_case_notification_rate` | `population_density` | `log_population_density` | `proper_housing_percent` | `health_facilities_ratio` | `poverty_rate` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **`tb_case_notification_rate`** | 1.0000 | 0.3077 | -0.1386 | -0.2096 | 0.3061 | 0.1091 |
| **`population_density`** | 0.3077 | 1.0000 | 0.6011 | -0.1874 | -0.3155 | -0.2054 |
| **`log_population_density`** | -0.1386 | 0.6011 | 1.0000 | 0.2215 | -0.7777 | -0.4299 |
| **`proper_housing_percent`** | -0.2096 | -0.1874 | 0.2215 | 1.0000 | -0.2716 | -0.5738 |
| **`health_facilities_ratio`** | 0.3061 | -0.3155 | -0.7777 | -0.2716 | 1.0000 | 0.5777 |
| **`poverty_rate`** | 0.1091 | -0.2054 | -0.4299 | -0.5738 | 0.5777 | 1.0000 |

### 💡 Surprising Findings (The Punchlines)
* **Burden vs. Detection:** The positive correlation between TB notification rates and `health_facilities_ratio` ($r = 0.3061$) may reflect differences in detection, reporting systems, or underlying disease burden. This reporting bias is likely non-uniform and may systematically underrepresent regions with limited healthcare access.
* **The Density Flip:** While raw `population_density` is positively correlated with the case notification rate ($r = 0.3077$), log-transforming it flips the correlation to negative ($r = -0.1386$), indicating the relationship is heavily influenced by the municipal outlier (see Outlier Profile).

---

## ⚖️ Baseline Model Performance (OLS Regression)

We fit two baseline Ordinary Least Squares (OLS) models predicting `tb_case_notification_rate` ($Y$). 

### Model 1: Raw Predictors ($X_1$ = `population_density`)
* **R-squared:** $0.2829$ (Adjusted R-squared: $0.1960$)
* **Model F-statistic:** $3.2552$ ($p = 0.0234$)

| Predictor | Coefficient | Std Error | t-statistic | p-value | Significance |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Intercept** | 214.4210 | 156.1001 | 1.3736 | 0.1788 | |
| **`population_density`** | 0.0246 | 0.0097 | 2.5312 | 0.0162 | $\star$ (Significant at 5%) |
| **`proper_housing_percent`**| -0.6595 | 1.8223 | -0.3619 | 0.7198 | |
| **`health_facilities_ratio`**| 18.2374 | 6.8456 | 2.6641 | 0.0118 | $\star$ (Significant at 5%) |
| **`poverty_rate`** | -2.9404 | 4.9207 | -0.5976 | 0.5542 | |

* **💡 The Baseline Punchline:** **Classical socioeconomic predictors do not appear statistically significant under this specific baseline specification within this small-sample setting, in the presence of healthcare system variables.** Neither `poverty_rate` ($p = 0.5542$) nor `proper_housing_percent` ($p = 0.7198$) are statistically significant in standard OLS. This suggests that naive models are heavily dominated by healthcare facility density and municipal density spikes.

### Model 2: Log Density ($X_1$ = `log_population_density`)
* **R-squared:** $0.1726$ (Adjusted R-squared: $0.0723$)
* **Model F-statistic:** $1.7206$ ($p = 0.1691$)

### Expected Model Behavior
* **R-squared:** Typically falls between $0.15 - 0.30$.
* **Overfitting / Leakage Sign:** Substantially higher R² values may indicate overfitting or data leakage.
* **Coefficients:** May be unstable across specifications.
* **Coefficient Behavior:** Coefficient signs may flip across specifications, especially under different outlier treatments. Sign flips across models are expected and should not be over-interpreted.
* **Sensitivity:** Significance of predictors is highly sensitive to outlier treatment.

---

## 🎯 Outlier Profile: DKI Jakarta (The Leverage Point)
DKI Jakarta acts as an extreme outlier in population density ($16,165$ people/km² vs. the provincial median of $95$ people/km²). Leaving Jakarta untreated in standard OLS regressions introduces a massive leverage effect, driving a significant positive density slope coefficient that disappears when Jakarta is removed or log-transformed. This makes the dataset highly sensitive to single-observation influence, a defining characteristic of small-N regression problems.

---

## ⚠️ Why Naive Models Fail on This Dataset

* **High Risk of Spurious Regression:** Due to the small sample size ($N = 38$) and correlated predictors, naive OLS runs a high risk of estimating unstable or theoretically contradictory coefficients.
* **Multicollinearity Inflation:** A moderate correlation exists between `poverty_rate` and `proper_housing_percent` ($r = -0.5738$), which is expected to inflate Variance Inflation Factors (VIF), leading to inflated standard errors and unstable coefficient estimates. VIF values are expected to exceed common thresholds ($>5$) for correlated predictors such as poverty and housing. Users should expect parameter instability unless regularization or dimensionality reduction (such as Ridge/Lasso or PCA) is applied. Users should explicitly test VIF and consider dropping or transforming correlated variables.
* **Reciprocal Endogeneity:** Regions with higher tuberculosis burdens are prioritized for health program funding (leading to more primary health centers/Puskesmas). This reciprocal feedback loop loops `health_facilities_ratio` and `tb_case_notification_rate`, introducing simultaneity bias.
* **Surveillance Intensity Proxy:** A high facility ratio may proxy surveillance intensity rather than healthcare quality. In regions with sparse clinics, cases go unrecorded, systematically underrepresenting rural regions with limited healthcare access.
* **Omitted Variable Bias:** This dataset focuses on structural and socioeconomic correlates. Epidemiological factors like regional air quality index (AQI), smoking rates, and diabetes/HIV co-morbidities are omitted, which may bias parameter estimations.

### 🛑 Expected Failure Modes
* **Naive OLS without transformations:** Will yield non-significant socioeconomic parameters and be heavily dominated by DKI Jakarta's density coefficient.
* **Unscaled models:** Coefficients will appear orders of magnitude apart due to the difference in variable scales (density in thousands vs. ratios under 20).

---

## 🚀 Minimal Usage (Quick Start)

Load the data and run a quick baseline model using the following snippets:

### 1. Statsmodels Baseline (Regression Diagnostics)
```python
import pandas as pd
import statsmodels.api as sm

# Load dataset
df = pd.read_csv('indonesia_tb_detection_bias_2024.csv')

# Define features and target
X = sm.add_constant(df[['health_facilities_ratio', 'poverty_rate']])
y = df['tb_case_notification_rate']

# Fit OLS
model = sm.OLS(y, X).fit()
print(model.summary())
```

### 2. Scikit-Learn Minimal Benchmark
```python
import pandas as pd
from sklearn.linear_model import LinearRegression

# Load dataset
df = pd.read_csv('indonesia_tb_detection_bias_2024.csv')

# Fit Linear Regression
model = LinearRegression()
model.fit(df[['health_facilities_ratio', 'poverty_rate']], df['tb_case_notification_rate'])
print("Coefficients:", model.coef_)
print("Intercept:", model.intercept_)
```

---

## 🎮 Kaggle Playground Challenges

Try these challenges to explore the boundaries of this dataset:
1. **The DKI Jakarta Stress Test:** Re-fit the regression model after removing DKI Jakarta from the dataset. Does `population_density` remain a significant predictor?
2. **The Poverty Paradox:** Can you construct a subset of provinces or apply a feature transformation (such as interaction terms `poverty * proper_housing_percent` or non-linear effects) where `poverty_rate` becomes a statistically significant predictor of notifications?
3. **Regularization Showdown:** Train OLS, Ridge, and Lasso models. Compare how Ridge and Lasso handle the collinearity between `poverty_rate` and `proper_housing_percent`.

---

## 🚀 Suggested Modeling Pipeline

To achieve robust results, we recommend the following diagnostic and modeling pipeline:
1. **Leverage Testing:** Compute Cook's Distance and leverage diagnostics to assess the impact of DKI Jakarta.
2. **Feature Engineering Hints:** Test interaction terms or non-linear transformations (e.g. combining log transformations with interaction terms like `log_population_density * poverty_rate`) to capture non-linear relationships. Combining log transformations with interaction terms may reveal hidden structure in the data not captured by linear assumptions.
3. **Feature Standardization:** Scale all continuous variables (mean=0, std=1) to directly compare coefficient weights.
4. **Compare Regularization Techniques:** Compare standard OLS against Ridge (to handle collinearity) and Lasso (for variable selection).

### What Good Modeling Looks Like
* **Stable Coefficients:** Model parameter estimates remain stable under leave-one-out cross-validation.
* **Consistent Signs:** Coefficients retain their hypothesized direction (e.g., poverty showing a consistent positive or housing a consistent negative sign) across specifications.
* **Jakarta Robustness:** Parameters remain robust to the inclusion or exclusion of the DKI Jakarta leverage point.

### Reproducibility Notes
All baseline results were generated using standard OLS with no scaling, no interaction terms, and full dataset inclusion (including DKI Jakarta). Variations in preprocessing, feature scaling, or outlier handling may produce different results. The dataset is deterministic, but modeling outputs may vary depending on preprocessing, feature engineering, and model specification.

**Environment details:** Python 3.8+; minimal library dependencies: `pandas`, `numpy`, `statsmodels`, `scikit-learn`.

---

## 🛠️ Data Quality & Dataset Limitations

### Dataset Limitations
* **Cross-Sectional Limitation:** Contains single-year data (constant `2024`).
* **Reporting Gaps:** Possible under-reporting bias in remote regions due to healthcare access disparities.
* **Small Sample Size:** Only contains $N=38$ provincial observations, meaning parameters are sensitive to specification changes.

### Data Compilation
The data was carefully compiled and cross-checked against official tabulated values and internally validated using deterministic consistency checks, though minor discrepancies due to rounding may still exist. Checks confirmed that regional faskes ratios mathematically aligned with BPS population projections (`pop_lk`) and health infrastructure counts.

---

## 🔗 Original Sources & Citations

### 1. Profil Kesehatan Indonesia 2024 (Ministry of Health RI)
Published on **September 12, 2025**, with 156,574+ downloads.
* **Source Link:** [Kemenkes Profil Kesehatan Indonesia 2024](https://kemkes.go.id/id/profil-kesehatan-indonesia-2024)
* **Specific Chapters/Tables used:**
  * **Lampiran 56.a:** Case Notification Rate (CNR) per 100,000 population $\rightarrow$ `tb_case_notification_rate`
  * **Lampiran 4.a:** Number of Puskesmas per Province $\rightarrow$ Used to calculate `health_facilities_ratio`
  * **Lampiran 8.c:** Number of Hospitals per Province $\rightarrow$ Used to calculate `health_facilities_ratio`
  * **Lampiran 83.e:** Percentage of Households occupying Decent/Proper Housing $\rightarrow$ `proper_housing_percent`

### 2. Badan Pusat Statistik (BPS)
* **Population Density:** [Kepadatan Penduduk Menurut Provinsi 2024](https://www.bps.go.id/id/statistics-table/3/V1ZSbFRUY3lTbFpEYTNsVWNGcDZjek53YkhsNFFUMDkjMw%253D%253D/penduduk--laju-pertumbuhan-penduduk--distribusi-persentase-penduduk--kepadatan-penduduk--rasio-jenis-kelamin-penduduk-menurut-provinsi--2024.html?year=2024) (Pertengahan tahun/Juni 2024).
* **Poverty Rate:** [Persentase Penduduk Miskin Menurut Provinsi 2024](https://www.bps.go.id/id/statistics-table/3/UkVkWGJVZFNWakl6VWxKVFQwWjVWeTlSZDNabVFUMDkjMw==/jumlah-dan-persentase-penduduk-miskin-menurut-provinsi--2024.html?year=2024) (March 2024 Survey).

---

## 📜 License

This dataset is derived from publicly available government statistical publications (BPS & Ministry of Health Indonesia). It is shared for educational, academic research, and machine learning benchmarking purposes under open research terms.
