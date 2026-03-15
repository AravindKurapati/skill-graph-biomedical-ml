---
description: InceptionV3 classification on Barlow Twins SSL embeddings from
  TCGA lung cancer whole slide images. Use when working on LUAD vs LUSC
  classification, HPL framework, HPC multi-GPU training, or comparing SSL
  embedding approaches for computational pathology.
---

# TCGA lung cancer classification

Binary classification of lung cancer subtypes (LUAD vs LUSC) from H&E
WSIs using InceptionV3 trained on Barlow Twins SSL tile embeddings.
Built on NYU HPC cluster, extending the HPL framework.

## Clinical task

LUAD (Lung Adenocarcinoma) vs LUSC (Lung Squamous Cell Carcinoma).
Morphologically distinct subtypes with different treatment implications.
Standard H&E slide available for every patient, RNA-seq is not.

## HPL framework (prior work)

This project reproduces and extends the Histomorphological Phenotype
Learning framework (Quiros et al., Nature Communications 2024).

HPL pipeline:
```
WSI tiles
  -> Barlow Twins SSL -> 128D embeddings
  -> UMAP + Leiden clustering -> 46 HPCs
  -> slide HPC proportion vector (46-dim)
  -> Logistic Regression -> subtype prediction
```

HPL results on TCGA: AUC = 0.93. On NYU cohort: AUC = 0.99.
Interpretability via SHAP: identifies which HPCs drive predictions,
connecting back to real tissue biology (TILs, acinar structures, etc).

## This project's contribution

Rather than using HPC proportion vectors, feed raw 128D tile embeddings
directly into InceptionV3, treating each slide as a 224x224x128 tensor.
```
128D tile embeddings
  -> reconstruct slide as (224 x 224 x 128) tensor
  -> InceptionV3 (weights=None, input overridden to 224x224)
  -> GlobalAveragePooling -> Dense(512) -> Dropout(0.5) -> sigmoid
  -> LUAD vs LUSC prediction
```

Note on input: InceptionV3 canonical input is 299x299. Since
weights=None is used (ImageNet incompatible with 128 channels),
input shape is overridden to 224x224 to match the tile extraction size.

## Results vs HPL

| Approach | Classifier | AUC | Accuracy |
|----------|-----------|-----|----------|
| HPL (prior work) | Logistic Regression on HPC vectors | 0.93 | 99% |
| This work | InceptionV3 on raw embeddings | 0.65 | 78% |

The simpler cluster-based approach outperformed the deep model.

Why: HPL uses structured biology-grounded features that logistic
regression exploits cleanly. InceptionV3 trains from scratch on
embedding tensors that lack the spatial structure CNNs are designed
to exploit. Without ImageNet pretraining there is no useful weight
initialization.

Key insight: SSL embeddings encode rich morphological information but
lose the spatial structure CNNs depend on. End-to-end fine tuning of
the SSL backbone together with the classifier would likely close this gap.

## Dataset

- Source: TCGA LUAD and LUSC
- Format: preprocessed tiles in HDF5 format
- Tile embeddings: 128D vectors from pretrained Barlow Twins SSL
- Train tiles: 537,474 across 628 slides
- Validation tiles: 154,240 across 197 slides
- Test tiles: 149,265 across 197 slides
- Balanced label distribution across LUAD and LUSC

## HPC training specifics

- NYU HPC cluster, multi-GPU SLURM job scripting
- HDF5-based tile storage essential for random-access I/O at 850K+ tiles
- Custom SLURM job scripts with GPU and memory allocation to avoid preemption
- TFRecords used for efficient TensorFlow data pipeline at scale

## Pitfalls specific to this project

- Patient-level split is critical, tile-level split causes severe leakage
- Class imbalance at tile level affects convergence, upsampling minority class needed
- HDF5 random access patterns matter significantly at this scale
- InceptionV3 without pretraining needs careful learning rate scheduling

## Planned improvements

- End-to-end fine tuning of SSL backbone with classifier jointly
- Replace frozen embeddings with DINO or pathology foundation model
- Reproduce HPL clustering step fully for direct comparison

## Links

- [[whole-slide-imaging]] -> shared pipeline fundamentals
- [[follicular-lymphoma]] -> similar SSL + MIL approach, different disease
- [[model-selection]] -> HPL logistic regression vs deep model comparison
- [[eval-metrics]] -> AUC gap between HPL and this work explained
- [[imbalanced-data]] -> tile-level class imbalance handling