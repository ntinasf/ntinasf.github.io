---
layout: page
title: Credit Risk Classification
subtitle: A Cost-Sensitive Credit Risk Ensemble on the German Credit Dataset
---

## Project summary

This project explores how a machine learning system can support credit approval decisions when the business cost of a bad approval is significantly higher than the cost of rejecting a good applicant. For this reason, we built an end-to-end credit risk pipeline around the [German Credit Dataset](https://archive.ics.uci.edu/dataset/144/statlog+german+credit+data), combining data cleaning, custom feature engineering, model-specific preprocessing, hyperparameter tuning, decision threshold optimization, experiment tracking, and deployment through a Streamlit app.

The final system uses four complementary classifiers — Logistic Regression, Random Forest, Support Vector Classifier, and CatBoost — combined into a weighted soft-voting ensemble. The goal was not only to improve predictive performance, but also to design a workflow that reflects the realities of risk-sensitive decision making: asymmetric error costs, small-sample uncertainty, and the need for traceable experimentation.

[Try the Live Demo](https://credit-risk-h6wzqyepauzgpp29kypx9e.streamlit.app){: .btn .btn-primary} · [View Repository](https://github.com/ntinasf/credit-risk){: .btn .btn-info}

---

## The problem

Credit approval is a classification problem, but not a symmetric one. The cost of approving a high-risk borrower is five times higher than the cost of rejecting a potentially good one. Because of that, a plain accuracy-driven approach would have been misleading. Instead, the project was framed around a cost-sensitive objective, where false positives carry a substantially larger penalty than false negatives.

*Decision cost according to the dataset's documentation:*

| Error | Decision | Cost |
| --- | --- | ---: |
| False Positive | Approve a bad borrower | 5 |
| False Negative | Reject a good borrower | 1 |

This framing influenced the entire workflow:
- model selection and tuning
- decision threshold optimization
- ensemble weighting

---

## Dataset and preprocessing decisions

The project uses the German Credit Dataset, which is far from ideal in a modern business setting. The dataset is relatively small, somewhat outdated, and includes several variables whose observed relationships are not always intuitive from a real-world lending perspective. That made data preparation especially challenging.

A custom preprocessing script was used to convert the raw dataset into a more readable and analysis-friendly structure based on the original dataset documentation. After cleaning, the data was split into train and test sets using a hash-based strategy to ensure reproducibility.

---

## Feature engineering strategy

Feature engineering became one of the most important parts of the project. After exploratory analysis and light domain research, a reusable `FeatureEngineer` transformer was built to create domain-informed features and apply meaningful transformations before encoding.

The engineered features include:
- a monthly repayment burden feature
- duration-to-age relationships
- log transforms for skewed numeric variables
- age-group binning
- category consolidation for sparse levels
- indicator variables such as `no_checking`
- optional duplicate or polynomial-style features for model-specific benefit

A key design decision was to avoid forcing every model through the exact same feature preprocessing path. Instead, each model goes through its own tailored pipeline, allowing it to operate on the feature representation that suits it best.

- [See the feature engineering exploration and validation here](https://github.com/ntinasf/credit-risk/blob/main/notebooks/feature_engineering.ipynb)

---

## Modeling approach

Rather than relying on one model family, the system combines four complementary classifiers to capture distinct signal types in the dataset:
- **Logistic Regression** as a strong linear baseline
- **Random Forest** for nonlinear structure and interaction effects
- **Support Vector Classifier** for margin-based separation
- **CatBoost** for boosted-tree performance

Beyond the tailored preprocessing mentioned earlier, two different approaches were used to address the imbalance of the target variable:
- Logistic Regression and SVC used oversampling techniques such as SMOTE or SVMSMOTE
- Random Forest and CatBoost used cost-sensitive weighting instead of oversampling.

Then, each model's hyperparameters were tuned using a Bayesian approach (Gaussian Processes), with the purpose of maximizing ROC AUC through 8-fold cross-validation. This ensured each model was a strong standalone classifier.

---

### Why an ensemble

The strongest individual model is not necessarily the best final system. Since each algorithm captured different structure in the data, a weighted soft-voting ensemble was used to combine them.

The ensemble weights were chosen through a validation-based sweep that ranked the top-15 weight combinations based on ROC AUC first and then precision, reflecting the business goal of reducing costly false approvals. The best-performing ensemble was then threshold-tuned to minimize the custom cost function.

## Results

The ensemble achieved test-set performance close to its validation performance, which is encouraging given the limited dataset size. More importantly, it outperformed the individual models overall, suggesting that each one contributed complementary information.

- [See the analysis and evaluation workflow here](https://github.com/ntinasf/credit-risk/blob/main/notebooks/investigation.ipynb)

### Final ensemble metrics

| Metric | Test | Notes |
| --- | ---: | --- |
| ROC AUC | 0.82 | Main ranking metric |
| Average Precision | 0.89 | Useful under class imbalance |
| Precision | 0.92 | Important due to false approval cost |
| Recall | 0.56 | Shows how many good borrowers were captured |
| F1 Score | 0.70 | Balance of precision and recall |
| Accuracy | 0.68 | Report, but do not overemphasize |
| Total Cost | 71 | Based on project cost matrix |
| Average Cost per Applicant | 0.44 | Best business-facing summary |
| Decision Threshold | 0.84 | Final production threshold |

### Model comparison table

| Model | ROC AUC | Average Precision | Precision | Recall | Accuracy | Average Cost |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| CatBoost | 0.808 | 0.888 | 0.888 | 0.670 | 0.723 | 0.503 |
| Logistic Regression | 0.813 | 0.888 | 0.907 | 0.462 | 0.610 | 0.516 |
| Random Forest | 0.814 | 0.891 | 0.909 | 0.566 | 0.673 | 0.478 |
| SVC | 0.816 | 0.886 | 0.861 | 0.698 | 0.723 | 0.579 |
| Ensemble | 0.822 | 0.893 | 0.923 | 0.566 | 0.679 | 0.447 |

---

## What this project demonstrates

This project reflects the full lifecycle of applied machine learning:
- translating a business objective into a modeling objective
- designing modular preprocessing for multiple model families
- working carefully with a small, imperfect dataset
- using experiment tracking to keep training decisions reproducible
- deploying the final system in an interactive application

## Limitations and next steps

The main limitation is the dataset itself. With only around one thousand records, model comparisons can become unstable, and strong conclusions about generalization should be made carefully. In addition, some variable relationships in the dataset are difficult to interpret from a modern lending standpoint, which limits how confidently one can generalize feature importance findings.

