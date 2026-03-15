# Biomedical ML Skill Graph

A knowledge graph of ML approaches across medical imaging, genomics,
and clinical data. Built as a Claude Code skill.

## What this is

A network of interconnected markdown files where each file covers one
specific topic and links to related topics via wikilinks. Claude Code
reads this graph and navigates relevant nodes automatically when working
on biomedical ML tasks.

## Structure
```
skill-graph-biomedical-ml/
  index.md                        <- entry point, map of the whole graph
  imaging/
    chest-xray.md                 <- CNN classification, preprocessing
    lung-cancer.md                <- CT scan analysis, 3D vs 2D tradeoffs
  genomics/
    dna-sequencing.md             <- k-mer features, DNABERT, sequence ML
    alphafold.md                  <- structure prediction, pLDDT, GNNs
  clinical/
    parkinsons.md                 <- tabular ML, hyperparameter tuning
    fraud-detection.md            <- extreme imbalance, anomaly detection
  cross-cutting/
    imbalanced-data.md            <- SMOTE, class weights, threshold tuning
    eval-metrics.md               <- AUC-ROC, MCC, precision-recall
    model-selection.md            <- classical ML vs deep learning tradeoffs
  .claude/skills/biomedical-ml/
    SKILL.md                      <- Claude Code skill entry point
```

## How the graph works

Each node links to related nodes with wikilinks. Example from
`chest-xray.md`:

> Class imbalance is severe in medical datasets - see [[imbalanced-data]]
> for handling strategies before training.

Claude Code follows these links and loads only what is relevant for the
current task. not the whole graph at once.

## Visualizing the graph

Open this folder in [Obsidian](https://obsidian.md) to see the
connections rendered as an interactive network diagram.

## Using with Claude Code

Clone this repo into your project and Claude Code will automatically
read the skill when working on biomedical ML tasks. No setup required
beyond cloning.

## Projects this graph covers

## Projects this graph covers

- [Chest X-ray Pneumonia/COVID Detection](https://github.com/AravindKurapati/ChestXray_Pneumonia_COVID_Detection)
  — CNN-based classifier detecting pneumonia and COVID-19 from chest X-ray images using deep learning and transfer learning.

- [Lung Cancer Classification](https://github.com/AravindKurapati/LungCancer_Classification_Prediction)
  — ML pipeline for classifying lung cancer malignancy from clinical and imaging features.

- [Parkinson's Hyperparameter Tuning](https://github.com/AravindKurapati/Hyperparamter-Tuning-Parkinsons)
  — Systematic hyperparameter optimization for detecting Parkinson's disease from biomedical voice measurements.

- [Healthcare Fraud Detection](https://github.com/AravindKurapati/Fraud-Detection-Medical)
  — Fraud detection on medical claims data handling extreme class imbalance with ensemble methods.

- [DNA Sequencing ML](https://github.com/AravindKurapati/DNA-Sequencing-ML)
  — Classification of DNA sequences using k-mer feature extraction and machine learning.

- [AlphaFold Prediction](https://github.com/AravindKurapati/alphafold-prediction)
  — Protein structure prediction pipeline using AlphaFold2 with downstream structure-based analysis.