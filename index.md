# Biomedical ML Skill Graph

Knowledge graph covering ML approaches across medical imaging, genomics,
and clinical data. Each node links to related concepts. Follow wikilinks
relevant to the current task.


## Domains

- [[medical-imaging]] - CNNs for chest X-ray, lung cancer detection, preprocessing
  - [[whole-slide-imaging]] - patch extraction, SSL pretraining, MIL aggregation
  - [[follicular-lymphoma]] - FL subtype prediction from H&E WSIs
  - [[tcga-lung-classification]] - LUAD vs LUSC classification on NYU HPC
- [[genomics]] - DNA sequencing classification, protein structure with AlphaFold
- [[clinical-data]] - tabular data, Parkinson's, healthcare fraud detection

## Cross-cutting concerns

- [[imbalanced-data]] - handling severe class imbalance across all domains
- [[eval-metrics]] - why accuracy fails in medical ML, what to use instead
- [[model-selection]] - tradeoffs for imaging vs genomics vs tabular tasks

## Entry points by task

Working on a new medical imaging project? Start with [[medical-imaging]].
Dealing with class imbalance? Go straight to [[imbalanced-data]].
Unsure which model to use? See [[model-selection]].
Evaluating a model? See [[eval-metrics]].