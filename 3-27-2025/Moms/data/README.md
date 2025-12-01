# Maternal Mice Data

## Overview

This directory contains processed data from the MoSeq pipeline and analysis for maternal mice from the March 2025 run.

## Directory Structure

```
data/
├── analysis_output/
│   └── constructed_fingerprints/
│       ├── final_data/
│       ├── offspring_PCA8_embedding.csv.gz
│       ├── offspring_PCA8_embedding.pkl
│       └── pca8_logreg.joblib
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
│   ├── LNB_Mom_Female_syllable_counts.csv
│   └── NGH_Mom_Female_syllable_counts.csv
└── transition_matrices/
    ├── LNB_Mom_Female_rows_transition_matrix.csv
    └── NGH_Mom_Female_rows_transition_matrix.csv
```

## Core Data Files

### moseq_df/moseq_df.csv
Main MoSeq dataframe containing:
- `uuid`: Universal unique identifier
- `SessionName`: Human-readable session identifier
- `group`: Experimental group (Mom_EE, Mom_LNB, Mom_NGH, Mom_SI)
- Scalar behavioral metrics per session

### stats_df/stats_df.csv
Statistical summaries per session:
- Mean and variance of behavioral metrics
- Session-level aggregate statistics

## Fingerprint Data

### fingerprint_csv/
Behavioral fingerprint matrices:

**FingerprintSummary_full.csv**
- Multi-index DataFrame structure
- 5 behavioral metrics × 99 features = 495 total
- Complete data without outlier filtering

**FingerprintSummary_robust.csv**
- Robust statistics with outlier handling
- Median-based calculations

**FingerprintRangeDict_*.csv**
- Feature scaling and range information

## Analysis Outputs

### analysis_output/constructed_fingerprints/

**offspring_PCA8_embedding.csv.gz**
8-dimensional principal component embeddings:
- `uuid`: Mouse identifier
- `category`: Environmental condition
- `PC1` through `PC8`: Principal component coordinates

**pca8_logreg.joblib**
Trained classification pipeline:
- StandardScaler preprocessing
- PCA with 8 components
- Logistic regression classifier

## Syllable Data

### syllable_counts/
Per-group syllable usage frequencies:
- `LNB_Mom_Female_syllable_counts.csv`
- `NGH_Mom_Female_syllable_counts.csv`

### transition_matrices/
Syllable-to-syllable transition probabilities:
- Row-normalized transition matrices
- Per-group calculations

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
```

### Loading Embeddings
```python
# Compressed CSV
embeddings = pd.read_csv('analysis_output/constructed_fingerprints/offspring_PCA8_embedding.csv.gz')

# Faster pickle loading
embeddings = pd.read_pickle('analysis_output/constructed_fingerprints/offspring_PCA8_embedding.pkl')
```

## Related Directories

- `../code/`: Processing and analysis notebooks
- `../../Data/`: Shareable data exports

