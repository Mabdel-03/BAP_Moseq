# Maternal Mice PC Embeddings

This directory contains principal component embeddings for maternal mice exposed to different environmental conditions.

## Dataset Overview

- **Population**: Maternal mice
- **Principal Components**: 8 PCs
- **Environmental Conditions**: EE, LNB, NGH, SI
- **Sample Size**: [To be specified based on actual data]

## Files

### `moms_PCA8_embedding.csv.gz`
Compressed CSV file containing the 8-dimensional principal component embeddings.

**Columns**:
- `uuid`: Unique identifier for each mouse
- `category`: Environmental condition (EE, LNB, NGH, SI)
- `PC1` through `PC8`: Principal component coordinates

### Additional Files
- `moms_PCA8_embedding.pkl`: Pickle format for fast Python loading
- `*.joblib`: Trained model files (if available)

## Methodology

### Data Source
- **Input**: MoSeq fingerprint summary data for maternal mice
- **Metrics**: 5 behavioral measures (distance to center, height, length, velocity, MoSeq features)
- **Total Features**: 495 (99 features × 5 metrics)

### Model Optimization
- **Algorithm**: PCA followed by multinomial logistic regression
- **Hyperparameter Tuning**: GridSearchCV with 5-fold stratified cross-validation
- **Optimal Components**: 8 PCs selected based on classification performance
- **Preprocessing**: StandardScaler normalization

### Performance
- **Validation Method**: 5-fold stratified cross-validation
- **Metric**: Balanced accuracy
- **Results**: [To be added when available]

## Usage

### Loading the Data
```python
import pandas as pd

# Load embeddings
df_moms = pd.read_csv('moms_PCA8_embedding.csv.gz')

# Faster loading with pickle
df_moms = pd.read_pickle('moms_PCA8_embedding.pkl')
```

### Basic Statistics
```python
# Dataset shape
print(f"Dataset shape: {df_moms.shape}")

# Condition distribution
print("Condition distribution:")
print(df_moms['category'].value_counts())

# Principal component summary
pc_cols = [col for col in df_moms.columns if col.startswith('PC')]
print(f"Principal components: {len(pc_cols)}")
```

### Visualization
```python
import matplotlib.pyplot as plt
import seaborn as sns

# 2D visualization
plt.figure(figsize=(10, 8))
for condition in df_moms['category'].unique():
    mask = df_moms['category'] == condition
    plt.scatter(df_moms[mask]['PC1'], df_moms[mask]['PC2'], 
                label=condition, alpha=0.7, s=60)

plt.xlabel('PC1')
plt.ylabel('PC2')
plt.title('Maternal Mice - PC1 vs PC2')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## Environmental Conditions

- **EE**: Enriched Environment - Enhanced sensory and motor stimulation
- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]
- **SI**: Social Isolation - Reduced social interaction

## Notes

- Embeddings created using the same pipeline as offspring data for consistency
- 8 PCs were optimal for maternal mice classification
- All preprocessing steps identical across experimental groups
- Model files available for reproducing results or making predictions on new data

## Related Files

- See `../Offsprings/` for offspring embeddings
- Main methodology documented in repository root README.md