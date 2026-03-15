---
description: Using AlphaFold for protein structure prediction. Use when
  working on structure prediction, protein function analysis, or
  integrating predicted structures into downstream ML tasks.
---

# AlphaFold protein structure prediction

AlphaFold2 predicts 3D protein structure from amino acid sequence.
Unlike other nodes in this graph, the typical use case is not training
from scratch. it is running inference with pretrained weights or using
predicted structures as features.

## When to use AlphaFold

- You have a protein sequence and need its 3D structure
- Downstream task needs structural features (binding sites, folding regions)
- Validating experimental structure predictions
- No crystal structure available in PDB for your protein of interest

## How to run it

- **AlphaFold2 locally** - requires GPU, complex setup, full control
- **ColabFold** - easiest option, runs in Google Colab, no local setup needed
- **AlphaFold Database** - check if structure already predicted at
  alphafold.ebi.ac.uk before running anything

Always check the database first - over 200 million structures already predicted.

## Output

- `.pdb` file - 3D coordinates of predicted structure
- **pLDDT score** -mper-residue confidence score (0-100). Above 90 is
  high confidence, below 70 is unreliable, do not use for downstream tasks.
- **PAE (Predicted Aligned Error)** - use this to assess multi-domain
  protein predictions

## Using predicted structures in ML

- Extract structural features (secondary structure, solvent accessibility)
  using BioPython or MDAnalysis
- Graph Neural Networks (GNNs) work well on protein structures -
  treat residues as nodes, spatial proximity as edges
- Always filter by pLDDT score before using predictions as features -
  low confidence regions add noise

## Pitfalls

- pLDDT score is not a measure of biological relevance - high confidence
  does not mean the protein is functional or correctly folded in context
- AlphaFold predicts static structure - does not capture conformational
  flexibility or binding-induced changes
- Intrinsically disordered regions score low - expected, not a bug

## Links

- [[dna-sequencing]] - sequence is the input to structure prediction
- [[model-selection]] - GNNs for structure-based downstream tasks
- [[eval-metrics]] - evaluating structure-based ML outputs