---
description: ML approaches for DNA sequencing and genomic classification.
  Use when working on sequence classification, k-mer features, or any
  genomics ML task.
---

# DNA sequencing ML

ML on raw DNA sequences for classification tasks — pathogen detection,
gene function prediction, species classification.

## Representing DNA for ML

DNA can't go directly into most models. it needs encoding first.

- **K-mer frequency** - split sequence into overlapping k-length substrings,
  count frequencies, use as feature vector. Simple and effective baseline.
- **One-hot encoding** - encode each base (A, T, G, C) as a 4-dimensional
  vector. Works well for CNNs and RNNs.
- **Embeddings** - treat DNA like text, learn embeddings per k-mer.
  DNABERT does this with transformer architecture.

## Model approaches

- **Classical ML**  k-mer features + Random Forest or SVM. Strong baseline,
  interpretable, works on small datasets.
- **CNN** captures local sequence motifs. Good for fixed-length sequences.
- **LSTM/RNN** handles variable length sequences. Slower to train.
- **Transformers** DNABERT, Nucleotide Transformer. Best performance but
  needs significant compute and data.

## Dataset considerations

- Sequences vary in length. padding or truncation strategy matters
- Class imbalance is common in pathogen datasets. see [[imbalanced-data]]
- Train/test split must account for sequence similarity. random split leaks

## Evaluation

Standard classification metrics apply but see [[eval-metrics]] for
imbalanced dataset caveats. MCC (Matthews Correlation Coefficient) is
widely used in genomics specifically.

## Pitfalls

- Random train/test split on similar sequences = data leakage
- K-mer size k is a critical hyperparameter - too small loses context,
  too large explodes feature space
- GC content bias across species can confound models

## Links

- [[alphafold]] - when the task moves from sequence to structure prediction
- [[imbalanced-data]] - class imbalance common in pathogen classification
- [[eval-metrics]] - MCC recommended alongside AUC-ROC for genomics
- [[model-selection]] - classical ML vs deep learning tradeoff for small genomics datasets