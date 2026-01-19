---
layout: page
title: Credit Risk Classification
subtitle: Ensemble Machine Learning Model with Interactive Demo
---

A machine learning system that predicts credit risk for loan applicants using an ensemble of three models combined through soft voting.

**[ Try the Live Demo](https://credit-risk-svm-hokxpyoex9pcn95vardjas.streamlit.app){: .btn .btn-primary}** · **[View Code on GitHub](https://github.com/ntinasf/credit-risk-svm){: .btn .btn-info}**

---

## Project Overview

Financial institutions need reliable methods to assess credit risk before approving loans. This project builds an ensemble classifier that combines the strengths of three different algorithms to produce more robust predictions than any single model.

### The Ensemble Approach

| Model | Strengths | Role in Ensemble |
|-------|-----------|------------------|
| **Logistic Regression** | Interpretable, stable probabilities | Baseline predictions |
| **Random Forest** | Handles non-linear relationships | Captures complex patterns |
| **Support Vector Classifier** | Strong with high-dimensional data | Decision boundary refinement |

The final prediction uses **weighted soft voting**, combining probability estimates from all three models based on their individual performance characteristics.

---

## Interactive Demo

The Streamlit application allows you to:

- **Test with random samples** from the dataset to see how the model performs
- **Input custom values** to get predictions for hypothetical applicants
- **View model breakdown** showing how each algorithm contributed to the final decision

<iframe src="(https://credit-risk-svm-hokxpyoex9pcn95vardjas.streamlit.app)" width="100%" height="700" frameborder="0"></iframe>

---

## Technical Approach

### Feature Engineering

The `FeatureEngineer` transformer creates derived features that improve model performance:

- **Monthly Burden:** `credit_amount / duration_months` — captures payment intensity
- **Duration-to-Age Ratio:** Relative loan length compared to borrower's age
- **Category Consolidation:** Merging sparse categories for better generalization
- **Log Transformations:** Normalizing skewed distributions

### Preprocessing Pipelines

Each model uses a tailored encoding strategy optimized for its characteristics:

| Model | Encoding Strategy |
|-------|-------------------|
| Logistic Regression | WOE + Count + One-Hot |
| Random Forest | Ordinal + Target + One-Hot |
| SVC | WOE + Target + One-Hot |

### Class Imbalance Handling

The dataset has ~70% good credit vs ~30% bad credit. Addressed through:

- **SMOTE/SVMSMOTE** for synthetic minority oversampling
- **Cost-sensitive learning** with custom class weights
- **Threshold tuning** using `TunedThresholdClassifierCV`

### Cost Function

The model optimizes for a business-realistic cost function:

| Error Type | Cost | Rationale |
|------------|------|-----------|
| False Negative (reject good customer) | 1 | Lost business opportunity |
| False Positive (accept bad customer) | 5 | Potential default loss |

---

## Model Performance

| Metric | Ensemble | Best Individual |
|--------|----------|-----------------|
| ROC AUC | **~0.79** | ~0.78 (SVC) |
| Average Cost | **~0.43** | ~0.45 |
| Accuracy | ~0.75 | ~0.74 |

The ensemble consistently outperforms individual models, demonstrating the value of combining diverse algorithms.

---

## Experiment Tracking

All training runs were logged using **MLflow**, tracking:

- Hyperparameters and tuning results
- Performance metrics across validation folds
- Learning curves for bias-variance diagnosis
- Confusion matrices and precision-recall curves

---

## Dataset

**German Credit Dataset** from UCI Machine Learning Repository

- **Samples:** 1,000 loan applicants
- **Features:** 20 attributes (7 numerical, 13 categorical)
- **Target:** Binary classification (Good/Bad credit risk)

---

## Tools & Technologies

**Languages & Libraries:** Python, Pandas, NumPy, Scikit-learn, Imbalanced-learn

**Experiment Tracking:** MLflow

**Deployment:** Streamlit Cloud

**Version Control:** Git, GitHub

---

## Repository Structure
```
credit-risk-svm/
├── notebooks/           # EDA and experimentation
├── scripts/             # Training pipelines
├── streamlit_app/       # Interactive demo
├── data/                # Raw and processed data
└── mlruns/              # MLflow artifacts
```

**[View Full Repository](https://github.com/ntinasf/credit-risk-svm)**
