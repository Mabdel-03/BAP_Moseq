# Offspring Mice Analysis

## Overview

This directory contains the complete MoSeq processing and analysis pipeline for offspring mice from the March 2025 run. The analysis includes sex-stratified behavioral fingerprint construction, PCA optimization, and classification of environmental conditions.

## Directory Structure

```
Offsprings/
├── code/
│   ├── Processing/              # MoSeq pipeline notebooks
│   └── Analysis/                # Statistical analysis notebooks
├── data/
│   ├── analysis_output/         # Classification and embedding outputs
│   ├── fingerprint_csv/         # Processed fingerprint files
│   ├── moseq_df/                # Session metadata
│   ├── stats_df/                # Statistical summaries
│   ├── syllable_counts/         # Syllable usage by group
│   └── transition_matrices/     # Transition probabilities
└── figs/
    └── Scalar_Figs/             # Scalar metric visualizations
```

## Population Details

### Sample Composition
- **Total Offspring**: 66 mice
- **Males**: 33 mice
- **Females**: 33 mice

### Environmental Conditions

| Condition | Description |
|-----------|-------------|
| **EE** | Enriched Environment |
| **LNB** | Limited Nesting and Bedding |
| **NGH** | Normal Growth Housing (control) |
| **SI** | Social Isolation |

## Model Information

- **Population-Specific**: MoSeq model trained exclusively on offspring behavioral data
- **Optimal PCs (Males)**: 25 principal components
- **Optimal PCs (Females)**: 12 principal components
- **Feature Matrix**: 495 dimensions (5 metrics × 99 features)

## Analysis Pipeline

### Processing Stage
Standard MoSeq pipeline:
1. Depth video processing and pose extraction
2. AR-HMM modeling for syllable identification
3. Syllable assignment and scalar metric computation

### Analysis Stage
Extended sex-stratified analysis:
1. Separate male and female feature matrix construction
2. Sex-specific PCA optimization
3. Independent classification pipelines
4. Comparative performance evaluation

## Syllable Videos

Crowd movies showing representative examples of each behavioral syllable are available at:

[Syllable Videos (Dropbox)](https://www.dropbox.com/scl/fo/tqox70qyxmikhzjwgd6v7/AODnVlrv4D5BMBp1PqAilUM?rlkey=uiknwgcuqw9tzj7oe2nxj9lpu&st=bq2cth8t&dl=0)

These videos provide visual interpretation of the discrete behavioral motifs identified by the MoSeq model, enabling qualitative understanding of syllable-level differences across environmental conditions.

## Classification Performance

### Male Offspring
- **Principal Components**: 25 (95% variance retention)
- **Balanced Accuracy**: 75.8%
- **Balanced Recall**: 74.1%
- **Regularization (C)**: 4.64

### Female Offspring
- **Principal Components**: 12
- **Balanced Accuracy**: 69.7%
- **Balanced Recall**: 66.4%

## Key Outputs

### Data Files
- `data/moseq_df/moseq_df.csv`: Session metadata
- `data/fingerprint_csv/`: Fingerprint summaries
- `data/analysis_output/`: Sex-stratified embeddings

### Exported Data
- `../Data/PC_Embeddings/Offsprings/Male/`: Male embeddings
- `../Data/PC_Embeddings/Offsprings/Female/`: Female embeddings

## Sex Stratification Rationale

Sex-stratified analysis was performed because:
1. **Biological Differences**: Male and female mice exhibit different behavioral responses
2. **Optimal Parameters Differ**: Different PC counts optimize classification
3. **Statistical Power**: Within-sex analysis increases detection sensitivity

## Related Directories

- `../Moms/`: Maternal mice analysis
- `../Data/`: Shareable data exports
- `../../12-1-2025_Run/Offsprings/`: Updated processing run

