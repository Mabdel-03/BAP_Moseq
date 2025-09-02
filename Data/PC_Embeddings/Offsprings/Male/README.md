# Male Offspring PC Embeddings

This directory contains 25-dimensional principal component embeddings for male offspring mice, derived from comprehensive MoSeq behavioral analysis.

## Dataset Specifications

- **Population**: Male offspring mice
- **Sample Size**: 33 mice
- **Principal Components**: 25 PCs (95% variance retention)
- **Environmental Conditions**: 4 conditions (EE, LNB, NGH, SI)
- **Classification Performance**: 77.5% balanced accuracy

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

## Methodology Details

### Data Processing Pipeline

1. **Source Data Loading**:
   ```python
   summary_df = pd.read_csv('FingerprintSummary_full.csv', index_col=[0, 1], header=[0, 1])
   males_df = summary_df[summary_df.index.get_level_values('group').str.contains('Male')]
   ```

2. **Behavioral Metrics Extraction**:
   - `dist_to_center_px`: Spatial positioning relative to arena center
   - `height_ave_mm`: Average height during behavioral sequences
   - `length_mm`: Body length measurements
   - `velocity_2d_mm`: Two-dimensional velocity metrics
   - `MoSeq`: MoSeq-specific behavioral fingerprints

3. **Feature Engineering**:
   ```python
   # Reformat multi-index data
   m_pos_df = reformat_df(males_df, 'dist_to_center_px')
   m_height_df = reformat_df(males_df, 'height_ave_mm')
   m_length_df = reformat_df(males_df, 'length_mm')
   m_vel_df = reformat_df(males_df, 'velocity_2d_mm')
   m_moseq_df = reformat_df(males_df, 'MoSeq')
   
   # Aggregate per mouse
   fingerprints = {
       'dist_to_center_px': np.vstack(m_pos_list),
       'height_ave_mm': np.vstack(m_height_list),
       'length_mm': np.vstack(m_length_list),
       'velocity_2d_mm': np.vstack(m_vel_list),
       'MoSeq': np.vstack(m_moseq_list)
   }
   ```

4. **Feature Matrix Construction**:
   - Padded all feature vectors to 99 dimensions
   - Concatenated: 5 metrics × 99 features = 495 total features
   - Applied StandardScaler normalization

5. **Model Training**:
   ```python
   pipe = Pipeline([
       ('pca', PCA(random_state=0)),
       ('clf', LogisticRegression(
           multi_class='multinomial',
           solver='lbfgs',
           penalty='l2',
           max_iter=10_000,
           n_jobs=-1))
   ])
   
   param_grid = {
       'pca__n_components': [5, 8, 10, 12, 15, 0.90, 0.95],
       'clf__C': np.logspace(-2, 2, 7)
   }
   ```

### Hyperparameter Optimization

**Grid Search Results**:
- **Best Parameters**: 
  - `pca__n_components`: 0.95 (95% variance retention → 25 components)
  - `clf__C`: 4.641588833612777
- **Cross-Validation**: 5-fold stratified
- **Best Score**: 77.5% balanced accuracy

### Class Distribution and Performance

**Per-Class Results**:
```
              precision    recall  f1-score   support
        EE      1.000     0.833     0.909         6
       LNB      0.833     0.714     0.769         7
       NGH      0.688     0.846     0.759        13
        SI      0.667     0.571     0.615         7

  accuracy                          0.758        33
 macro avg      0.797     0.741     0.763        33
weighted avg    0.771     0.758     0.758        33
```

## Statistical Analysis

### Principal Component Significance
ANOVA testing revealed significant differences between conditions:
- **PC1**: F = 13.79, p < 0.0001 (highly significant)
- **PC2**: F = 2.27, p = 0.1015 (not significant)
- **PC3**: F = 4.90, p = 0.0071 (significant)

### Post-hoc Analysis
Tukey HSD multiple comparisons for PC1 showed significant differences between:
- EE vs LNB (p = 0.0022)
- EE vs NGH (p = 0.0005)
- EE vs SI (p < 0.001)
- NGH vs SI (p = 0.0452)

## Usage Examples

### Loading and Basic Analysis
```python
import pandas as pd
import numpy as np

# Load the embeddings
df = pd.read_csv('offspring_PCA25_embedding.csv.gz')

# Basic statistics
print(f"Dataset shape: {df.shape}")
print(f"Conditions: {df['category'].unique()}")
print(f"Sample sizes per condition:")
print(df['category'].value_counts())

# Extract PC coordinates
pc_coords = df[[f'PC{i+1}' for i in range(25)]].values
labels = df['category'].values
```

### Visualization
```python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# 2D visualization
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# PC1 vs PC2
for condition in df['category'].unique():
    mask = df['category'] == condition
    ax1.scatter(df[mask]['PC1'], df[mask]['PC2'], 
                label=condition, alpha=0.7, s=60)
ax1.set_xlabel('PC1')
ax1.set_ylabel('PC2')
ax1.set_title('Male Offspring - PC1 vs PC2')
ax1.legend()
ax1.grid(True, alpha=0.3)

# 3D visualization
ax2 = fig.add_subplot(122, projection='3d')
for condition in df['category'].unique():
    mask = df['category'] == condition
    ax2.scatter(df[mask]['PC1'], df[mask]['PC2'], df[mask]['PC3'],
                label=condition, alpha=0.7, s=60)
ax2.set_xlabel('PC1')
ax2.set_ylabel('PC2')
ax2.set_zlabel('PC3')
ax2.set_title('Male Offspring - PC1-PC3')
ax2.legend()

plt.tight_layout()
plt.show()
```

### Model Usage
```python
import joblib

# Load trained model
model = joblib.load('pca25_logreg.joblib')

# Make predictions on new data (after same preprocessing)
# predictions = model.predict(new_data_scaled)
# probabilities = model.predict_proba(new_data_scaled)
```

## Data Quality Notes

- All mice successfully classified into one of four environmental conditions
- Feature matrix standardized to prevent scale-dependent bias
- Cross-validation ensures robust performance estimates
- Model achieves good separation between most condition pairs

## Reproducibility

To reproduce these embeddings:

1. **Load source data**: MoSeq fingerprint summary data
2. **Apply preprocessing**: Feature extraction and standardization (see notebook)
3. **Train model**: Use provided hyperparameters or re-optimize
4. **Generate embeddings**: Transform standardized features through trained PCA

**Key Parameters**:
- PCA components: 25 (95% variance retention)
- Logistic regression C: 4.641588833612777
- Random state: 0 (for reproducibility)

## Related Files

- `../Female/`: Female offspring embeddings
- `../../Moms/`: Maternal mice embeddings
- See repository root for complete methodology documentation