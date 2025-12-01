# Maternal Mice Processing

## Overview

This directory contains Jupyter notebooks for processing maternal mice through the MoSeq2 pipeline.

## Notebooks

### MoSeq2-Extract-Modeling-Notebook.ipynb
Core MoSeq pipeline notebook covering:

1. **Extraction Phase**
   - Depth video processing
   - Pose and centroid extraction
   - Scalar metric computation (velocity, height, length, position)

2. **Modeling Phase**
   - Autoregressive Hidden Markov Model fitting
   - Syllable identification from pose dynamics
   - Model output generation

### MoSeq2-Analysis-Visualization-Notebook.ipynb
Standard MoSeq analysis and visualization:
- Syllable crowd movies
- Usage frequency distributions
- Transition probability matrices
- Group-level statistical comparisons

## Output Files

Processing generates outputs in `../../data/`:
- `moseq_df/moseq_df.csv`: Session metadata
- `stats_df/stats_df.csv`: Statistical summaries
- `syllable_counts/`: Per-group syllable usage
- `transition_matrices/`: Transition probabilities

## Usage

Run notebooks in sequence:
1. `MoSeq2-Extract-Modeling-Notebook.ipynb` - Process data
2. `MoSeq2-Analysis-Visualization-Notebook.ipynb` - Generate visualizations

## Population-Specific Model

The maternal MoSeq model provides:
- Features optimized for maternal behavioral patterns
- Reduced cross-population noise
- Enhanced sensitivity for condition effects

## Notes

- Extended analysis continues in `../Analysis/`
- Raw session data stored separately from processed outputs

