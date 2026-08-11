# Customer Churn Prediction

## Overview

This project analyzes customer churn using the Telco Customer Churn dataset.

The first phase focuses on data understanding, data cleaning, exploratory data analysis, statistical analysis, and feature engineering.

```text
Data Understanding
       ↓
Data Cleaning
       ↓
Exploratory Data Analysis
       ↓
Statistical Analysis
       ↓
Feature Engineering
```

---

## Business Problem

Customer churn represents customers who discontinue their service.

The objective of this phase is to understand which customer characteristics and service-related factors are associated with churn and prepare a feature-ready dataset for the next phase.

---

## Business Questions

* What proportion of customers churn?
* Which customer characteristics are associated with churn?
* How does tenure relate to churn?
* How does contract type relate to churn?
* How do monthly and total charges differ between churned and non-churned customers?
* Which services are associated with churn?
* How does service adoption relate to churn?
* Which customer segments show higher churn rates?
* Which variables show statistically meaningful relationships with churn?

---

## Dataset

The dataset contains:

* **7,043 customers**
* **21 columns**

The target variable is `Churn`.

| Churn | Customers | Percentage |
| ----- | --------: | ---------: |
| No    |     5,174 |     73.46% |
| Yes   |     1,869 |     26.54% |

---

## Project Structure

```text
customer-churn-prediction/
│
├── data/
│   ├── raw/
│   │   └── WA_Fn-UseC_-Telco-Customer-Churn.csv
│   │
│   └── processed/
│       ├── telco_cleaned.csv
│       └── telco_features.csv
│
├── images/
│
├── models/
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_statistical_analysis.ipynb
│   ├── 05_feature_engineering.ipynb
│   ├── 06_modeling.ipynb
│   └── 07_model_interpretation.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── features.py
│   ├── train.py
│   └── evaluate.py
│
├── .gitignore
├── README.md
└── requirements.txt
```

---

# Phase 1 — Data Analysis

## 1. Data Understanding

The first notebook focuses on understanding the structure and quality of the dataset.

The following areas were examined:

* Dataset dimensions
* Column names
* Data types
* Missing values
* Duplicate rows
* Unique values
* Cardinality
* Target distribution
* Data dictionary
* Initial business questions

The original dataset contains 7,043 customer records and 21 columns.

The initial analysis identified `TotalCharges` as an important data-quality issue because it was stored as an object rather than a numeric variable.

---

## 2. Data Cleaning

The cleaning process focused on preparing a reliable dataset while preserving the original information.

### Data Consistency Fix

`SeniorCitizen` was originally stored as `0`/`1`, while all other binary
attributes in the dataset used `"Yes"`/`"No"`. This column was standardized
to `"Yes"`/`"No"` for consistency across the dataset.

### Data Type Corrections

`TotalCharges` was converted from an object column to numeric.

During this process, 11 blank values were identified.

These records had:

```text
tenure = 0
```

Therefore, the blank `TotalCharges` values were treated as zero.

The cleaned dataset was checked for:

* Remaining missing values
* Duplicate rows
* Negative values
* Target consistency
* Data leakage
* Consistency between total charges, tenure, and monthly charges

The cleaned dataset contains:

```text
7,043 rows
21 columns
```

The cleaned data was saved as:

```text
data/processed/telco_cleaned.csv
```

---

# 3. Exploratory Data Analysis

EDA was performed to understand customer characteristics, service usage, financial behavior, and customer segments in relation to churn.

## 3.1 Customer Profile

The following characteristics were analyzed:

* Gender
* Senior Citizen
* Partner
* Dependents

### Churn by Gender

![Churn by Gender](images/churn-rate-by-gender.png)

### Churn by Senior Citizen

![Churn by Senior Citizen](images/churn-rate-by-senior-citizen-status.png)

### Churn by Partner

![Churn by Partner](images/churn-rate-by-partner-status.png)

### Churn by Dependents

