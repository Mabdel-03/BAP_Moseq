# Offspring Processing

## Overview

This directory contains Jupyter notebooks for processing offspring mice through the MoSeq2 pipeline.

## Notebooks

### Inspect Metadata.ipynb
Validates session metadata for offspring subjects:
- Verifies subject identifiers (uuid, SessionName)
- Confirms sex and condition assignments
- Checks recording quality and completeness

### MoSeq2-Extract-Modeling-Notebook.ipynb
Executes the core MoSeq pipeline:

1. **Extraction**
   - Processes depth recordings from offspring sessions
   - Extracts pose estimates and centroid trajectories
   - Computes scalar metrics (velocity, height, length, position)

2. **Modeling**
   - Fits autoregressive Hidden Markov Model
   - Identifies behavioral syllables from pose dynamics
   - Generates model outputs (model-006-4641589)

### MoSeq2-Analysis-Visualization-Notebook.ipynb
Produces standard MoSeq visualizations:
- Syllable crowd movies for behavioral interpretation
- Usage frequency distributions
- Transition probability matrices
- Scalar metric comparisons

## Output Files

Processing generates the following outputs in `../../data/`:
- `moseq_df.csv`: Main dataframe with session metadata
- `stats_df.csv`: Statistical summaries per session
- `syll_info.yaml`: Syllable information
- `moseq2-index.yaml`: Session index file

## Usage

Run notebooks sequentially:
1. `Inspect Metadata.ipynb` - Validate data before processing
2. `MoSeq2-Extract-Modeling-Notebook.ipynb` - Run extraction and modeling
3. `MoSeq2-Analysis-Visualization-Notebook.ipynb` - Generate visualizations

## Model Details

- **Model**: model-006-4641589
- **Training Data**: Offspring mice only
- **Population-Specific**: Yes (optimized for offspring behavioral patterns)

## Notes

- Extended analysis continues in `../Analysis/`
- Raw session data remains in source directories

