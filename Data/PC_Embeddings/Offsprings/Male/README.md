# Male Offspring PC Embeddings

This directory contains 25-dimensional principal component embeddings for male offspring mice, derived from comprehensive MoSeq behavioral analysis.

## Dataset Specifications

- **Population**: Male offspring mice
- **Sample Size**: 33 mice
- **Principal Components**: 25 PCs (optimal for male behavioral patterns)
- **Environmental Conditions**: 4 conditions (EE, LNB, NGH, SI)
- **Classification Performance**: 75.8% balanced accuracy (from offspring-specific MoSeq model)

## Files

### `males_final_data.csv`
Primary data file containing the 25-dimensional embeddings.

**Schema**:
```
uuid (string): Universal unique identifier (UUID format)
moseq_id (string): BAP group identifier (session format: session_YYYYMMDDHHMMSS)
category (string): Environmental condition {EE, LNB, NGH, SI}
PC1-PC25 (float): Principal component coordinates
```

### Dual Identifier System
- **uuid**: Universal identifier for cross-study compatibility
- **moseq_id**: BAP group-specific identifier used for internal tracking and session identification

## Methodology Details

### Data Processing Pipeline

1. **Source Data Loading**:
   ```python
   # Load from offspring-specific MoSeq model
   summary_df = pd.read_csv('../../Fingerprint_Data/Offsprings/FingerprintSummary_full.csv.zip', 
                           index_col=[0, 1], header=[0, 1])
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

**Confusion Matrix** (Cross-validated):
```
         Predicted Class
True     EE    LNB   NGH   SI
EE      0.83  0.00  0.17  0.00
LNB     0.00  0.71  0.14  0.14  
NGH     0.00  0.08  0.85  0.08
SI      0.00  0.00  0.43  0.57
```

**Key Observations**:
- EE condition shows excellent precision (100%) with good recall (83%)
- NGH condition has highest recall (85%) but moderate precision (69%)
- SI condition shows most confusion, primarily with NGH (43% misclassification)
- LNB shows balanced performance across precision and recall

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
df = pd.read_csv('males_final_data.csv')

# Check dual identifiers
print("Available identifiers:")
print(f"UUID example: {df['uuid'].iloc[0]}")
print(f"MoSeq ID example: {df['moseq_id'].iloc[0]}")

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

1. **Load source data**: Offspring-specific MoSeq fingerprint data (`../../Fingerprint_Data/Offsprings/`)
2. **Apply preprocessing**: Feature extraction and standardization (see notebook)
3. **Train model**: Use provided hyperparameters or re-optimize
4. **Generate embeddings**: Transform standardized features through trained PCA

**Important**: Use the offspring-specific fingerprint data, not the maternal model data, to ensure population-appropriate feature extraction.

**Key Parameters**:
- PCA components: 25 (95% variance retention)
- Logistic regression C: 4.641588833612777
- Random state: 0 (for reproducibility)

## Related Files

- `../Female/`: Female offspring embeddings
- `../../Moms/`: Maternal mice embeddings
- See repository root for complete methodology documentation