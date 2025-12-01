# Maternal Mice Analysis

## Overview

This directory contains Jupyter notebooks for extended behavioral analysis of maternal mice, including fingerprint construction, PCA optimization, and classification.

## Notebooks

### Analyze_Moseq.ipynb
Core analysis notebook for maternal MoSeq data:
- Loads and preprocesses MoSeq dataframes
- Computes syllable usage statistics
- Generates group comparisons across conditions
- Performs statistical testing

### Moms_Fingerprint_Classifiers.ipynb
Classification pipeline for maternal behavioral fingerprints:
- Constructs 495-dimensional feature matrices
- Applies PCA dimensionality reduction
- Optimizes number of components (8 PCs optimal)
- Trains and evaluates logistic regression classifiers

### Moms_Offsprings_Classifiers_vMahmoud.ipynb
Comparative analysis between maternal and offspring populations:
- Cross-population behavioral comparisons
- Feature importance analysis
- Population-specific classification performance

## Key Functions

### Feature Matrix Construction
Combines five behavioral metrics into unified feature space:
- `dist_to_center_px`: 99 features
- `height_ave_mm`: 99 features
- `length_mm`: 99 features
- `velocity_2d_mm`: 99 features
- `MoSeq`: 99 features
- **Total**: 495 features

### Classification Pipeline
```
Input (495D) → StandardScaler → PCA(8) → LogisticRegression
```

## Input Data

Notebooks require data from `../../data/`:
- `moseq_df/moseq_df.csv`: Session metadata
- `fingerprint_csv/FingerprintSummary_full.csv`: Raw fingerprints

## Output Files

### Model Files
- `best_pca_logreg.joblib`: Trained classification pipeline

### Embedding Files
- `../../data/analysis_output/offspring_PCA8_embedding.csv.gz`: 8-PC embeddings
- `../../data/analysis_output/offspring_PCA8_embedding.pkl`: Pickle format

## Statistical Methods

### Hyperparameter Optimization
- PCA components: [5, 8, 10, 12, 15, 0.90, 0.95]
- Regularization (C): logarithmic range
- Cross-validation: 5-fold stratified
- Metric: Balanced accuracy

## Dependencies

- pandas, numpy
- scikit-learn
- scipy, statsmodels
- matplotlib, seaborn
- joblib

## Related Directories

- `../Processing/`: MoSeq pipeline notebooks
- `../../data/`: Input and output data files
- `../../../Data/PC_Embeddings/Moms/`: Exported embeddings