![Churn by Dependents](images/churn-rate-by-dependents.png)

---

## 3.2 Customer Relationship

Customer relationship characteristics were examined through:

* Tenure
* Contract type
* Payment method

### Churn by Tenure

![Churn by Tenure](images/tenure-distribution-by-churn-status.png)

Tenure showed a clear relationship with churn, with newer customers showing substantially higher churn.

### Churn by Contract

![Churn by Contract](images/churn-rate-by-contract-type.png)

Contract type was one of the strongest patterns identified during EDA.

Month-to-month customers showed considerably higher churn compared with customers under longer-term contracts.

### Churn by Payment Method

![Churn by Payment Method](images/churn-rate-by-payment-method.png)

---

## 3.3 Financial Analysis

The financial analysis examined:

* Monthly Charges
* Total Charges
* Average Monthly Spend

### Monthly Charges Distribution

![Monthly Charges Distribution](images/monthly-charges-distribution-by-churn-status.png)

### Total Charges Distribution

![Total Charges Distribution](images/total-charges-distribution-by-churn-status.png)

### Tenure vs Monthly Charges by Churn

![Monthly Charges by Churn](images/tenure-vs-monthly-charges-by-churn-status.png)


---

## 3.4 Service Analysis

The following services were analyzed:

* Internet Service
* Online Security
* Online Backup
* Device Protection
* Tech Support
* Streaming TV
* Streaming Movies

### Churn by Internet Service

![Churn by Internet Service](images/churn-rate-by-internet-service-type.png)

### Churn by Online Security

![Churn by Online Security](images/churn-rate-by-online-security.png)

### Churn by Online Backup

![Churn by Online Backup](images/churn-rate-by-online-backup.png)

### Churn by Device Protection

![Churn by Device Protection](images/churn-rate-by-device-protection.png)

### Churn by Tech Support

![Churn by Tech Support](images/churn-rate-by-tech-support.png)

### Churn by Streaming TV and Churn by Streaming Movies

![Churn by Streaming TV](images/churn-rate-by-streaming-tv-and-streaming-movies.png)


---

## 3.5 Customer Segmentation

Several customer segments were created and analyzed.

### Customer Value

Customers were analyzed according to their spending and value characteristics.

![Churn by Customer Value](images/churn-rate-high-value-vs-low-value-customers.png)

### New vs Long-Term Customers

![Churn by Customer Tenure Segment](images/churn-rate-new-vs-long-term-customers.png)

### Contract × Tenure

![Churn by Contract and Tenure](images/churn-rate-by-contract-tenure-segment.png)

### Service Adoption

The number of subscribed services was analyzed in relation to churn.

![Churn by Service Count](images/churn-rate-by-service-adoption-count.png)

---

# 4. Statistical Analysis

Statistical analysis was performed to determine whether the relationships observed during EDA were statistically meaningful.

## 4.1 Categorical Variables

Chi-square tests were used to examine associations between categorical variables and churn.

Cramér's V was used to measure the strength of association.

| Variable         | Cramér's V |
| ---------------- | ---------: |
| Contract         |      0.410 |
| Online Security  |      0.347 |
| Tech Support     |      0.343 |
| Internet Service |      0.322 |
| Payment Method   |      0.303 |

Gender showed almost no association with churn:

```text
Cramér's V = 0.008
```

### Categorical Association Strength

![Categorical Association with Churn](images/chi-square-statistic-by-categorical-variable.png)

---

## 4.2 Numerical Variables

Welch's t-test was used to compare numerical variables between churned and non-churned customers.

The variables examined included:

* Tenure
* Monthly Charges
* Total Charges

Effect sizes were measured using Cohen's d.

| Variable        | Cohen's d |
| --------------- | --------: |
| Tenure          |    -0.852 |
| Total Charges   |    -0.458 |
| Monthly Charges |     0.446 |

### Numerical Effect Sizes

![Numerical Effect Sizes](images/mean-comparison-churned-vs-not-churned.png)

