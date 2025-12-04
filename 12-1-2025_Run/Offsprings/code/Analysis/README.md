# Offspring Analysis

## Overview

This directory contains Jupyter notebooks for extended behavioral analysis of offspring mice, including fingerprint classification, sex-stratified analyses, and machine learning pipelines.

## Notebooks

### Analyze_Moseq.ipynb
Core analysis notebook for MoSeq behavioral data:
- Loads and preprocesses MoSeq dataframes
- Computes syllable usage and transition statistics
- Generates group comparisons across environmental conditions
- Performs statistical testing (Kruskal-Wallis with FDR correction)

### Offsprings_Fingerprint_Classifiers.ipynb
Classification pipeline for behavioral fingerprints:
- Constructs 495-dimensional feature matrices
- Applies PCA dimensionality reduction
- Trains logistic regression classifiers
- Evaluates classification performance

### Male_Offsprings_Classifiers_vMahmoud.ipynb
Male-specific behavioral classification:
- Extracts male offspring data
- Optimizes PCA components for male behavioral patterns
- Trains and evaluates sex-specific classifiers
- Generates male-specific visualizations

### Female_Offsprings_Classifiers_vMahmoud.ipynb
Female-specific behavioral classification:
- Extracts female offspring data
- Optimizes PCA components for female behavioral patterns
- Trains and evaluates sex-specific classifiers
- Generates female-specific visualizations

### ID_Reassignment.ipynb
Utility notebook for subject identifier management:
- Maps between uuid and SessionName identifiers
- Handles group label reassignment
- Manages control group subdivisions (Ctrl-LNB, Ctrl-EE/SI)

## Key Functions

### rename_groups()
Reassigns group labels accounting for different control conditions based on session naming conventions.

### run_kruskal()
Performs Kruskal-Wallis tests with FDR correction for syllable-level comparisons between conditions.

## Input Data

Notebooks require data from `../../data/`:
- `moseq_df.csv`: Session metadata and MoSeq metrics
- `stats_df.csv`: Statistical summaries
- `fingerprints/`: Fingerprint summary files

### Data Download

Large data files are not included in the GitHub repository. Download from Dropbox:

**Dropbox Path**: `All Files/Mahmoud Abdelmoneum/BAP/Moseq/12-1-2025_Run/Offsprings_Only/Dataframes/`

| File | Size |
|------|------|
| `moseq_df.csv.gz` | 226 MB |
| `stats_df.csv` | 3.7 MB |
| `fingerprints/full/fingerprint_summary_full.csv` | 437 KB |

## Output Files

### Model Files
- `best_pca_logreg.joblib`: Trained PCA + Logistic Regression pipeline

### Data Files
- Analysis outputs saved to `../../data/analysis_output/`
- P-value tables saved to `../../data/analysis_output/pvalues/`

## Statistical Methods

### Classification Pipeline
```
Input Features (495D) → StandardScaler → PCA → Logistic Regression
```

### Hyperparameter Optimization
- PCA components tested: [5, 8, 10, 12, 15, 0.90, 0.95]
- Regularization (C): [0.01, 0.046, 0.215, 1.0, 4.64, 21.54, 100.0]
- Cross-validation: 5-fold stratified
- Optimization metric: Balanced accuracy

## Dependencies

- pandas, numpy
- scikit-learn (PCA, LogisticRegression, GridSearchCV)
- scipy (statistical tests)
- statsmodels (multiple testing correction)
- matplotlib, seaborn (visualization)
- joblib (model serialization)

## Syllable Usage Analysis Results

Analysis performed comparing syllable usage between experimental conditions (EE, SI, LNB) and their respective controls (Ctrl-EE/SI, Ctrl-LNB).

### Statistical Methods Used
- **Uncorrected**: Mann-Whitney U test, significance at p < 0.05
- **FDR Corrected**: Kruskal-Wallis test with Benjamini-Hochberg correction, significance at FDR p ≤ 0.05

### Males Summary

| Comparison | Uncorrected Significant | FDR Significant | Key Syllables (uncorrected) |
|------------|------------------------|-----------------|----------------------------|
| EE vs. Ctrl-EE/SI | 18 | 0 | 6, 15, 21, 23, 27, 34, 42, 44, 61 |
| SI vs. Ctrl-EE/SI | 17 | 0 | 6, 12, 22, 30, 31, 38, 43, 46, 54 |
| LNB vs. Ctrl-LNB | 4 | 0 | 3, 8, 20, 31 |

### Females Summary

| Comparison | Uncorrected Significant | FDR Significant | Key Syllables (uncorrected) |
|------------|------------------------|-----------------|----------------------------|
| EE vs. Ctrl-EE/SI | 11 | 0 | 12, 15, 23, 51, 59 |
| SI vs. Ctrl-EE/SI | 11 | 0 | 1, 10, 26, 29 |
| LNB vs. Ctrl-LNB | 4 | 0 | 6, 38, 53, 60 |

### Key Findings
- No syllables survive FDR correction in any comparison
- Males show more nominally significant syllables than females in EE/SI comparisons
- Full p-value tables available in `../../data/analysis_output/pvalues/`
- Figures available in `../../figs/Syllable_Usage/`

## Related Directories

- `../Processing/`: MoSeq pipeline notebooks
- `../../data/`: Input and output data files
- `../../figs/`: Generated figures

