# Female Offspring PC Embeddings

This directory contains 12-dimensional principal component embeddings for female offspring mice, derived from comprehensive MoSeq behavioral analysis.

## Dataset Specifications

- **Population**: Female offspring mice
- **Sample Size**: 33 mice
- **Principal Components**: 12 PCs (optimal for female behavioral patterns)
- **Environmental Conditions**: 4 conditions (EE, LNB, NGH, SI)
- **Classification Performance**: 69.7% accuracy, 66.4% balanced recall with 12 PCs

## Files

### `females_final_data.csv`
Primary data file containing the 12-dimensional embeddings.

**Schema**:
```
uuid (string): Universal unique identifier (UUID format)
moseq_id (string): BAP group identifier (session format: session_YYYYMMDDHHMMSS)
category (string): Environmental condition {EE, LNB, NGH, SI}
PC1-PC12 (float): Principal component coordinates (optimal for females)
```

### Dual Identifier System
- **uuid**: Universal identifier for cross-study compatibility
- **moseq_id**: BAP group-specific identifier used for internal tracking and session identification

## Methodology

### Identical Pipeline to Male Offspring
This dataset was processed using the exact same methodology as the male offspring embeddings to ensure comparability:

1. **Source Data**: Same offspring-specific MoSeq model (`../../Fingerprint_Data/Offsprings/`)
2. **Feature Extraction**: Same 5 behavioral metrics (495 total features)
3. **Preprocessing**: Identical standardization and padding
4. **Model Architecture**: PCA → Logistic Regression pipeline
5. **Hyperparameter Optimization**: Same grid search parameters
6. **Validation**: 5-fold stratified cross-validation

### Sex-Specific Optimization
- Model trained exclusively on female offspring data
- **Optimal Components**: 12 PCs (vs. 25 PCs for males)
- Reflects different behavioral complexity patterns between sexes
- Allows detection of sex-specific responses to environmental conditions

**Confusion Matrix** (Cross-validated with 12 PCs):
```
         Predicted Class
True     EE    LNB   NGH   SI
EE      0.67  0.00  0.33  0.00
LNB     0.00  0.29  0.71  0.00  
NGH     0.00  0.15  0.85  0.00
SI      0.00  0.00  0.14  0.86
```

**Key Observations**:
- SI condition shows excellent performance (100% precision, 86% recall)
- EE condition shows perfect precision (100%) but moderate recall (67%)
- NGH condition has high recall (85%) but lower precision (58%)
- LNB shows challenging classification (50% precision, 29% recall)
- Overall: Different confusion patterns compared to males, with better SI separation

## Behavioral Metrics

The 12 principal components capture variance from:

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
  - Social interaction opportunities
  
- **LNB**: Limited Nesting and Bedding
  - Restricted nesting materials
  - Minimal bedding provision
  - Reduced comfort materials

- **NGH**: Normal Growth Housing
  - Standard laboratory housing conditions
  - Normal nesting and bedding materials
  - Baseline control condition

- **SI**: Social Isolation
  - Individual housing
  - Reduced social stimulation
  - Minimal environmental enrichment

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
df_female = pd.read_csv('females_final_data.csv')

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
loadings = pca.components_  # Shape: (12, 495)
```

## Performance Metrics

### Cross-Validation Results
- **Validation Method**: 5-fold stratified cross-validation
- **Primary Metric**: Balanced accuracy (accounts for class imbalance)
- **Optimal Components**: 12 PCs
- **Overall Accuracy**: 69.7%
- **Balanced Recall**: 66.4%
- **Macro F1-Score**: 69.4%
- **Source Model**: Offspring-specific MoSeq training (separate from Moms model)

**Per-Class Performance**:
```
              precision    recall  f1-score   support
        EE      1.000     0.667     0.800         6
       LNB      0.500     0.286     0.364         7
       NGH      0.579     0.846     0.688        13
        SI      1.000     0.857     0.923         7

  accuracy                          0.697        33
 macro avg      0.770     0.664     0.694        33
weighted avg    0.728     0.697     0.689        33
```

### Model Interpretability
- **Variance Explained**: Optimized with 12 principal components
- **Feature Loadings**: Available in trained model
- **Separability**: Better SI and NGH separation compared to males

## Quality Assurance

- **Reproducibility**: Fixed random state (random_state=0)
- **Validation**: Stratified cross-validation ensures balanced sampling
- **Feature Engineering**: Consistent pipeline across all experimental groups
- **Model Saving**: Complete pipeline saved for future use

## Comparison with Male Offspring

- **Same Methodology**: Identical processing pipeline for direct comparison
- **Different PC Count**: 12 components (vs. 25 for males) - reflects sex-specific behavioral complexity
- **Performance Comparison**: [To be added when female analysis complete]

## Notes

- Embeddings optimized to 12 dimensions for female-specific behavioral patterns
- Model optimized specifically for female behavioral patterns
- Compatible with male offspring embeddings for comparative analysis
- All preprocessing steps documented for reproducibility

## Related Documentation

- `../Male/README.md`: Male offspring methodology and results
- `../README.md`: Offspring-specific documentation
- `../../README.md`: Complete methodology overview