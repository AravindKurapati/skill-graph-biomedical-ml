---
description: Model selection tradeoffs for biomedical ML tasks. Use when
  choosing between classical ML and deep learning, comparing architectures,
  or deciding how to approach a new biomedical ML problem.
---

# Model selection in biomedical ML

The best model is almost never the most complex one. Medical datasets are
small, interpretability matters, and overfitting is the primary risk.

## The core tradeoff

| Factor | Favors classical ML | Favors deep learning |
|--------|--------------------|--------------------|
| Dataset size | Small (<10k samples) | Large (>100k samples) |
| Input type | Tabular, engineered features | Images, sequences, raw signals |
| Interpretability need | High (clinical deployment) | Low (research) |
| Compute budget | Limited | Available |
| Baseline needed fast | Yes | No |

## By input type

### Tabular data
Start with tree-based models. Always.
- XGBoost or LightGBM as primary model
- Random Forest as secondary
- Logistic Regression as interpretable baseline
- Neural networks only if dataset exceeds ~50k samples
- Relevant nodes: [[parkinsons]], [[fraud-detection]]

### Medical images
CNNs are the standard. Transfer learning is almost always better than
training from scratch given dataset sizes.
- ResNet50 or DenseNet121 as starting backbone
- EfficientNet for compute-constrained settings
- Vision Transformers (ViT) only if dataset is large enough (>50k images)
- Relevant nodes: [[chest-xray]], [[lung-cancer]]

### Sequences (DNA, protein)
Depends heavily on dataset size and sequence length.
- K-mer + Random Forest for small datasets - strong baseline
- CNN for fixed-length sequences
- Transformers (DNABERT) for large datasets with compute budget
- GNNs for structure-based tasks - see [[alphafold]]
- Relevant nodes: [[dna-sequencing]], [[alphafold]]

## Dataset size rules of thumb

- Under 1k samples - classical ML only, heavy cross-validation
- 1k-10k samples - classical ML primary, simple CNNs with transfer learning
- 10k-100k samples - deep learning becomes competitive
- Over 100k samples - deep learning preferred for images and sequences

## Interpretability considerations

Clinical deployment often requires explainability:
- Tree-based models - feature importance built in
- Logistic Regression - coefficients are directly interpretable
- CNNs - use Grad-CAM for visual explanation of image predictions
- Black box models - SHAP values work across model types

## When to use anomaly detection

Supervised classification needs labels. When fraud or disease labels are
scarce or unreliable, consider:
- Isolation Forest - unsupervised, no labels needed
- Autoencoders - learn normal pattern, flag deviations
- Relevant node: [[fraud-detection]]

## Links

- [[imbalanced-data]] - imbalance handling varies by model type
- [[eval-metrics]] - metric choice is tied to model selection
- [[chest-xray]] - CNN selection for imaging
- [[dna-sequencing]] - sequence model tradeoffs
- [[parkinsons]] - tabular model selection
- [[fraud-detection]] - anomaly detection vs supervised classification