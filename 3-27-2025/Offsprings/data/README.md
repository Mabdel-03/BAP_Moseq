# Offspring Data

## Overview

This directory contains processed data from the MoSeq pipeline and analysis for offspring mice from the March 2025 run, including sex-stratified outputs.

## Directory Structure

```
data/
├── analysis_output/
│   └── constructed_fingerprints/
│       ├── Females/
│       │   ├── final_data/
│       │   │   └── females_final_data.csv
│       │   ├── offspring_PCA25_embedding.csv.gz
│       │   ├── offspring_PCA25_embedding.pkl
│       │   └── pca12_logreg.joblib
│       └── Males/
│           ├── final_data/
│           │   └── males_final_data.csv
│           ├── offspring_PCA12_embedding.csv.gz
│           ├── offspring_PCA25_embedding.csv.gz
│           └── pca12_logreg.joblib
├── fingerprint_csv/
│   ├── FingerprintRangeDict_full.csv
│   ├── FingerprintRangeDict_robust.csv
│   ├── FingerprintSummary_full.csv
│   └── FingerprintSummary_robust.csv
├── moseq_df/
│   └── moseq_df.csv
├── stats_df/
│   └── stats_df.csv
├── syllable_counts/
│   └── [8 files: 4 conditions × 2 sexes]
└── transition_matrices/
    └── [8 files: 4 conditions × 2 sexes]
```

## Core Data Files

### moseq_df/moseq_df.csv
Main MoSeq dataframe containing:
- `uuid`: Universal unique identifier
- `SessionName`: Human-readable session identifier
- `group`: Experimental group (Male_EE, Female_LNB, etc.)
- Scalar behavioral metrics per session

### stats_df/stats_df.csv
Statistical summaries per session:
- Mean and variance of behavioral metrics
- Session-level aggregate statistics

## Fingerprint Data

### fingerprint_csv/

**FingerprintSummary_full.csv**
Multi-index DataFrame with complete fingerprint data:
- Index: (group, uuid)
- Columns: (metric, feature_index)
- 5 metrics × 99 features = 495 total dimensions

**FingerprintSummary_robust.csv**
Robust fingerprint summary:
- Outlier-resistant statistics
- Median-based calculations

## Analysis Outputs

### Males (analysis_output/constructed_fingerprints/Males/)

**males_final_data.csv**
Final 25-PC embeddings for male offspring:
- `uuid`: Mouse identifier
- `moseq_id`: BAP session identifier
- `category`: Environmental condition
- `PC1` through `PC25`: Principal component coordinates

**pca12_logreg.joblib** / **pca25_logreg.joblib**
Trained classification pipelines for different PC counts.

### Females (analysis_output/constructed_fingerprints/Females/)

**females_final_data.csv**
Final 12-PC embeddings for female offspring:
- `uuid`: Mouse identifier
- `moseq_id`: BAP session identifier
- `category`: Environmental condition
- `PC1` through `PC12`: Principal component coordinates

## Syllable Data

### syllable_counts/
Per-group syllable usage frequencies:
- `EE_Normal_Male_syllable_counts.csv`
- `EE_Normal_Female_syllable_counts.csv`
- `LNB_Normal_Male_syllable_counts.csv`
- `LNB_Normal_Female_syllable_counts.csv`
- `NGH_Normal_Male_syllable_counts.csv`
- `NGH_Normal_Female_syllable_counts.csv`
- `SI_Normal_Male_syllable_counts.csv`
- `SI_Normal_Female_syllable_counts.csv`

### transition_matrices/
Syllable-to-syllable transition probabilities:
- Row-normalized matrices
- 8 files matching syllable_counts structure

## Usage

### Loading Session Data
```python
import pandas as pd

moseq_df = pd.read_csv('moseq_df/moseq_df.csv')
stats_df = pd.read_csv('stats_df/stats_df.csv')
```

### Loading Fingerprint Data
```python
# Multi-index fingerprint summary
fingerprint = pd.read_csv('fingerprint_csv/FingerprintSummary_full.csv',
                          index_col=[0, 1], header=[0, 1])

# Stratify by sex
males = fingerprint[fingerprint.index.get_level_values('group').str.contains('Male')]
females = fingerprint[fingerprint.index.get_level_values('group').str.contains('Female')]
```

### Loading Sex-Specific Embeddings
```python
# Male embeddings (25 PCs)
males_emb = pd.read_csv('analysis_output/constructed_fingerprints/Males/final_data/males_final_data.csv')

# Female embeddings (12 PCs)
females_emb = pd.read_csv('analysis_output/constructed_fingerprints/Females/final_data/females_final_data.csv')
```

## Related Directories

- `../code/`: Processing and analysis notebooks
- `../figs/`: Generated figures
- `../../Data/`: Shareable data exports

