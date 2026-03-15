---
description: Evaluation metrics for medical ML. Use when choosing how to
  measure model performance, reporting results, or comparing models on
  biomedical tasks. Accuracy is almost never the right metric here.
---

# Evaluation metrics in medical ML

Choosing the wrong metric is one of the most common mistakes in medical ML.
Accuracy looks good on paper and fails patients in practice.

## Why accuracy fails

Medical datasets are imbalanced - see [[imbalanced-data]].
A model predicting the majority class every time scores high accuracy.
That model has zero clinical utility.

## Core metrics

### AUC-ROC
- Area under the receiver operating characteristic curve
- Measures ability to discriminate between classes across all thresholds
- Threshold-independent - useful for comparing models
- Reliable primary metric for most binary medical classification tasks
- Limitation: can be optimistic on severely imbalanced data

### Precision and recall
- **Precision** - of all positive predictions, how many were correct
- **Recall (sensitivity)** - of all actual positives, how many were caught
- These trade off against each other - threshold tuning controls the balance
- In medical contexts, recall is usually more important - missing a disease
  is worse than a false alarm

### F1 score
- Harmonic mean of precision and recall
- Use when you need a single number that balances both
- Use F1 per class, not macro average on imbalanced datasets

### AUC-PR (Precision-Recall curve)
- More informative than AUC-ROC on severely imbalanced data
- Use this for [[fraud-detection]] and severe imbalance cases
- If positive class is rare, AUC-PR tells a more honest story

### MCC (Matthews Correlation Coefficient)
- Single metric that accounts for all four cells of confusion matrix
- Robust to class imbalance
- Standard in genomics - see [[dna-sequencing]]
- Range -1 to +1, where +1 is perfect, 0 is random

## Metric selection by domain

| Domain | Primary | Secondary |
|--------|---------|-----------|
| [[chest-xray]] | AUC-ROC | F1 per class |
| [[lung-cancer]] | AUC-ROC | Recall (missing cancer is costly) |
| [[dna-sequencing]] | MCC | AUC-ROC |
| [[parkinsons]] | AUC-ROC | F1 |
| [[fraud-detection]] | AUC-PR | Precision-Recall at threshold |

## Reporting results

- Always report confusion matrix alongside summary metrics
- Report metrics per class, not just overall
- State what threshold was used for precision/recall/F1
- On small datasets ([[parkinsons]]) - report cross-validation results,
  not single train/test split

## Links

- [[imbalanced-data]] - why these metrics matter
- [[model-selection]] - metric choice affects model selection decisions
- [[fraud-detection]] - AUC-PR over AUC-ROC for extreme imbalance
- [[dna-sequencing]] - MCC standard in genomics tasks