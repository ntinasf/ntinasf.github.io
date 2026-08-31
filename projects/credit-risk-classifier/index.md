---
layout: page
title: Credit Risk Classification
subtitle: A Cost-Sensitive Credit Risk Ensemble on the German Credit Dataset
---

## Project summary

This project explores how a machine learning system can support credit approval decisions when the business cost of a bad approval is significantly higher than the cost of rejecting a good applicant. For this reason it is built as an end-to-end credit risk pipeline around the [German Credit Dataset](https://archive.ics.uci.edu/dataset/144/statlog+german+credit+data), combining data cleaning, custom feature engineering, model-specific preprocessing, hyperparameter tuning, decision threshold optimization, experiment tracking, and deployment through a Streamlit app.

The final system combines four classifiers (Logistic Regression, Random Forest, Support Vector Classifier, and CatBoost) into a weighted soft-voting ensemble. Predictive performance was one goal. The other was a workflow that reflects how risk-sensitive decisions actually get made, with asymmetric error costs, small-sample uncertainty, and traceable experimentation.

[Try the Live Demo](https://credit-risk-h6wzqyepauzgpp29kypx9e.streamlit.app){: .btn .btn-primary} · [View Repository](https://github.com/ntinasf/credit-risk){: .btn .btn-info}

---

## The problem

Credit approval is a classification problem with asymmetric error costs. Approving a high-risk borrower costs five times as much as rejecting a potentially good one. A plain accuracy-driven approach would therefore be misleading, so the project was framed around a cost-sensitive objective where false positives carry the larger penalty.

*Decision cost according to the dataset's documentation:*

| Error | Decision | Cost |
| --- | --- | ---: |
| False Positive | Approve a bad borrower | 5 |
| False Negative | Reject a good borrower | 1 |

This framing influenced the entire workflow:
- model selection and tuning
- decision threshold optimization
- ensemble weighting

One design decision follows from it. The cost matrix is applied in a single place, when the decision threshold is chosen on a held-out validation set. Model fitting optimizes ROC AUC, and class weights handle imbalance only. Encoding the 5:1 ratio in both the class weights and the threshold would count it twice.

---

## Dataset and preprocessing decisions

The project uses the German Credit Dataset, which is far from ideal in a modern business setting. The dataset is relatively small, somewhat outdated, and includes several variables whose observed relationships are not always intuitive from a real-world lending perspective. That made data preparation especially challenging.

A custom preprocessing script was used to convert the raw dataset into a more readable and analysis-friendly structure based on the original dataset documentation. After cleaning, the data was split into train and test sets using a hash-based strategy to ensure reproducibility. The 841 training rows were split again into 691 rows for fitting and 150 for validation, and every stage of the pipeline reads that same split so the partition cannot drift between training, ensemble selection, and the notebook.

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

A key design decision was to avoid forcing every model through the exact same feature preprocessing path. Instead, each model goes through its own tailored pipeline, allowing it to operate on the feature representation that suits it best. Depending on the model, the encoders include one-hot, weight of evidence, target, count, and ordinal encoding, plus CatBoost's native categorical handling.

- [See the feature engineering exploration and validation here](https://github.com/ntinasf/credit-risk/blob/main/notebooks/feature_engineering.ipynb)

---

## Modeling approach

Rather than relying on one model family, the system combines four complementary classifiers to capture distinct signal types in the dataset:
- **Logistic Regression** as a strong linear baseline
- **Random Forest** for nonlinear structure and interaction effects
- **Support Vector Classifier** for margin-based separation
- **CatBoost** for boosted-tree performance

Beyond the tailored preprocessing mentioned earlier, bad borrowers make up roughly 29% of the training rows, and each model handles that imbalance in the way that suits it:

| Model | Strategy |
| --- | --- |
| Logistic Regression | `SMOTE` inside the pipeline |
| Support Vector Classifier | `SVMSMOTE` inside the pipeline |
| Random Forest | `class_weight="balanced"` |
| CatBoost | `scale_pos_weight` set to the inverse class ratio |

The resampling steps sit inside the pipeline, so they are refit within each cross-validation fold rather than before the split. None of these strategies encode the cost matrix.

Then, each model's hyperparameters were tuned using a Bayesian approach (Gaussian Processes), with the purpose of maximizing ROC AUC through 8-fold cross-validation. This ensured each model was a strong standalone classifier.

---

### Why an ensemble

Each algorithm captured different structure in the data, so a weighted soft-voting ensemble was used to combine them.

The weights and the decision threshold were chosen on the validation set, never on the test set. A sweep over 256 weight combinations shortlisted the top 15 by ROC AUC, which is threshold-independent and therefore the right metric while the threshold is still unknown. Each of those 15 candidates then had its threshold tuned to minimize the cost function, and the lowest-cost candidate was promoted. That promoted configuration was scored once against the hold-out test set.

The selection runs as a script rather than by hand, so the shipped weights and threshold are reproducible: re-running it produces the same configuration.

## Results

The ensemble achieved test-set performance close to its validation performance, which is encouraging given the limited dataset size.

- [See the analysis and evaluation workflow here](https://github.com/ntinasf/credit-risk/blob/main/notebooks/investigation.ipynb)

### Final ensemble metrics

| Metric | Test | Notes |
| --- | ---: | --- |
| ROC AUC | 0.82 | Main ranking metric |
| Average Precision | 0.89 | Useful under class imbalance |
| Precision | 0.90 | Important due to false approval cost |
| Recall | 0.61 | Shows how many good borrowers were captured |
| F1 Score | 0.73 | Balance of precision and recall |
| Accuracy | 0.70 | Report, but do not overemphasize |
| Total Cost | 76 | Based on project cost matrix |
| Average Cost per Applicant | 0.48 | Best business-facing summary |
| Decision Threshold | 0.66 | Final production threshold |

![Confusion matrix for the final ensemble on the test set](/assets/images/credit-risk-classifier/confusion_matrix.png)
*Confusion matrix for the ensemble on the test set.*

### Model comparison table

Each model is scored on the test set at its own validation-tuned threshold.

| Model | ROC AUC | Average Precision | Precision | Recall | Accuracy | Average Cost |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| CatBoost | 0.808 | 0.890 | 0.850 | **0.642** | 0.686 | 0.616 |
| Logistic Regression | 0.813 | 0.888 | 0.907 | 0.462 | 0.610 | 0.516 |
| Random Forest | 0.803 | 0.885 | 0.906 | 0.453 | 0.604 | 0.522 |
| SVC | 0.817 | 0.893 | **0.910** | 0.575 | 0.679 | **0.472** |
| Ensemble | **0.823** | **0.895** | 0.903 | 0.613 | **0.698** | 0.478 |

### Reading these numbers honestly

The ensemble leads on the ranking metrics, ROC AUC and average precision, and on accuracy. On the metric the project actually optimizes it does not win: the ensemble costs 76 against SVC's 75. The two are effectively tied.

The ensemble is still the configuration that ships, because it is more stable across the validation candidates than any single model. But this dataset does not demonstrate an ensemble advantage, and it would be wrong to claim one.

Bootstrapping the test set with 5,000 resamples puts the ensemble's cost at 76 with a 95% interval of [51, 104]. Its advantage over simply rejecting every applicant is 30.1, with an interval of [-2, 58], which only just fails to exclude zero. So the model is probably better than doing nothing, but 159 test rows cannot establish it at 95% confidence, and any comparison between configurations differing by less than about 40 cost units is reading noise.

### What the threshold actually does

A 5:1 cost ratio buys precision by refusing business. At the production threshold the system approves 45.3% of applicants. It turns away 41 of the 106 creditworthy applicants in order to avoid 46 of the 53 bad loans. Whether that trade is worth making depends on the margin per good loan against the loss given default, and the 5:1 ratio itself comes from the UCI documentation rather than from any measurement here.

Accuracy deserves a caveat too. Approving every applicant scores 66.7% on this class balance, so an accuracy of 0.70 at a cost-optimal threshold is expected rather than impressive. Cost is the metric worth reading.

---

## What this project demonstrates

This project reflects the full lifecycle of applied machine learning:
- translating a business objective into a modeling objective
- designing modular preprocessing for multiple model families
- working carefully with a small, imperfect dataset
- using experiment tracking to keep training decisions reproducible
- deploying the final system in an interactive application

## Limitations

The main limitation is the dataset itself. With only around one thousand records, model comparisons can become unstable, and strong conclusions about generalization should be made carefully. The bootstrap intervals above make that concrete: the gap between the ensemble and the best single model is far smaller than the uncertainty around either number. In addition, some variable relationships in the dataset are difficult to interpret from a modern lending standpoint, which limits how confidently one can generalize feature importance findings.
