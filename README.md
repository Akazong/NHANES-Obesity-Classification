# Predicting Obesity Among U.S. Adults Using Machine Learning

## Overview

This project develops and evaluates machine-learning classification models for identifying BMI-defined obesity among adults using data from the U.S. National Health and Nutrition Examination Survey (NHANES).

The analysis integrates three NHANES survey cycles:

- 2013–2014
- 2015–2016
- 2017–2018

After data integration, cleaning and restriction to adults aged 18 years and older with available BMI measurements, the final analytical dataset contained **16,943 participants**.

Obesity was defined as **BMI ≥ 30 kg/m²**.

Importantly, although BMI was used to define the outcome, **BMI and waist circumference were excluded from the predictor set**. This avoids circular prediction and reduces reliance on an anthropometric measure closely related to the BMI-defined outcome.

---

## Research Question

**How well can machine-learning classifiers identify BMI-defined obesity among U.S. adults using demographic, socioeconomic, lifestyle and clinical characteristics without BMI or waist circumference?**

---

## Data Source

Data were obtained from the publicly available National Health and Nutrition Examination Survey (NHANES), conducted by the U.S. National Center for Health Statistics.

The project uses participant-level data from the 2013–2014, 2015–2016 and 2017–2018 survey cycles.

NHANES datasets covering demographic, socioeconomic, lifestyle, clinical and laboratory characteristics were integrated using the participant identifier (`SEQN`).

---

## Analytical Workflow

The project was organised into four Jupyter notebooks:

### 1. Data Collection and Integration

NHANES datasets were imported and merged within each survey cycle using the participant identifier. Variables relevant to obesity classification were selected, and the three survey cycles were subsequently combined.

### 2. Data Cleaning and Preprocessing

The analysis was restricted to adults aged 18 years and older.

Special NHANES response codes and missing values were identified and handled appropriately. BMI was used to derive the binary obesity outcome:

- **0:** Not obese
- **1:** Obese (BMI ≥ 30 kg/m²)

The final analytical dataset contained **16,943 adults**.

### 3. Exploratory Data Analysis

Obesity patterns were explored across demographic, socioeconomic, lifestyle and clinical characteristics.

The EDA examined:

- Sex
- Age
- Race/ethnicity
- Education
- Income-to-poverty ratio
- Smoking history
- Physical activity
- Weight-change intention
- Diabetes and prediabetes
- Cardiovascular conditions
- Arthritis
- Blood pressure
- HDL cholesterol
- Total cholesterol
- HbA1c
- Sedentary time
- Depressive symptoms
- Drug-use history

The exploratory findings were descriptive and unadjusted and were not interpreted as causal relationships.

### 4. Classification Modelling

Seven machine-learning classification algorithms were compared:

- Logistic Regression
- Naive Bayes
- Support Vector Machine (SVM)
- Random Forest
- Gradient Boosting
- Decision Tree
- K-Nearest Neighbours (KNN)

Models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC

Preprocessing was implemented within machine-learning pipelines and fitted using the training data to reduce the risk of data leakage.

---

## Baseline Model Performance

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Random Forest | 0.8014 | 0.7342 | 0.7724 | 0.7528 | **0.8719** |
| Gradient Boosting | 0.7982 | 0.7305 | 0.7679 | 0.7487 | 0.8716 |
| SVM | **0.8035** | 0.7313 | **0.7875** | **0.7583** | 0.8682 |
| Logistic Regression | 0.8011 | 0.7298 | 0.7815 | 0.7547 | 0.8659 |
| Naive Bayes | 0.7439 | 0.6631 | 0.7031 | 0.6825 | 0.8142 |
| KNN | 0.7309 | 0.6633 | 0.6353 | 0.6490 | 0.7882 |
| Decision Tree | 0.7170 | 0.6415 | 0.6285 | 0.6349 | 0.7012 |

**Random Forest achieved the highest baseline ROC-AUC (0.8719)**, while **SVM achieved the highest accuracy (0.8035), recall (0.7875) and F1-score (0.7583)**.

The relatively small performance differences among Random Forest, Gradient Boosting, SVM and Logistic Regression indicate that several modelling approaches provided good discrimination.

---

## Hyperparameter Tuning

Random Forest, Gradient Boosting, SVM and Logistic Regression were selected for hyperparameter tuning using cross-validation.

Tuning produced only small changes in held-out test-set performance. The baseline models were therefore retained for final comparison rather than assuming that additional model complexity necessarily improved generalisation.

Among the tuned models, Random Forest achieved a ROC-AUC of **0.8726**, while SVM achieved the highest recall (**0.8320**) and F1-score (**0.7627**).

---

## Predictor Interpretation

Random Forest predictor importance was examined using two complementary approaches:

1. Impurity-based feature importance
2. Permutation importance on the held-out test set

Both approaches identified **weight-change intention (`WHQ030`) as the dominant predictor**.

In the permutation analysis, permuting `WHQ030` reduced test-set ROC-AUC by approximately **0.2377**, substantially more than any other individual predictor.

Other predictors contributing useful predictive information included:

- Race/ethnicity
- HDL cholesterol
- HbA1c
- Blood pressure
- Age
- Education
- Sex
- Diabetes status
- Income-to-poverty ratio

The strong contribution of weight-change intention should be interpreted cautiously because it may partly reflect participants' existing awareness of or response to their weight status.

Feature importance represents **predictive contribution within the fitted model and does not establish causality**.

---

## Key Findings

- Several machine-learning algorithms successfully discriminated between participants classified as obese and non-obese using predictors that excluded BMI and waist circumference.
- Random Forest achieved the highest baseline ROC-AUC (**0.8719**).
- SVM achieved the highest baseline accuracy, recall and F1-score.
- Hyperparameter tuning produced only modest changes in held-out test performance.
- Weight-change intention was the strongest predictor in the Random Forest model.
- Predictive information was distributed across behavioural, demographic, socioeconomic and clinical characteristics.
- Model performance and feature importance should not be interpreted as evidence of causal relationships.

---

## Repository Structure

```text
NHANES-Obesity-Classification/
│
├── notebooks/
│   ├── 01_data_collection_integration.ipynb
│   ├── 02_data_cleaning_preprocessing.ipynb
│   ├── 03_obesity_exploratory_data_analysis.ipynb
│   └── 04_classification_modelling.ipynb
│
├── outputs/
│   ├── classification_model_comparison.csv
│   ├── classification_tuning_results.csv
│   ├── classification_tuning_comparison.csv
│   ├── random_forest_feature_importance.csv
│   └── random_forest_permutation_importance.csv
│
├── README.md
├── requirements.txt
└── .gitignore