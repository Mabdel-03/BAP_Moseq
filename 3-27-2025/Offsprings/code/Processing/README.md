# Offspring Processing

## Overview

This directory contains Jupyter notebooks for processing offspring mice through the MoSeq2 pipeline.

## Notebooks

### MoSeq2-Extract-Modeling-Notebook.ipynb
Core MoSeq pipeline notebook covering:

1. **Extraction Phase**
   - Depth video processing for offspring sessions
   - Pose and centroid extraction
   - Scalar metric computation (velocity, height, length, position)

2. **Modeling Phase**
   - Autoregressive Hidden Markov Model fitting
   - Syllable identification from pose dynamics
   - Population-specific model generation

### MoSeq2-Analysis-Visualization-Notebook.ipynb
Standard MoSeq analysis and visualization:
- Syllable crowd movies for behavioral interpretation
- Usage frequency distributions by group and sex
- Transition probability matrices
- Group-level statistical comparisons

## Output Files

Processing generates outputs in `../../data/`:
- `moseq_df/moseq_df.csv`: Session metadata
- `stats_df/stats_df.csv`: Statistical summaries
- `syllable_counts/`: Per-group syllable usage (8 files: 4 conditions × 2 sexes)
- `transition_matrices/`: Transition probabilities (8 files)

## Usage

Run notebooks in sequence:
1. `MoSeq2-Extract-Modeling-Notebook.ipynb` - Process data
2. `MoSeq2-Analysis-Visualization-Notebook.ipynb` - Generate visualizations

## Population-Specific Model

The offspring MoSeq model provides:
- Features optimized for offspring behavioral patterns
- Reduced influence of maternal-specific movements
- Foundation for sex-stratified analysis

## Sex Stratification

While this processing uses a unified offspring model, subsequent analysis in `../Analysis/` stratifies by sex:
- **Males**: 33 samples → 25 optimal PCs
- **Females**: 33 samples → 12 optimal PCs

## Notes

- Extended analysis continues in `../Analysis/`
- Raw session data stored separately

