# Maternal Mice Processing

## Overview

This directory contains MoSeq processing notebooks specifically for maternal mice from the December 2025 processing run.

## Purpose

Population-specific processing of maternal mice enables:
- Optimized feature extraction for maternal behavioral patterns
- Reduced noise from cross-population behavioral variability
- Enhanced sensitivity for detecting condition-specific effects

## Directory Structure

```
Moms/
└── code/
    └── Processing/
        ├── Inspect Metadata.ipynb
        ├── MoSeq2-Analysis-Visualization-Notebook.ipynb
        └── MoSeq2-Extract-Modeling-Notebook.ipynb
```

## Environmental Conditions

Maternal mice were exposed to four conditions:
- **EE**: Enriched Environment
- **LNB**: Limited Nesting and Bedding
- **NGH**: Normal Growth Housing
- **SI**: Social Isolation

## Processing Pipeline

The MoSeq pipeline for maternal mice follows the standard workflow:

1. **Metadata Inspection**: Verify session quality and group assignments
2. **Extraction and Modeling**: Process depth videos and fit behavioral model
3. **Analysis and Visualization**: Generate syllable statistics and comparisons

## Status

| Stage | Status |
|-------|--------|
| Processing | In progress |
| Model | Not run |
| Analysis | Not started |

## Related Directories

- `../All/`: Combined population processing
- `../Offsprings/`: Offspring-specific processing with extended analysis
- `../../3-27-2025/Moms/`: Previous maternal analysis run with complete results

