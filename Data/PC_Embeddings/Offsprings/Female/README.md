# Female Offspring PC Embeddings

This directory contains 25-dimensional principal component embeddings for female offspring mice, derived from comprehensive MoSeq behavioral analysis.

## Dataset Specifications

- **Population**: Female offspring mice
- **Sample Size**: [To be specified based on actual data]
- **Principal Components**: 25 PCs (95% variance retention)
- **Environmental Conditions**: 4 conditions (EE, LNB, NGH, SI)
- **Classification Performance**: [To be added when available]

## Files

### `offspring_PCA25_embedding.csv.gz`
Primary data file containing the embeddings in compressed CSV format.

**Schema**:
```
uuid (string): Unique mouse identifier (UUID format)
category (string): Environmental condition {EE, LNB, NGH, SI}
PC1-PC25 (float): Principal component coordinates
```

### `offspring_PCA25_embedding.pkl`
Pickle format for fast loading in Python environments.

### `pca25_logreg.joblib` 
Trained scikit-learn pipeline for reproducibility and predictions on new data.

## Methodology

### Identical Pipeline to Male Offspring
This dataset was processed using the exact same methodology as the male offspring embeddings to ensure comparability:

1. **Feature Extraction**: Same 5 behavioral metrics (495 total features)
2. **Preprocessing**: Identical standardization and padding
3. **Model Architecture**: PCA → Logistic Regression pipeline
4. **Hyperparameter Optimization**: Same grid search parameters
5. **Validation**: 5-fold stratified cross-validation

### Sex-Specific Considerations
- Model trained exclusively on female offspring data
- Hyperparameters optimized specifically for female behavioral patterns
- Allows detection of sex-specific responses to environmental conditions

## Behavioral Metrics

The 25 principal components capture variance from:

1. **Spatial Behavior** (`dist_to_center_px`): 
   - Arena exploration patterns
   - Center vs. periphery preferences
   - Spatial anxiety indicators

2. **Postural Dynamics** (`height_ave_mm`):
   - Rearing behavior
   - Vertical exploration
   - Body posture variations

3. **Morphometric Features** (`length_mm`):
   - Body length measurements
   - Stretching behaviors
   - Size-related movement patterns

4. **Locomotion** (`velocity_2d_mm`):
   - Movement speed patterns
   - Activity levels
   - Locomotor rhythm

5. **MoSeq Features**:
   - Sequence-specific behavioral patterns
   - Temporal dynamics
   - Complex behavioral motifs

## Environmental Conditions

- **EE**: Enriched Environment
  - Enhanced sensory and motor stimulation
  - Complex housing with enrichment items
  
- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]

- **SI**: Social Isolation
  - Individual housing
  - Reduced social stimulation
  - Standard laboratory conditions

## Technical Implementation

### Model Pipeline
```python
from sklearn.pipeline import Pipeline
from sklearn.decomposition import PCA
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler

# Feature standardization
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Optimized pipeline
pipe = Pipeline([
    ('pca', PCA(n_components=0.95, random_state=0)),
    ('clf', LogisticRegression(
        multi_class='multinomial',
        solver='lbfgs',
        penalty='l2',
        max_iter=10_000,
        n_jobs=-1))
])
```

### Hyperparameter Search Space
```python
param_grid = {
    'pca__n_components': [5, 8, 10, 12, 15, 0.90, 0.95],
    'clf__C': np.logspace(-2, 2, 7)  # [0.01, 0.046, 0.215, 1.0, 4.64, 21.54, 100.0]
}
```

## Usage

### Loading Data
```python
import pandas as pd
import numpy as np

# Load female offspring embeddings
df_female = pd.read_csv('offspring_PCA25_embedding.csv.gz')

# Quick info
print(f"Shape: {df_female.shape}")
print(f"Conditions: {df_female['category'].value_counts()}")
```

### Visualization
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Set up plotting
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
fig.suptitle('Female Offspring PC Embeddings', fontsize=16)

# PC1 vs PC2
ax = axes[0, 0]
for condition in df_female['category'].unique():
    mask = df_female['category'] == condition
    ax.scatter(df_female[mask]['PC1'], df_female[mask]['PC2'], 
               label=condition, alpha=0.7, s=60)
ax.set_xlabel('PC1')
ax.set_ylabel('PC2')
ax.legend()
ax.grid(True, alpha=0.3)

# PC1 vs PC3
ax = axes[0, 1]
for condition in df_female['category'].unique():
    mask = df_female['category'] == condition
    ax.scatter(df_female[mask]['PC1'], df_female[mask]['PC3'], 
               label=condition, alpha=0.7, s=60)
ax.set_xlabel('PC1')
ax.set_ylabel('PC3')
ax.legend()
ax.grid(True, alpha=0.3)

# PC2 vs PC3
ax = axes[1, 0]
for condition in df_female['category'].unique():
    mask = df_female['category'] == condition
    ax.scatter(df_female[mask]['PC2'], df_female[mask]['PC3'], 
               label=condition, alpha=0.7, s=60)
ax.set_xlabel('PC2')
ax.set_ylabel('PC3')
ax.legend()
ax.grid(True, alpha=0.3)

# Condition distribution
ax = axes[1, 1]
df_female['category'].value_counts().plot(kind='bar', ax=ax)
ax.set_title('Sample Distribution by Condition')
ax.set_ylabel('Count')
ax.tick_params(axis='x', rotation=45)

plt.tight_layout()
plt.show()
```

### Model Application
```python
import joblib

# Load trained model
model = joblib.load('pca25_logreg.joblib')

# Model components
pca = model.named_steps['pca']
classifier = model.named_steps['clf']

# Get explained variance
explained_var = pca.explained_variance_ratio_
print(f"Cumulative variance explained: {explained_var.cumsum()[-1]:.3f}")

# Feature importance (loadings)
loadings = pca.components_  # Shape: (25, 495)
```

## Performance Metrics

### Cross-Validation Results
- **Validation Method**: 5-fold stratified cross-validation
- **Primary Metric**: Balanced accuracy (accounts for class imbalance)
- **Results**: [To be added when available]

### Model Interpretability
- **Variance Explained**: 95% by 25 principal components
- **Feature Loadings**: Available in trained model
- **Separability**: [Add condition separability analysis when available]

## Quality Assurance

- **Reproducibility**: Fixed random state (random_state=0)
- **Validation**: Stratified cross-validation ensures balanced sampling
- **Feature Engineering**: Consistent pipeline across all experimental groups
- **Model Saving**: Complete pipeline saved for future use

## Comparison with Male Offspring

- **Same Methodology**: Identical processing pipeline for direct comparison
- **Same PC Count**: 25 components for consistent dimensionality
- **Performance Comparison**: [To be added when both analyses complete]

## Notes

- Embeddings capture 95% of original behavioral variance in 25 dimensions
- Model optimized specifically for female behavioral patterns
- Compatible with male offspring embeddings for comparative analysis
- All preprocessing steps documented for reproducibility

## Related Documentation

- `../Male/README.md`: Male offspring methodology and results
- `../README.md`: Offspring-specific documentation
- `../../README.md`: Complete methodology overview