# BAP_Moseq: Principal Component Embeddings for Mouse Behavior Analysis

## Overview

This repository contains principal component (PC) embeddings derived from Motion Sequencing (MoSeq) behavioral data for mice exposed to different environmental conditions. The embeddings were created by aggregating multiple behavioral metrics from the MoSeq pipeline and reducing them to a lower-dimensional latent space suitable for classification and analysis.

## Data Structure

The repository is organized hierarchically by experimental groups:

```
Data/
├── Fingerprint_Data/
│   ├── Moms/
│   │   ├── FingerprintSummary_full.csv.zip (Mom-specific MoSeq model)
│   │   └── README.md
│   ├── Offsprings/
│   │   ├── FingerprintSummary_full.csv.zip (Offspring-specific MoSeq model)
│   │   └── README.md
│   └── README.md (495-feature matrix documentation)
└── PC_Embeddings/
    ├── Moms/
    │   ├── moms_PCA8_embedding.csv.gz
    │   └── README.md
    └── Offsprings/
        ├── Female/
        │   ├── females_final_data.csv (12 PCs)
        │   └── README.md
        ├── Male/
        │   ├── males_final_data.csv (25 PCs)
        │   └── README.md
        └── README.md
```

## Experimental Conditions

The mice were exposed to four different environmental conditions:

- **EE**: Enriched Environment
- **LNB**: Limited Nesting and Bedding
- **NGH**: Normal Growth Housing
- **SI**: Social Isolation

## Methodology Overview

### 1. Data Aggregation
- Started with **population-specific MoSeq pipeline outputs** (`Data/Fingerprint_Data/`)
  - Separate MoSeq models trained for Moms and Offsprings
  - Population-specific behavioral feature optimization
- Processed multi-index fingerprint summary data into 495-dimensional feature matrix
- Aggregated multiple behavioral metrics including:
  - `dist_to_center_px`: Distance to center (pixels) - 99 features
  - `height_ave_mm`: Average height (mm) - 99 features
  - `length_mm`: Length measurements (mm) - 99 features
  - `velocity_2d_mm`: 2D velocity (mm) - 99 features
  - `MoSeq`: MoSeq-specific behavioral features - 99 features

### 2. Feature Engineering
- Combined all behavioral metrics into a unified feature matrix
- Applied standardization (StandardScaler) to normalize features
- Padded feature vectors to ensure consistent dimensionality across metrics

### 3. Dimensionality Reduction
- Applied Principal Component Analysis (PCA) with hyperparameter tuning
- Used grid search with stratified cross-validation to optimize:
  - Number of principal components (5, 8, 10, 12, 15, or variance retention 0.90, 0.95)
  - Logistic regression regularization parameter (C)

### 4. Classification Pipeline
- Built sklearn Pipeline: PCA → Logistic Regression
- Used multinomial logistic regression for multi-class classification
- Evaluated using balanced accuracy with 5-fold stratified cross-validation

## File Formats

### Embedding Files (`.csv`)
Each embedding file contains:
- **uuid**: Universal unique identifier for each mouse (UUID format)
- **moseq_id**: BAP group identifier used for internal tracking (session format: session_YYYYMMDDHHMMSS)
- **category**: Experimental condition (EE, LNB, NGH, SI)
- **PC1, PC2, ..., PCn**: Principal component coordinates

**Note**: The dual identifier system allows both universal cross-study compatibility (uuid) and BAP group-specific tracking (moseq_id).

### Model Files (`.joblib`)
Saved trained models for reproducibility and future predictions.

## Usage

The embeddings can be loaded using pandas:

```python
import pandas as pd

# Load embeddings
df = pd.read_csv('path/to/embedding.csv')

# Data includes dual identifiers
print("Identifiers available:")
print(f"- uuid: {df['uuid'].iloc[0]}")  # UUID format
print(f"- moseq_id: {df['moseq_id'].iloc[0]}")  # BAP group session format
```

## Performance Metrics

Classification performance varies by group:
- **Male Offspring**: 75.8% accuracy, 74.1% balanced recall with 25 PCs (33 samples)
- **Female Offspring**: 69.7% accuracy, 66.4% balanced recall with 12 PCs (33 samples)
- **Moms**: [Performance with 8 PCs to be specified]

Note: Different groups required different numbers of principal components for optimal classification performance.

## Dependencies

- Python 3.x
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn

## Contact

[Add contact information or collaboration details]

## Citation

[Add citation information when available]