# Offspring Mice Code

## Overview

This directory contains all code for processing and analyzing offspring mice behavioral data from the December 2025 run.

## Directory Structure

```
code/
├── Processing/          # MoSeq pipeline notebooks
└── Analysis/            # Statistical analysis notebooks
```

## Processing

The `Processing/` directory contains standard MoSeq2 pipeline notebooks for:
- Metadata inspection
- Video extraction and model fitting
- Syllable visualization and analysis

## Analysis

The `Analysis/` directory contains extended analysis notebooks for:
- Behavioral fingerprint classification
- Sex-stratified analyses (Male and Female)
- Statistical comparisons across environmental conditions
- Machine learning classification pipelines

## Notebook Dependencies

Analysis notebooks depend on outputs from the Processing stage:
- `../../data/moseq_df.csv`: Session metadata and metrics
- `../../data/stats_df.csv`: Statistical summaries
- `../../data/fingerprints/`: Fingerprint summary files

## Related Directories

- `../../data/`: Output data files
- `../../figs/`: Generated figures

