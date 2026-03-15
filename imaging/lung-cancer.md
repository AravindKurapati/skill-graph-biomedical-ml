---
description: ML approaches for lung cancer detection and classification.
  Use when working on CT scan analysis, nodule detection, or cancer
  staging classification tasks.
---

# Lung cancer detection

Lung cancer classification from CT scans and histopathology images.
Pipeline is similar to [[chest-xray]] but input data and label complexity
differ significantly.

## Key differences from chest X-ray

- CT scans are 3D. either slice as 2D or use 3D CNN architectures
- Nodule detection is an object detection problem, not pure classification
- Dataset sizes tend to be even smaller. transfer learning is essential
- Labels are more granular (malignant/benign + staging)

## Architecture

- 2D approach: treat CT slices as images, same ResNet/DenseNet as [[chest-xray]]
- 3D approach: 3D CNN or inflated convolutions. higher compute cost
- For nodule detection: Faster R-CNN or YOLO adapted for medical imaging

## Preprocessing

- HU (Hounsfield Unit) windowing - lung window [-1000, 400] range
- Normalize after windowing, not before
- Slice thickness matters. resample to consistent spacing if mixed datasets

## Evaluation

Small datasets + class imbalance = unreliable accuracy. See [[eval-metrics]]
for why AUC-ROC and per-class F1 matter here even more than in [[chest-xray]].

## Pitfalls

- 3D models overfit easily on small datasets. start with 2D slices
- Scanner variability across hospitals causes distribution shift
- Staging labels are often noisy. consider label smoothing

## Links

- [[chest-xray]] - shared CNN pipeline fundamentals
- [[imbalanced-data]] - malignant cases are rare, imbalance is severe
- [[eval-metrics]] - staging is multi-class, metrics change accordingly
- [[model-selection]] - 2D vs 3D CNN tradeoff discussion