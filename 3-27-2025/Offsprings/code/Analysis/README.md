# Offspring Analysis

## Overview

This directory contains Jupyter notebooks for extended behavioral analysis of offspring mice, including sex-stratified fingerprint classification and machine learning pipelines.

## Notebooks

### Analyze_Moseq.ipynb
Core analysis notebook for offspring MoSeq data:
- Loads and preprocesses MoSeq dataframes
- Computes syllable usage and transition statistics
- Generates group and sex-stratified comparisons
- Performs statistical testing (Kruskal-Wallis with FDR correction)

### Offsprings_Fingerprint_Classifiers.ipynb
Combined offspring classification pipeline:
- Constructs 495-dimensional feature matrices
- Demonstrates sex differences in optimal parameters
- Compares male and female classification performance

### Male_Offsprings_Classifiers_vMahmoud.ipynb
Male-specific behavioral classification:
- Extracts male offspring data (33 samples)
- Optimizes PCA to 25 components (95% variance)
- Achieves 75.8% balanced accuracy
- Generates confusion matrices and performance metrics

### Female_Offsprings_Classifiers_vMahmoud.ipynb
Female-specific behavioral classification:
- Extracts female offspring data (33 samples)
- Optimizes PCA to 12 components
- Achieves 69.7% accuracy, 66.4% balanced recall
- Analyzes condition-specific confusion patterns

## Key Functions

### rename_groups()
Handles control group subdivisions:
- NGH mice split into Ctrl-LNB and Ctrl-EE/SI based on cohort
- Uses SessionName parsing for assignment

### run_kruskal()
Statistical testing function:
- Kruskal-Wallis test for each syllable
- FDR correction for multiple comparisons
- Handles small sample sizes and constant values

## Feature Matrix Construction

Five behavioral metrics combined:
| Feature Range | Metric | Description |
|---------------|--------|-------------|
| 0-98 | `dist_to_center_px` | Spatial positioning |
| 99-197 | `height_ave_mm` | Vertical behavior |
| 198-296 | `length_mm` | Body length |
| 297-395 | `velocity_2d_mm` | Movement velocity |
| 396-494 | `MoSeq` | Behavioral fingerprints |

## Classification Results

### Male Offspring (25 PCs)
| Condition | Recall | Precision |
|-----------|--------|-----------|
| EE | 83% | 100% |
| LNB | 71% | 83% |
| NGH | 85% | 69% |
| SI | 57% | 67% |

### Female Offspring (12 PCs)
| Condition | Recall | Notes |
|-----------|--------|-------|
| EE | 67% | - |
| LNB | 29% | Often confused with NGH |
| NGH | 85% | - |
| SI | 86% | Best separation |

## Input Data

Notebooks require data from `../../data/`:
- `moseq_df/moseq_df.csv`: Session metadata
- `fingerprint_csv/FingerprintSummary_full.csv`: Raw fingerprints

## Output Files

### Male Outputs
- `../../data/analysis_output/constructed_fingerprints/Males/males_final_data.csv`
- `../../data/analysis_output/constructed_fingerprints/Males/pca12_logreg.joblib`

### Female Outputs
- `../../data/analysis_output/constructed_fingerprints/Females/females_final_data.csv`
- `../../data/analysis_output/constructed_fingerprints/Females/pca12_logreg.joblib`

## Statistical Methods

### Pipeline Architecture
```
Input (495D) → StandardScaler → PCA → LogisticRegression (multinomial)
```

### Hyperparameter Search
- PCA components: [5, 8, 10, 12, 15, 0.90, 0.95]
- Regularization (C): np.logspace(-2, 2, 7)
- Cross-validation: 5-fold stratified
- Metric: Balanced accuracy

## Dependencies

- pandas, numpy
- scikit-learn (PCA, LogisticRegression, GridSearchCV, Pipeline)
- scipy (kruskal)
- statsmodels (multipletests)
- matplotlib, seaborn
- joblib

## Related Directories

- `../Processing/`: MoSeq pipeline notebooks
- `../../data/`: Input and output data files
- `../../figs/`: Generated figures
- `../../../Data/PC_Embeddings/Offsprings/`: Exported embeddings

