# Offspring Mice PC Embeddings

This directory contains principal component embeddings for offspring mice, stratified by sex and exposed to different environmental conditions.

## Dataset Overview

- **Population**: Offspring mice (stratified by sex)
- **Principal Components**: 25 PCs (optimized for both male and female groups)
- **Environmental Conditions**: EE, LNB, NGH, SI
- **Stratification**: Male and Female subgroups

## Directory Structure

```
Offsprings/
├── Male/
│   ├── offspring_PCA25_embedding.csv.gz
│   ├── offspring_PCA25_embedding.pkl
│   ├── pca25_logreg.joblib
│   └── README.md
├── Female/
│   ├── offspring_PCA25_embedding.csv.gz
│   ├── offspring_PCA25_embedding.pkl
│   ├── pca25_logreg.joblib
│   └── README.md
└── README.md (this file)
```

## Sex-Stratified Analysis

### Why Sex Stratification?
Sex stratification was performed because:
1. **Biological Differences**: Male and female mice may exhibit different behavioral responses to environmental conditions
2. **Statistical Power**: Separate analysis allows for detection of sex-specific effects
3. **Model Performance**: Individual models may achieve better classification accuracy within sex groups

### Shared Methodology
Both male and female embeddings use identical processing:
- Same 5 behavioral metrics
- Same feature engineering pipeline
- Same PCA hyperparameter optimization
- Same validation approach (5-fold stratified CV)

## Environmental Conditions

All offspring were exposed to one of four conditions:

- **EE**: Enriched Environment
  - Enhanced sensory stimulation
  - Increased motor challenges
  - Social interaction opportunities

- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]

- **SI**: Social Isolation
  - Reduced social contact
  - Standard housing conditions
  - Minimal environmental enrichment

## Technical Specifications

### Feature Pipeline
1. **Raw Metrics** (5 types × 99 features = 495 total):
   - `dist_to_center_px`: Spatial positioning metrics
   - `height_ave_mm`: Height-based behavioral measures
   - `length_mm`: Length-based measurements
   - `velocity_2d_mm`: Movement velocity features
   - `MoSeq`: MoSeq-specific behavioral fingerprints

2. **Preprocessing**:
   - Feature padding to ensure consistent dimensionality
   - StandardScaler normalization (zero mean, unit variance)

3. **Dimensionality Reduction**:
   - PCA with hyperparameter optimization
   - Components tested: [5, 8, 10, 12, 15, 0.90, 0.95]
   - Optimal: 25 components (95% variance retention)

### Classification Pipeline
```python
from sklearn.pipeline import Pipeline
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression

pipe = Pipeline([
    ('pca', PCA(random_state=0)),
    ('clf', LogisticRegression(
        multi_class='multinomial',
        solver='lbfgs',
        penalty='l2',
        max_iter=10_000,
        n_jobs=-1))
])
```

## Performance Summary

### Male Offspring
- **Balanced Accuracy**: 77.5%
- **Sample Size**: 33 mice
- **Principal Components**: 25 (95% variance retention)
- **Regularization**: C = 4.64

### Female Offspring
- **Performance**: [To be added when available]
- **Sample Size**: [To be specified]
- **Principal Components**: 25

## Usage

### Loading Both Datasets
```python
import pandas as pd

# Load both male and female embeddings
df_male = pd.read_csv('Male/offspring_PCA25_embedding.csv.gz')
df_female = pd.read_csv('Female/offspring_PCA25_embedding.csv.gz')

# Combine if needed
df_combined = pd.concat([
    df_male.assign(sex='Male'),
    df_female.assign(sex='Female')
], ignore_index=True)
```

### Comparative Analysis
```python
import matplotlib.pyplot as plt

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Plot male embeddings
for condition in df_male['category'].unique():
    mask = df_male['category'] == condition
    ax1.scatter(df_male[mask]['PC1'], df_male[mask]['PC2'], 
                label=condition, alpha=0.7)
ax1.set_title('Male Offspring')
ax1.set_xlabel('PC1')
ax1.set_ylabel('PC2')
ax1.legend()

# Plot female embeddings
for condition in df_female['category'].unique():
    mask = df_female['category'] == condition
    ax2.scatter(df_female[mask]['PC1'], df_female[mask]['PC2'], 
                label=condition, alpha=0.7)
ax2.set_title('Female Offspring')
ax2.set_xlabel('PC1')
ax2.set_ylabel('PC2')
ax2.legend()

plt.tight_layout()
plt.show()
```

## Validation

- **Cross-Validation**: 5-fold stratified to ensure balanced representation
- **Metric**: Balanced accuracy (accounts for potential class imbalance)
- **Reproducibility**: Random state fixed (random_state=0) for consistent results

## Related Documentation

- `../README.md`: Overall PC embeddings methodology
- `../../README.md`: Complete data processing pipeline
- Individual sex-specific READMEs in `Male/` and `Female/` subdirectories