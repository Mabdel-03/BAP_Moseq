# Principal Component Embeddings

This directory contains the final principal component embeddings for different experimental groups of mice analyzed using the MoSeq behavioral pipeline.

## Overview

The embeddings represent a dimensionally-reduced latent space derived from comprehensive behavioral fingerprints. Each embedding captures the essential behavioral patterns that distinguish between different environmental conditions.

## Methodology

### Feature Engineering Pipeline

1. **Source Data**: MoSeq fingerprint summary data (`FingerprintSummary_full.csv`)
2. **Metric Extraction**: Five behavioral metrics per mouse:
   - Distance to center (pixels)
   - Average height (mm) 
   - Length measurements (mm)
   - 2D velocity (mm)
   - MoSeq behavioral features

3. **Data Processing**:
   - Reformatted multi-index data to single-index format
   - Padded all feature vectors to 99 dimensions
   - Concatenated into unified 495-feature matrix
   - Applied StandardScaler normalization

4. **Dimensionality Reduction**:
   - Principal Component Analysis (PCA)
   - Hyperparameter tuning via GridSearchCV
   - Stratified 5-fold cross-validation
   - Optimization metric: balanced accuracy

### Model Architecture

```
Input Features (495D) → StandardScaler → PCA → Logistic Regression
```

**PCA Hyperparameters Tested**:
- Components: [5, 8, 10, 12, 15, 0.90, 0.95] (variance retention)
- Logistic Regression C: [0.01, 0.046, 0.215, 1.0, 4.64, 21.54, 100.0]

## Experimental Groups

### Moms (`Moms/`)
- **Subjects**: Maternal mice 
- **Optimal Components**: 8 PCs
- **Sample Size**: [To be specified]

### Offsprings (`Offsprings/`)
- **Males** (`Male/`): 25 PCs, 33 samples, 75.8% balanced accuracy
- **Females** (`Female/`): 12 PCs, 33 samples, [performance to be specified]

**Note**: Different optimal PC counts reflect sex-specific behavioral patterns.

## Environmental Conditions

All groups were exposed to four experimental conditions:

- **EE**: Enriched Environment
- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]  
- **SI**: Social Isolation

## File Structure

Each group directory contains:
- `*_embedding.csv.gz`: Compressed CSV with embeddings
- `*_embedding.pkl`: Pickle file for fast Python loading
- `*.joblib`: Trained model for reproducibility
- `README.md`: Group-specific documentation

## Data Schema

All embedding files follow the same schema:

| Column | Type | Description |
|--------|------|-------------|
| `uuid` | string | Universal unique identifier |
| `moseq_id` | string | BAP group identifier (session format) |
| `category` | string | Environmental condition (EE/LNB/NGH/SI) |
| `PC1` | float | First principal component |
| `PC2` | float | Second principal component |
| `...` | float | Additional principal components |
| `PCn` | float | Final principal component (varies: 8/12/25) |

## Usage Examples

### Loading Data
```python
import pandas as pd

# Load male offspring embeddings (25 PCs)
df_male = pd.read_csv('Offsprings/Male/males_final_data.csv')

# Load female offspring embeddings (12 PCs)  
df_female = pd.read_csv('Offsprings/Female/females_final_data.csv')
```

### Basic Analysis
```python
# Check data shape
print(f"Shape: {df_male.shape}")

# View condition distribution
print(df_male['category'].value_counts())

# Access PC coordinates
pc_coords = df_male[['PC1', 'PC2', 'PC3']].values
```

### Visualization
```python
import matplotlib.pyplot as plt

# 2D scatter plot
fig, ax = plt.subplots(figsize=(8, 6))
for condition in df_male['category'].unique():
    mask = df_male['category'] == condition
    ax.scatter(df_male[mask]['PC1'], df_male[mask]['PC2'], 
               label=condition, alpha=0.7)
ax.set_xlabel('PC1')
ax.set_ylabel('PC2')
ax.legend()
plt.show()
```

## Quality Metrics

The embeddings were validated using:
- **Cross-validation**: 5-fold stratified
- **Metric**: Balanced accuracy (accounts for class imbalance)
- **Performance**: Varies by group (see individual READMEs)

## Notes

- All embeddings use the same feature engineering pipeline for consistency
- Principal component counts were optimized per group based on classification performance
- Models are saved for reproducibility and future predictions