Tenure showed the largest effect among the numerical variables examined.

---

## 4.3 Confidence Intervals

Confidence intervals were calculated for relevant group comparisons, including contract-related churn differences.

![Contract Churn Confidence Interval](images/churn-rate-by-contract-95percent-confidence-intervals.png)

---

## 4.4 Point-Biserial Correlation

Point-biserial correlation was used to measure relationships between numerical variables and the binary churn target.

| Variable        | Correlation |
| --------------- | ----------: |
| Tenure          |      -0.352 |
| Total Charges   |      -0.198 |
| Monthly Charges |       0.193 |

### Point-Biserial Correlation

![Point-Biserial Correlation](images/point-biserial-correlation-with-churn.png)

---

# 5. Feature Engineering

The feature engineering phase created additional variables based on findings from EDA and statistical analysis.

The following features were created:

```text
tenure_group
monthly_charge_group
avg_monthly_spend
service_count
has_security
has_support
is_new_customer
is_high_value_customer
is_month_to_month
contract_length_months
is_new_month_to_month
is_high_charge_no_support
```

The resulting feature-ready dataset contains:

```text
7,043 rows
33 columns
```

and was saved as:

```text
data/processed/telco_features.csv
```

---

## Feature Relationship Analysis

Multicollinearity was examined among numerical features.

A particularly strong relationship was identified between:

```text
MonthlyCharges
avg_monthly_spend
```

with a correlation of approximately:

```text
0.996
```

Other strong relationships were also observed between:

* Monthly Charges and Service Count
* Total Charges and Service Count

These relationships will be considered during the machine learning preprocessing stage.

In particular, `avg_monthly_spend` and `MonthlyCharges` are near-duplicate
features (r ≈ 0.996) and one of the two should be dropped for any linear or
distance-based model in Phase 2.

### Feature Correlation Matrix

![Feature Correlation Matrix](images/multicollinearity-check-numeric-features.png)

---

# Key Findings

### Contract Type

Contract type was the strongest categorical variable associated with churn.

```text
Cramér's V = 0.410
```

Month-to-month customers showed substantially higher churn.

### Tenure

Tenure showed the strongest numerical effect:

```text
Cohen's d = -0.852
```

and the strongest numerical correlation with churn:

```text
r = -0.352
```

Higher tenure was associated with lower churn.

### Monthly Charges

Monthly charges showed a positive relationship with churn:

```text
r = 0.193
```

Higher monthly charges were associated with higher churn tendency.

### Services

Online Security and Tech Support showed relatively strong associations with churn:

```text
OnlineSecurity → Cramér's V = 0.347
TechSupport    → Cramér's V = 0.343
```

### Gender

Gender showed almost no relationship with churn:

```text
Cramér's V = 0.008
```

### Payment Method

Customers paying by electronic check showed a higher churn rate compared
with customers using automatic payment methods (bank transfer or credit
card).

---

# Phase 1 Conclusion

Phase 1 established a cleaned and feature-ready dataset and identified the main customer characteristics associated with churn.

The most important patterns identified were related to:

* Contract type
* Customer tenure
* Monthly charges
* Security services
* Technical support
* Payment method
* Service adoption

The final feature-ready dataset is prepared for the next phase of the project.

---

# Phase 2 — Machine Learning

The following notebooks are reserved for the next phase:

```text
06_modeling.ipynb
07_model_interpretation.ipynb
```

Machine learning implementation and model interpretation are not part of the completed Phase 1 analysis.

---

## Technologies

* Python
* Pandas
* NumPy
* Matplotlib
* SciPy
* Jupyter Notebook

---

## Dataset Setup

The raw dataset is not stored in the GitHub repository.

After downloading the dataset, place the CSV file inside:

```text
data/raw/
```

Expected file:

```text
WA_Fn-UseC_-Telco-Customer-Churn.csv
```

Processed datasets generated during Phase 1 are stored in:

```text
data/processed/
```
