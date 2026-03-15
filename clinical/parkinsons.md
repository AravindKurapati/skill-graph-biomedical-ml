---
description: ML approaches for Parkinson's disease detection and progression
  prediction from clinical tabular data. Use when working on biomedical
  tabular data, hyperparameter tuning, or clinical classification tasks.
---

# Parkinson's disease classification

Binary classification from clinical tabular data - detecting Parkinson's
from biomedical voice measurements or other clinical features.

## Data characteristics

- Tabular format - no images, no sequences, just numerical clinical features
- Small dataset - UCI Parkinson's dataset has ~200 samples
- Class imbalance - roughly 75% positive cases - see [[imbalanced-data]]
- Features are biomedical voice measurements (jitter, shimmer, HNR, RPDE)

## Model approaches

- **Start with tree-based models**  Random Forest, XGBoost. Work well on
  small tabular datasets, interpretable, robust to feature scaling
- **SVM**  strong baseline for small datasets with clear margins
- **Logistic Regression**  always run as interpretable baseline
- **Neural networks**  rarely justified at this dataset size, overfit easily

## Hyperparameter tuning

Small dataset means tuning matters more than architecture choice.
Key parameters to tune:
- Tree depth and number of estimators (Random Forest/XGBoost)
- Regularization strength (SVM, Logistic Regression)
- Class weight parameter - critical given imbalance

Use cross-validation, not a single train/test split at this dataset size.
See [[model-selection]] for tuning strategy tradeoffs.

## Feature importance

Tree-based models give feature importance for free - use it.
Helps identify which voice measurements are most predictive.
Useful for clinical interpretability - doctors need to understand predictions.

## Evaluation

Do not use accuracy - 75% positive rate means a naive classifier scores
high. Use AUC-ROC, F1, and confusion matrix. See [[eval-metrics]].

## Pitfalls

- Small dataset + many features = overfitting risk - use cross-validation
- Feature leakage is common in clinical data - check for derived features
- Clinical interpretability matters - black box models are harder to deploy
  in medical settings

## Links

- [[fraud-detection]] - shares tabular ML pipeline, different domain
- [[imbalanced-data]] - class imbalance present, needs explicit handling
- [[eval-metrics]] - accuracy misleading, AUC-ROC and F1 preferred
- [[model-selection]] - tree-based vs neural network tradeoff for small tabular data