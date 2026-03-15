---
description: Handling class imbalance in medical ML datasets. Use when
  dealing with skewed class distributions, rare disease detection, or
  any biomedical classification task with unequal class sizes.
---

# Imbalanced data in medical ML

Class imbalance is the norm in medical datasets, not the exception.
Rare diseases are rare by definition. Fraud is rare by design.
Every node in this graph deals with it to some degree.

## Why it matters more in medical ML

A model that predicts "no cancer" for every patient achieves 99% accuracy
on a dataset where 1% have cancer. That model is clinically useless and
dangerous. Accuracy is not a valid metric here - see [[eval-metrics]].

## Severity by domain

- [[chest-xray]] - moderate imbalance, ~10-30% positive cases
- [[lung-cancer]] - severe, malignant nodules are rare
- [[parkinsons]] - mild, ~75% positive in UCI dataset
- [[fraud-detection]] - extreme, under 1% fraud cases
- [[dna-sequencing]] - varies by dataset and pathogen

## Techniques

### Resampling
- **SMOTE** (Synthetic Minority Oversampling) - generates synthetic minority
  samples. Apply to training set only, never to test set.
- **Random undersampling**  remove majority class samples. Fast but loses
  information. Use when dataset is large.
- **Combined** SMOTE + undersampling together often outperforms either alone.

### Class weights
- Most sklearn models accept `class_weight='balanced'` parameter
- XGBoost uses `scale_pos_weight = negative_cases / positive_cases`
- Always try class weights before resampling - simpler and often sufficient

### Threshold tuning
- Default 0.5 decision threshold is wrong for imbalanced data
- Lower threshold = higher recall, more false positives
- Higher threshold = higher precision, more false negatives
- Tune based on the cost of each error type in your specific clinical context

### Ensemble methods
- Balanced Random Forest - undersamples majority class per tree
- EasyEnsemble - trains multiple classifiers on balanced subsets
- Both outperform standard Random Forest on severely imbalanced data

## Critical rules

- Always split data BEFORE applying any resampling
- SMOTE on the full dataset before splitting = data leakage
- Evaluate on the original imbalanced test set, never on resampled test set
- Report per-class metrics, not just overall metrics

## Links

- [[eval-metrics]] - accuracy fails here, use these instead
- [[fraud-detection]] - most extreme imbalance case in this graph
- [[chest-xray]] - moderate imbalance, good starting point
- [[model-selection]] - some models handle imbalance better than others