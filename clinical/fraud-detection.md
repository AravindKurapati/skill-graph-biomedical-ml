---
description: ML approaches for healthcare fraud detection. Use when working
  on anomaly detection, highly imbalanced classification, or financial
  and claims data in medical contexts.
---

# Healthcare fraud detection

Fraud detection in medical claims data - identifying fraudulent billing,
unnecessary procedures, or provider fraud from transactional records.

## Data characteristics

- Tabular format - claims records, billing codes, provider info
- Extreme class imbalance - fraud cases are typically under 1% of data
  - see [[imbalanced-data]], this is the most severe case in this graph
- High dimensional - many billing codes, procedure types, provider features
- Temporal structure - claim sequences per provider matter

## Model approaches

- **XGBoost/LightGBM** - best starting point, handles imbalance well
  with scale_pos_weight parameter, fast on large datasets
- **Isolation Forest** - unsupervised anomaly detection, useful when
  labeled fraud cases are scarce
- **Logistic Regression** - strong interpretable baseline, required
  for regulatory contexts
- **Neural networks** - only justified on very large claims datasets

## Handling extreme imbalance

Fraud datasets are more imbalanced than any other node in this graph.
Standard approaches from [[imbalanced-data]] apply but need more
aggressive settings:
- SMOTE oversampling on training set only, never on test
- Higher class weights than typical imbalanced datasets
- Threshold tuning is critical - default 0.5 threshold will miss most fraud

## Evaluation

Accuracy is completely useless here - 99% accuracy by predicting no fraud.
Precision-recall tradeoff is the core concern - catching fraud vs false
accusations. See [[eval-metrics]] for precision-recall curve and
threshold selection guidance.

## Pitfalls

- Concept drift - fraud patterns change over time, models go stale fast
- Temporal leakage - never shuffle time-series claims data randomly
- False positives have real consequences - denying legitimate claims
  harms patients
- Regulatory requirements may mandate explainability - tree-based or
  logistic regression over black box models

## Links

- [[parkinsons]] - shares tabular ML pipeline
- [[imbalanced-data]] - extreme imbalance, most critical link in this node
- [[eval-metrics]] - precision-recall tradeoff, threshold tuning
- [[model-selection]] - anomaly detection vs supervised classification tradeoff