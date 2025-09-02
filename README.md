# BAP_Moseq: Principal Component Embeddings for Mouse Behavior Analysis

## Overview

This repository contains principal component (PC) embeddings derived from Motion Sequencing (MoSeq) behavioral data for mice exposed to different environmental conditions. The embeddings were created by aggregating multiple behavioral metrics from the MoSeq pipeline and reducing them to a lower-dimensional latent space suitable for classification and analysis.

## Data Structure

The repository is organized hierarchically by experimental groups:

```
Data/
├── PC_Embeddings/
│   ├── Moms/
│   │   ├── moms_PCA8_embedding.csv.gz
│   │   └── README.md
│   └── Offsprings/
│       ├── Female/
│       │   ├── offspring_PCA25_embedding.csv.gz
│       │   └── README.md
│       ├── Male/
│       │   ├── offspring_PCA25_embedding.csv.gz
│       │   └── README.md
│       └── README.md
```

## Experimental Conditions

The mice were exposed to four different environmental conditions:

- **EE**: Enriched Environment
- **LNB**: [Description needed]
- **NGH**: [Description needed]  
- **SI**: Social Isolation

## Methodology Overview

### 1. Data Aggregation
- Started with MoSeq pipeline output containing behavioral fingerprints
- Aggregated multiple behavioral metrics including:
  - `dist_to_center_px`: Distance to center (pixels)
  - `height_ave_mm`: Average height (mm)
  - `length_mm`: Length measurements (mm)
  - `velocity_2d_mm`: 2D velocity (mm)
  - `MoSeq`: MoSeq-specific behavioral features

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

### Embedding Files (`.csv.gz`)
Each embedding file contains:
- **uuid**: Unique identifier for each mouse
- **category**: Experimental condition (EE, LNB, NGH, SI)
- **PC1, PC2, ..., PCn**: Principal component coordinates

### Model Files (`.joblib`)
Saved trained models for reproducibility and future predictions.

## Usage

The embeddings can be loaded using pandas:

```python
import pandas as pd

# Load embeddings
df = pd.read_csv('path/to/embedding.csv.gz')

# Or load from pickle for faster access
df = pd.read_pickle('path/to/embedding.pkl')
```

## Performance Metrics

Classification performance varies by group:
- Male Offsprings: ~77.5% balanced accuracy with 25 PCs
- [Add metrics for other groups as available]

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