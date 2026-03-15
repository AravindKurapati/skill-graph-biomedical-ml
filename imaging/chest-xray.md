---
description: CNN approaches for chest X-ray classification. Use when working
  on medical imaging, binary or multi-class classification, pneumonia or
  COVID detection tasks.
---

# Chest X-ray classification

Binary and multi-class classification on X-ray images. Severe class imbalance
is the norm in medical datasets. see [[imbalanced-data]] for handling strategies
before training.

## Architecture

Standard CNN backbones work well. ResNet50 and DenseNet121 are the most
validated for chest X-ray tasks. Transfer learning from ImageNet gives a
strong starting point even though the domain is different.

## Preprocessing

- Normalize pixel values to [0, 1] or use ImageNet mean/std if transfer learning
- Resize to 224x224 minimum
- Augmentation: horizontal flip, slight rotation (±10°), brightness adjustment
- Do NOT use aggressive augmentation. medical images have clinical meaning

## Evaluation

Accuracy is misleading on imbalanced medical data. Use AUC-ROC as primary
metric, F1 per class as secondary. See [[eval-metrics]] for full breakdown.

## Pitfalls

- Model latches onto non-clinical features (scanner type, patient positioning)
- Overfit risk is high with small medical datasets. use dropout + weight decay
- Always check per-class performance, not just overall metrics

## Links

- [[lung-cancer]] - similar CNN pipeline, different label structure
- [[imbalanced-data]] - critical for this domain
- [[eval-metrics]] - AUC-ROC, F1, precision-recall tradeoffs
- [[model-selection]] - when to use CNN vs ViT for imaging tasks