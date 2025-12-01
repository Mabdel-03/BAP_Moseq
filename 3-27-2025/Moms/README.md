# Maternal Mice Analysis

## Overview

This directory contains the complete MoSeq processing and analysis pipeline for maternal mice from the March 2025 run. The analysis includes behavioral fingerprint construction, PCA dimensionality reduction, and classification of environmental conditions.

## Directory Structure

```
Moms/
├── code/
│   ├── Processing/              # MoSeq pipeline notebooks
│   └── Analysis/                # Statistical analysis notebooks
└── data/
    ├── analysis_output/         # Classification and embedding outputs
    ├── fingerprint_csv/         # Processed fingerprint files
    ├── moseq_df/                # Session metadata
    ├── stats_df/                # Statistical summaries
    ├── syllable_counts/         # Syllable usage data
    └── transition_matrices/     # Syllable transition probabilities
```

## Population Details

### Sample Composition
Maternal mice exposed to early-life environmental conditions.

### Environmental Conditions

| Condition | Description |
|-----------|-------------|
| **EE** | Enriched Environment |
| **LNB** | Limited Nesting and Bedding |
| **NGH** | Normal Growth Housing (control) |
| **SI** | Social Isolation |

## Model Information

- **Population-Specific**: MoSeq model trained exclusively on maternal behavioral data
- **Optimal PCs**: 8 principal components
- **Feature Matrix**: 495 dimensions (5 metrics × 99 features)

## Analysis Pipeline

### Processing Stage
The MoSeq pipeline extracts behavioral features:
1. Depth video processing and pose extraction
2. AR-HMM modeling for syllable identification
3. Syllable assignment and scalar metric computation

### Analysis Stage
Extended analysis with fingerprint-based classification:
1. 495-dimensional feature matrix construction
2. StandardScaler normalization
3. PCA optimization via GridSearchCV
4. Logistic regression classification

## Key Outputs

### Data Files
- `moseq_df/moseq_df.csv`: Session-level metadata and metrics
- `stats_df/stats_df.csv`: Statistical summaries
- `fingerprint_csv/`: Full and robust fingerprint summaries

### Analysis Outputs
- `analysis_output/constructed_fingerprints/`: Processed embeddings
- `analysis_output/offspring_PCA8_embedding.csv.gz`: 8-PC embeddings

## Performance

The 8-PC maternal embedding achieves classification of environmental conditions using:
- 5-fold stratified cross-validation
- Balanced accuracy optimization
- Multinomial logistic regression

## Related Directories

- `../Offsprings/`: Offspring mice analysis with sex stratification
- `../Data/`: Shareable data exports (fingerprints and embeddings)
- `../../12-1-2025_Run/Moms/`: Updated processing run

