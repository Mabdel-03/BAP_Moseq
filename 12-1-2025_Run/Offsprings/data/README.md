# Offspring Data

## Overview

This directory contains processed data from the MoSeq pipeline and analysis notebooks for offspring mice from the December 2025 run.

## Directory Structure

```
data/
├── fingerprints/
│   ├── full/
│   │   └── fingerprint_summary_full.csv
│   └── robust/
│       └── fingerprint_summary_robust.csv
├── analysis_output/
│   ├── constructed_fingerprints/
│   └── pvalues/
├── plots/
│   ├── bigram_transition_matrix.pdf/png
│   ├── moseq_fingerprint.pdf/png
│   ├── syllable_dendrogram.pdf/png
│   └── syllable_needed_explained_variance.pdf/png
├── crowd_movies/
├── moseq_df.csv
├── stats_df.csv
├── moseq2-index.yaml
├── syll_info.yaml
├── model-006-4641589.p
├── *_syllable_counts.csv
└── *_bigram_transition_matrix.csv
```

## Syllable Videos

Crowd movies showing representative examples of each behavioral syllable are available externally:

[Syllable Videos (Dropbox)](https://www.dropbox.com/scl/fo/tqox70qyxmikhzjwgd6v7/AODnVlrv4D5BMBp1PqAilUM?rlkey=uiknwgcuqw9tzj7oe2nxj9lpu&st=bq2cth8t&dl=0)

## Core Data Files

### moseq_df.csv
Main MoSeq dataframe containing:
- `uuid`: Universal unique identifier
- `SessionName`: Human-readable session name
- `group`: Experimental group assignment
- Scalar behavioral metrics per session
- Session metadata

### stats_df.csv
Statistical summary per session including:
- Mean and variance of behavioral metrics
- Session-level statistics

### Model File
- `model-006-4641589.p`: Trained AR-HMM model for syllable assignment

## Fingerprints

### fingerprints/full/
Complete fingerprint summary without outlier removal:
- Multi-index DataFrame structure
- 5 behavioral metrics × 99 features = 495 total features

### fingerprints/robust/
Fingerprint summary with robust statistics:
- Outlier-resistant measures
- Median-based calculations

## Syllable Data

### Syllable Counts
Per-group syllable usage files:
- `EE_Normal_Male_syllable_counts.csv`
- `EE_Normal_Female_syllable_counts.csv`
- `LNB_Normal_Male_syllable_counts.csv`
- `LNB_Normal_Female_syllable_counts.csv`
- `NGH_Normal_Male_syllable_counts.csv`
- `NGH_Normal_Female_syllable_counts.csv`
- `SI_Normal_Male_syllable_counts.csv`
- `SI_Normal_Female_syllable_counts.csv`

### Transition Matrices
Syllable-to-syllable transition probabilities:
- `*_bigram_transition_matrix.csv` for each group

## Analysis Outputs

### analysis_output/constructed_fingerprints/
Processed fingerprint data used for classification:
- Feature matrices after preprocessing
- PCA-transformed embeddings

### analysis_output/pvalues/
Statistical test results:
- Kruskal-Wallis test p-values
- FDR-corrected significance values

## Plots

Generated visualizations in PDF and PNG formats:
- `bigram_transition_matrix`: Syllable transition heatmaps
- `moseq_fingerprint`: Behavioral fingerprint visualizations
- `syllable_dendrogram`: Hierarchical clustering of syllables
- `syllable_needed_explained_variance`: PCA variance analysis

## Configuration Files

### moseq2-index.yaml
Session index with file paths and metadata for the MoSeq pipeline.

### syll_info.yaml
Syllable information including:
- Syllable labels and descriptions
- Usage statistics

## Usage

### Loading Main Dataframes
```python
import pandas as pd

moseq_df = pd.read_csv('moseq_df.csv')
stats_df = pd.read_csv('stats_df.csv')
```

### Loading Fingerprint Data
```python
# Load multi-index fingerprint summary
fingerprint = pd.read_csv('fingerprints/full/fingerprint_summary_full.csv',
                          index_col=[0, 1], header=[0, 1])
```

## Related Directories

- `../code/`: Processing and analysis notebooks
- `../figs/`: Generated figures

