---
layout: page
title: Credit Risk Classification
subtitle: Ensemble Machine Learning for Loan Default Prediction
---

This project builds a production-ready credit risk classifier using the [German Credit Dataset](https://archive.ics.uci.edu/dataset/144/statlog+german+credit+data). It demonstrates end-to-end machine learning workflow including custom feature engineering, multiple encoding strategies, class imbalance handling, and cost-sensitive optimization — culminating in an ensemble model deployed as an interactive web application.

<p style="margin: 1.5rem 0;">
  <a href="https://credit-risk-svm-hokxpyoex9pcn95vardjas.streamlit.app" class="btn btn-primary" target="_blank">🚀 Try the Live Demo</a>
  <a href="https://github.com/ntinasf/credit-risk-svm" class="btn btn-secondary" target="_blank">📂 View Repository</a>
</p>

---

## Business Context

When a lending institution receives a loan application, it must decide whether to approve based on the applicant's profile. Lending to high-risk applicants is the largest source of financial loss for lenders. This project addresses that challenge by predicting credit risk (good/bad) for loan applicants using an ensemble of three machine learning models.

**The key insight:** Not all prediction errors are equal. Approving a customer who defaults (false positive) costs significantly more than rejecting a creditworthy customer (false negative). The model is optimized for this asymmetric cost structure.

---

## Technical Approach

### The Ensemble Architecture

The final model uses **soft voting** across three classifiers, each with its own preprocessing pipeline optimized for its characteristics:

| Model | Encoding Strategy | Imbalance Handling |
|-------|-------------------|-------------------|
| **Support Vector Classifier** | WOE + Target + Count + One-Hot | SVMSMOTE |
| **Logistic Regression** | WOE + Count + One-Hot | SMOTE |
| **Random Forest** | One-Hot + Count + Target | Cost-sensitive weights |

The ensemble combines predictions with learned weights [2.5, 1.5, 3.0] and uses a tuned decision threshold of 0.63.

### Cost Function Optimization

Following the dataset's documentation, the model optimizes for a business-realistic cost function:

- **False Positive** (approve a defaulter): **5 cost units** — represents potential default losses
- **False Negative** (reject a good customer): **1 cost unit** — represents lost business opportunity

This 5:1 cost ratio drives all threshold tuning and model selection decisions.

### Custom Feature Engineering

The `FeatureEngineer` transformer applies domain-specific transformations:

- **Monthly Burden:** `credit_amount / duration_months` (log-transformed) — captures payment intensity
- **Duration-to-Age Ratio:** Loan duration relative to applicant age — captures lifecycle appropriateness  
- **Age Groups:** Young, Early Career, Prime, Mature — categorical binning for non-linear effects
- **Credit Amount Bins:** Quintile-based categorization
- **Category Consolidation:** Merging sparse categories (e.g., rare job types, housing situations) for better generalization
- **No-Checking Flag:** Binary indicator for applicants without checking accounts — a strong risk signal

---

## Model Performance

After threshold tuning for the cost function:

| Model | ROC AUC | Average Cost |
|-------|---------|--------------|
| SVC (SVMSMOTE) | ~0.81 | ~0.43 |
| Logistic Regression | ~0.80 | ~0.49 |
| Random Forest | ~0.82 | ~0.53 |
| **Ensemble** | **~0.79** | **~0.43** |

The ensemble achieves the lowest average cost while maintaining competitive AUC, demonstrating the value of combining diverse models for robust predictions.

---

## Interactive Demo

The Streamlit application below allows you to test the model with real data:

- **Sample from test set:** Load actual applicant profiles (with known outcomes) to validate predictions
- **Manual input:** Enter custom applicant information to see how the model responds

<iframe src="https://credit-risk-svm-hokxpyoex9pcn95vardjas.streamlit.app?embedded=true" 
  width="100%" height="700" frameborder="0" style="border-radius: 8px; margin: 1rem 0;"></iframe>

---

## Experiment Tracking with MLflow

All model training is tracked using MLflow, enabling:

- **Hyperparameter logging** for reproducibility
- **Metric comparison** across runs (ROC AUC, precision, recall, cost)
- **Artifact storage** including learning curves, confusion matrices, and PR curves
- **Model versioning** for the production ensemble

---

## Dataset Overview

- **Source:** UCI Machine Learning Repository — German Credit Data
- **Samples:** 1,000 loan applicants
- **Features:** 20 attributes (7 numerical, 13 categorical)
- **Target:** Binary classification (Good=0, Bad=1)
- **Class Distribution:** ~70% Good, ~30% Bad

---

## Key Takeaways

1. **Cost-sensitive optimization** dramatically changes model behavior compared to accuracy-focused training
2. **Ensemble methods** provide more stable predictions than individual models when costs are asymmetric
3. **Feature engineering** with domain knowledge (monthly burden, age groups) improves both performance and interpretability
4. **Multiple encoding strategies** work better when tailored to each model's characteristics

---

## Tools & Technologies

<div style="display: flex; flex-wrap: wrap; gap: 0.5rem; margin: 1rem 0;">
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">Python</span>
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">scikit-learn</span>
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">imbalanced-learn</span>
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">category_encoders</span>
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">MLflow</span>
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">Streamlit</span>
  <span style="background: var(--light-bg); padding: 0.3rem 0.8rem; border-radius: 4px; font-size: 0.9rem;">joblib</span>
</div>

---

<p style="text-align: center; margin-top: 2rem;">
  <a href="https://github.com/ntinasf/credit-risk-svm" target="_blank">View Full Repository →</a>
</p>
