# Maternal Mice Code

## Overview

This directory contains code for processing and analyzing maternal mice behavioral data from the March 2025 run.

## Directory Structure

```
code/
├── Processing/          # MoSeq pipeline notebooks
└── Analysis/            # Statistical analysis notebooks
```

## Processing

The `Processing/` directory contains MoSeq2 pipeline notebooks for:
- Video extraction and preprocessing
- AR-HMM model fitting
- Syllable visualization and analysis

## Analysis

The `Analysis/` directory contains extended analysis notebooks for:
- Behavioral fingerprint construction
- PCA dimensionality reduction
- Classification pipeline development
- Statistical comparisons

## Notebook Dependencies

Analysis notebooks require outputs from the Processing stage:
- `../../data/moseq_df/moseq_df.csv`: Session metadata
- `../../data/stats_df/stats_df.csv`: Statistical summaries
- `../../data/fingerprint_csv/`: Fingerprint data files

## Related Directories

- `../../data/`: Output data files
- `../../../Data/`: Shareable data exports

