---
description: Shared WSI pipeline concepts for computational pathology.
  Use when working on patch extraction, tissue filtering, self-supervised
  pretraining on histology patches, or multiple instance learning for
  slide-level classification.
---

# Whole slide image pipeline

WSIs are gigapixel images that cannot be processed as a single input.
The standard approach breaks them into patches, encodes each patch,
then aggregates patch representations into a slide-level prediction.

## Standard pipeline
```
WSI (.svs)
  -> patch extraction (224x224 tiles)
  -> tissue filtering (remove background and artifacts)
  -> patch encoding (SSL or pretrained backbone)
  -> slide-level aggregation (MIL or pooling)
  -> slide-level prediction
```

## Patch extraction

- Extract non-overlapping 224x224 patches at level 0 (full resolution)
- Store (x, y) coordinates alongside each patch for spatial reconstruction
- Up to 800-1000 tissue patches per slide is typical

## Tissue filtering

Critical step. Background patches waste compute and hurt model quality.

Grayscale thresholding: simple and fast but misses subtle artifacts.
Known to produce noisy patch bags.

HSV saturation + Laplacian blur check: better approach
- Saturation threshold separates tissue (colorful) from glass (gray/white)
- Laplacian variance catches blurry or out-of-focus patches
- Always prefer this over grayscale thresholding

## Self-supervised pretraining on patches

ImageNet pretraining is suboptimal for H&E histology. The domain gap
is large. SSL pretraining on your own patches adapts the encoder to
H&E stain characteristics and tissue morphology without any labels.

Barlow Twins is the SSL method used across both projects here:
- Two augmented views of each patch passed through backbone
- Loss pushes diagonal of cross-correlation matrix toward 1 (invariance)
- Loss pushes off-diagonal toward 0 (decorrelation)

Augmentation pipeline for H&E patches:
- Random resized crop
- Horizontal and vertical flip
- Color jitter (moderate, not aggressive)
- Gaussian blur
- Avoid aggressive color distortion, H&E stain carries clinical meaning

## Multiple instance learning

MIL treats each slide as a bag of patch instances. The bag label is
known but individual patch labels are not. Standard setup for WSI
classification because annotating hundreds of patches per slide is
infeasible.

Gated Attention MIL:
```
V(h) = Tanh(W_V * h)       content projection
U(h) = Sigmoid(W_U * h)    gating
A = Softmax(w * (V * U))   per-patch attention weights
z = sum(A_i * h_i)          weighted slide representation
logits = Dropout -> Linear(z)
```

Attention weights reveal which patches drove the prediction.
Top-k attention patch visualization gives interpretability needed
for clinical applications.

## Pathology foundation models

Drop-in replacements for generic ImageNet encoders:
- CONCH: github.com/mahmoodlab/CONCH
- UNI: github.com/mahmoodlab/UNI
- PLIP: github.com/PathologyFoundation/plip

All pretrained on large pathology datasets. Expected to outperform
both ImageNet ViT and small in-domain SSL on institutional datasets.

## Pitfalls

- Never split at patch level, always split at slide level to avoid leakage
- Scanner variability across institutions causes distribution shift
- Tissue mask quality directly affects downstream model performance
- Save SSL checkpoints periodically, cloud and HPC sessions end unexpectedly

## Links

- [[follicular-lymphoma]] -> FL subtype prediction using this pipeline
- [[tcga-lung-classification]] -> LUAD vs LUSC classification using this pipeline
- [[model-selection]] -> encoder choice for WSI tasks
- [[eval-metrics]] -> AUC over accuracy for slide-level predictions
- [[imbalanced-data]] -> class imbalance strategies for small WSI datasets