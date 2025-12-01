# Offspring Mice Analysis

## Overview

This directory contains the complete MoSeq processing and analysis pipeline for offspring mice from the December 2025 run. The offspring analysis is the most extensively developed component of this processing run.

## Directory Structure

```
Offsprings/
├── code/
│   ├── Processing/              # MoSeq pipeline notebooks
│   └── Analysis/                # Statistical analysis notebooks
├── data/
│   ├── fingerprints/            # Full and robust fingerprint summaries
│   ├── analysis_output/         # Classification and embedding outputs
│   ├── plots/                   # Generated plot files
│   ├── moseq_df.csv             # Main MoSeq dataframe
│   ├── stats_df.csv             # Statistics dataframe
│   ├── *_syllable_counts.csv    # Syllable counts by group
│   └── *_transition_matrix.csv  # Transition matrices by group
└── figs/
    ├── Scalar_Figs/             # Scalar metric visualizations
    └── Syllable_Usage/          # Syllable usage plots
```

## Population Details

### Sample Composition
- **Total Offspring**: 66 mice
- **Males**: 33 mice
- **Females**: 33 mice

### Environmental Conditions
Offspring mice were exposed to four conditions:

| Condition | Description |
|-----------|-------------|
| **EE** | Enriched Environment |
| **LNB** | Limited Nesting and Bedding |
| **NGH** | Normal Growth Housing (control) |
| **SI** | Social Isolation |

## Model Information

- **Model ID**: model-006-4641589
- **Source**: `/om/scratch/Mon/mabdel03/Moseq/Run_Offsprings/moseq_data/models/model-006-4641589`
- **Training**: Population-specific (offspring only)

## Analysis Pipeline

### Processing Stage
1. Metadata inspection and validation
2. MoSeq extraction and modeling
3. Syllable assignment and visualization

### Analysis Stage
1. Fingerprint construction and classification
2. Sex-stratified behavioral analysis
3. Statistical comparisons across conditions

## Syllable Videos

Crowd movies showing representative examples of each behavioral syllable are available at:

[Syllable Videos (Dropbox)](https://www.dropbox.com/scl/fo/tqox70qyxmikhzjwgd6v7/AODnVlrv4D5BMBp1PqAilUM?rlkey=uiknwgcuqw9tzj7oe2nxj9lpu&st=bq2cth8t&dl=0)

These videos provide visual interpretation of the discrete behavioral motifs identified by the MoSeq model, enabling qualitative understanding of syllable-level differences across environmental conditions.

## Key Outputs

### Data Files
- `moseq_df.csv`: Session-level MoSeq metrics and metadata
- `stats_df.csv`: Statistical summaries per session
- `fingerprints/`: Behavioral fingerprint matrices

### Figures
- Scalar metric comparisons (position, velocity, height)
- Syllable usage distributions
- Pairwise statistical comparisons (FDR-corrected and uncorrected)

## Status

| Stage | Status |
|-------|--------|
| Processing | Complete |
| Model | model-006-4641589 |
| Analysis | Ready to run |

## Analysis Notebooks

The analysis notebooks in `code/Analysis/` include:
- `Analyze_Moseq.ipynb`: Core MoSeq analysis and visualization
- `Offsprings_Fingerprint_Classifiers.ipynb`: Fingerprint-based classification
- `Male_Offsprings_Classifiers_vMahmoud.ipynb`: Male-specific analysis
- `Female_Offsprings_Classifiers_vMahmoud.ipynb`: Female-specific analysis
- `ID_Reassignment.ipynb`: Subject identifier utilities

## Related Directories

- `../Moms/`: Maternal mice processing
- `../All/`: Combined population processing
- `../../3-27-2025/Offsprings/`: Previous analysis run with complete results

