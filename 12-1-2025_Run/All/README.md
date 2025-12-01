# All Subjects Processing

## Overview

This directory contains MoSeq processing notebooks for the combined population analysis, which includes all maternal and offspring mice from the December 2025 processing run.

## Purpose

The combined population processing serves to:
- Establish a unified behavioral model across all subjects
- Enable cross-population comparisons with consistent syllable definitions
- Provide baseline behavioral characterization for the entire cohort

## Directory Structure

```
All/
└── code/
    └── Processing/
        ├── Inspect Metadata.ipynb
        ├── MoSeq2-Analysis-Visualization-Notebook.ipynb
        └── MoSeq2-Extract-Modeling-Notebook.ipynb
```

## Processing Notebooks

### Inspect Metadata.ipynb
Notebook for examining and validating session metadata, including subject identifiers, experimental conditions, and recording parameters.

### MoSeq2-Extract-Modeling-Notebook.ipynb
Core MoSeq pipeline notebook covering:
- Depth video extraction and preprocessing
- Principal component extraction from pose data
- Autoregressive Hidden Markov Model fitting
- Syllable assignment and model validation

### MoSeq2-Analysis-Visualization-Notebook.ipynb
Analysis and visualization notebook for:
- Syllable usage statistics
- Transition probability matrices
- Scalar behavioral metrics
- Group-level comparisons

## Status

| Stage | Status |
|-------|--------|
| Processing | In progress |
| Model | Not run |
| Analysis | Not started |

## Notes

- This combined analysis complements the population-specific analyses in `Moms/` and `Offsprings/`
- Raw session data remains in the original Run_* directories
- See `../Offsprings/` for the most complete analysis pipeline

