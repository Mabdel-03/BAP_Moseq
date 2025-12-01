# Combined Population Processing

## Overview

This directory contains Jupyter notebooks for processing all subjects (maternal and offspring mice combined) through the MoSeq2 pipeline.

## Notebooks

### Inspect Metadata.ipynb
Examines session metadata to verify:
- Subject identifiers and group assignments
- Recording parameters and quality
- Session completeness and data integrity

### MoSeq2-Extract-Modeling-Notebook.ipynb
Executes the core MoSeq pipeline:

1. **Extraction Phase**
   - Processes raw depth recordings
   - Extracts pose estimates and centroid positions
   - Computes scalar metrics (velocity, height, length)

2. **Modeling Phase**
   - Fits autoregressive Hidden Markov Model
   - Identifies behavioral syllables
   - Assigns syllable labels to each frame

### MoSeq2-Analysis-Visualization-Notebook.ipynb
Generates analyses and visualizations:

1. **Syllable Analysis**
   - Usage frequencies per group
   - Crowd movies for syllable interpretation
   - Syllable duration distributions

2. **Transition Analysis**
   - Syllable-to-syllable transition matrices
   - Transition probability comparisons

3. **Scalar Metrics**
   - Velocity, height, and position distributions
   - Group-level statistical comparisons

## Usage

These notebooks are designed to be run in sequence:
1. Run `Inspect Metadata.ipynb` to verify data quality
2. Run `MoSeq2-Extract-Modeling-Notebook.ipynb` for processing
3. Run `MoSeq2-Analysis-Visualization-Notebook.ipynb` for analysis

## Dependencies

- moseq2-extract
- moseq2-model
- moseq2-viz
- jupyter
- numpy, pandas, matplotlib, seaborn

## Notes

- The combined population model provides a unified syllable vocabulary
- For population-specific optimization, see `../../Moms/` and `../../Offsprings/`

