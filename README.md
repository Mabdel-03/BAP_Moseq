# BAP_Moseq

## Overview

This repository contains behavioral analysis data and code for the BAP (Behavioral Analysis Pipeline) project using Motion Sequencing (MoSeq). The project investigates how different early-life environmental conditions affect mouse behavior across maternal and offspring populations.

## Experimental Design

### Environmental Conditions

Mice were exposed to four distinct environmental conditions:

| Condition | Description |
|-----------|-------------|
| **EE** | Enriched Environment - Enhanced sensory stimulation, motor challenges, and social interaction |
| **LNB** | Limited Nesting and Bedding - Restricted nesting materials with minimal bedding |
| **NGH** | Normal Growth Housing - Standard laboratory housing conditions (baseline control) |
| **SI** | Social Isolation - Reduced social contact with individual housing |

### Populations

- **Moms**: Maternal mice exposed to experimental conditions
- **Offsprings**: Offspring mice stratified by sex (Males and Females)

## Repository Structure

```
BAP_Moseq/
├── README.md                    # This file
├── 12-1-2025_Run/              # December 2025 analysis run
│   ├── All/                    # Combined population processing
│   ├── Moms/                   # Maternal mice processing
│   └── Offsprings/             # Offspring mice processing and analysis
│       ├── code/               # Processing and analysis notebooks
│       ├── data/               # Model outputs and processed data
│       └── figs/               # Generated figures
└── 3-27-2025/                  # March 2025 analysis run
    ├── Data/                   # Shareable data outputs
    │   ├── Fingerprint_Data/   # Raw MoSeq fingerprint data
    │   └── PC_Embeddings/      # Principal component embeddings
    ├── Moms/                   # Maternal mice analysis
    │   ├── code/               # Processing and analysis notebooks
    │   └── data/               # Processed data outputs
    └── Offsprings/             # Offspring mice analysis
        ├── code/               # Processing and analysis notebooks
        ├── data/               # Processed data outputs
        └── figs/               # Generated figures
```

## Methodology

### MoSeq Pipeline

The Motion Sequencing (MoSeq) pipeline extracts quantitative behavioral features from depth-camera recordings of mouse behavior. Key steps include:

1. **Extraction**: Raw depth data processed to extract pose and position information
2. **Modeling**: Autoregressive Hidden Markov Models identify discrete behavioral syllables
3. **Analysis**: Syllable usage, transition probabilities, and scalar metrics quantified

### Population-Specific Models

Separate MoSeq models were trained for each population:
- **Moms Model**: Trained exclusively on maternal behavioral data
- **Offsprings Model**: Trained exclusively on offspring behavioral data

This approach optimizes feature extraction for each population's behavioral repertoire.

### Feature Engineering

Five behavioral metrics are extracted per mouse:
1. **dist_to_center_px**: Spatial positioning relative to arena center
2. **height_ave_mm**: Average height and rearing behavior
3. **length_mm**: Body length measurements
4. **velocity_2d_mm**: Two-dimensional movement velocity
5. **MoSeq**: MoSeq-specific behavioral fingerprints

Each metric produces 99 features, yielding a 495-dimensional feature matrix per mouse.

### Dimensionality Reduction

Principal Component Analysis (PCA) reduces the 495-dimensional feature space:
- **Moms**: 8 principal components optimal
- **Male Offspring**: 25 principal components optimal (75.8% balanced accuracy)
- **Female Offspring**: 12 principal components optimal

## Analysis Runs

### 3-27-2025 Run

The March 2025 run contains the primary analysis with:
- Complete fingerprint data for both populations
- Optimized PC embeddings with classification results
- Comprehensive documentation of methodology

### 12-1-2025 Run

The December 2025 run includes:
- Updated processing notebooks
- New model outputs (model-006-4641589)
- Extended analysis with additional classifier notebooks

## Data Files

### Key Data Formats

| File Type | Description |
|-----------|-------------|
| `moseq_df.csv` | Main MoSeq dataframe with session metadata |
| `stats_df.csv` | Statistical summary per session |
| `*_syllable_counts.csv` | Syllable usage by group |
| `*_transition_matrix.csv` | Syllable transition probabilities |
| `FingerprintSummary_*.csv` | Multi-index behavioral fingerprint data |
| `*_embedding.csv` | Principal component coordinates |

### Identifier System

Each mouse is identified by:
- **uuid**: Universal unique identifier for cross-study compatibility
- **moseq_id**: BAP group-specific identifier (session format)
- **SessionName**: Human-readable session identifier

## Dependencies

- Python 3.x
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- scipy
- statsmodels
- joblib

## Usage

### Loading Embeddings

```python
import pandas as pd

# Load offspring embeddings
df_male = pd.read_csv('3-27-2025/Data/PC_Embeddings/Offsprings/Male/males_final_data.csv')
df_female = pd.read_csv('3-27-2025/Data/PC_Embeddings/Offsprings/Female/females_final_data.csv')

# Access PC coordinates by condition
ee_males = df_male[df_male['category'] == 'EE']
```

### Loading Fingerprint Data

```python
import pandas as pd

# Load multi-index fingerprint data
summary_df = pd.read_csv('path/to/FingerprintSummary_full.csv', 
                         index_col=[0, 1], header=[0, 1])

# Extract specific population
males_df = summary_df[summary_df.index.get_level_values('group').str.contains('Male')]
```

## References

For detailed methodology on Motion Sequencing:
- Wiltschko AB, et al. (2015) Mapping Sub-Second Structure in Mouse Behavior. Neuron.
- Markowitz JE, et al. (2018) The Striatum Organizes 3D Behavior via Moment-to-Moment Action Selection. Cell.

## Contact

For questions regarding this repository, please contact the BAP research group.

