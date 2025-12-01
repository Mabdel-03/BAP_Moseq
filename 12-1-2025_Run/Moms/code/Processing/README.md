# Maternal Mice Processing

## Overview

This directory contains Jupyter notebooks for processing maternal mice through the MoSeq2 pipeline with population-specific optimization.

## Notebooks

### Inspect Metadata.ipynb
Validates session metadata for maternal mice:
- Verifies subject identifiers and condition assignments
- Checks recording quality and completeness
- Confirms group structure (Mom_EE, Mom_LNB, Mom_NGH, Mom_SI)

### MoSeq2-Extract-Modeling-Notebook.ipynb
Executes the MoSeq pipeline with maternal-specific processing:

1. **Extraction**
   - Processes maternal mouse depth recordings
   - Extracts pose and position data
   - Computes behavioral scalar metrics

2. **Modeling**
   - Fits AR-HMM to maternal behavioral data
   - Identifies syllables characteristic of maternal behavior
   - Generates model output files

### MoSeq2-Analysis-Visualization-Notebook.ipynb
Produces maternal-specific analyses:
- Syllable usage by environmental condition
- Transition probability matrices
- Scalar metric distributions
- Statistical comparisons across conditions

## Population-Specific Benefits

Training on maternal mice exclusively provides:
- Feature extraction optimized for maternal behavioral patterns
- Reduced influence of offspring-specific movements
- Enhanced detection of condition effects in maternal behavior

## Usage

Run notebooks in sequence:
1. `Inspect Metadata.ipynb` - Verify data quality
2. `MoSeq2-Extract-Modeling-Notebook.ipynb` - Process data
3. `MoSeq2-Analysis-Visualization-Notebook.ipynb` - Generate results

## Notes

- For complete maternal analysis results, see `../../../../3-27-2025/Moms/`
- Raw session data remains in original recording directories

