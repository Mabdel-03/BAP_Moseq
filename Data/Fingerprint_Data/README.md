# MoSeq Fingerprint Data

This directory contains the raw fingerprint data from the MoSeq behavioral analysis pipeline and detailed documentation of how it was processed into the 495-dimensional feature matrix used for principal component analysis.

## Overview

The fingerprint data represents the output of the MoSeq (Motion Sequencing) behavioral analysis pipeline, which extracts quantitative behavioral features from mouse movement data. This raw data serves as the foundation for all subsequent principal component embeddings.

## Source Files

### `FingerprintSummary_full.csv`
The primary input file containing behavioral fingerprints for all mice across different experimental groups.

**File Structure**: Multi-index DataFrame
- **Index Level 0**: Group identifier (contains sex and condition information)
- **Index Level 1**: Mouse UUID
- **Columns Level 0**: Behavioral metric type
- **Columns Level 1**: Feature index/identifier

### `FingerprintRangeDict_full.csv`
Metadata file containing range and scaling information for the behavioral metrics.

## Data Structure Analysis

### Multi-Index Format
The source data uses a hierarchical (multi-index) structure:

```python
# Loading structure
summary_df = pd.read_csv('FingerprintSummary_full.csv', 
                         index_col=[0, 1],    # Group and UUID
                         header=[0, 1])       # Metric and feature index

# Example structure:
#                                    dist_to_center_px  height_ave_mm  length_mm  velocity_2d_mm  MoSeq
# group        uuid                  0    1    2  ...  0    1    2... 0   1  2... 0    1    2...  0  1  2...
# Male_EE      uuid1                val  val  val ... val  val  val... val val... val  val  val... val...
# Male_LNB     uuid2                val  val  val ... val  val  val... val val... val  val  val... val...
# ...
```

### Behavioral Metrics

The fingerprint data contains five primary behavioral metrics:

#### 1. `dist_to_center_px` (Distance to Center)
- **Description**: Spatial positioning relative to behavioral arena center
- **Features**: 99-dimensional vector per mouse
- **Biological Meaning**: 
  - Center preference vs. thigmotaxis (wall-hugging)
  - Spatial anxiety indicators
  - Exploration patterns

#### 2. `height_ave_mm` (Average Height)
- **Description**: Vertical positioning and rearing behavior metrics
- **Features**: 99-dimensional vector per mouse  
- **Biological Meaning**:
  - Rearing frequency and duration
  - Vertical exploration behavior
  - Postural dynamics

#### 3. `length_mm` (Body Length)
- **Description**: Body length measurements during behavioral sequences
- **Features**: 99-dimensional vector per mouse
- **Biological Meaning**:
  - Stretching behaviors
  - Body posture variations
  - Size-related movement patterns

#### 4. `velocity_2d_mm` (2D Velocity)
- **Description**: Two-dimensional movement velocity metrics
- **Features**: 99-dimensional vector per mouse
- **Biological Meaning**:
  - Locomotor activity levels
  - Movement speed patterns
  - Activity rhythm indicators

#### 5. `MoSeq` (MoSeq Features)
- **Description**: MoSeq-specific behavioral fingerprints
- **Features**: 99-dimensional vector per mouse
- **Biological Meaning**:
  - Complex behavioral motifs
  - Sequence-specific patterns
  - Temporal dynamics of behavior

## Feature Matrix Construction

### Step 1: Data Reformatting
The multi-index structure was reformatted into single-index DataFrames for each behavioral metric:

```python
def reformat_df(subset_df, metric_interest):
    """
    Take multi-index summary dataframe from Moseq fingerprint code
    Reformat into single index dataframe for further analysis
    Returns dataframe with category label, position, uuid, and avg_time
    """
    category = []
    position = []
    uuid_num = []
    avg_time = []
    
    for ix in subset_df[metric_interest]:
        for cat, uuid in subset_df[metric_interest][ix].index:
            category.append(cat)
            position.append(ix)
            uuid_num.append(uuid)
            avg_time.append(subset_df[metric_interest][ix][cat][uuid])
    
    return rename_groups(pd.DataFrame({
        'group': category, 
        'Metric': position, 
        'uuid': uuid_num, 
        'avg_time': avg_time
    }))
```

### Step 2: Per-Mouse Aggregation
For each mouse, the 99-dimensional vectors for each metric were extracted:

```python
# Extract vectors for each behavioral metric
m_pos_df = reformat_df(males_df, 'dist_to_center_px')
m_height_df = reformat_df(males_df, 'height_ave_mm')
m_length_df = reformat_df(males_df, 'length_mm')
m_vel_df = reformat_df(males_df, 'velocity_2d_mm')
m_moseq_df = reformat_df(males_df, 'MoSeq')

# Aggregate per mouse
for ix, uuid in enumerate(list(set(m_pos_df['uuid']))):
    labels.append(m_pos_df[m_pos_df['uuid'] == uuid].iloc[1]['category'])
    m_pos_list.append(m_pos_df[m_pos_df['uuid'] == uuid]['avg_time'].values)
    m_height_list.append(m_height_df[m_height_df['uuid'] == uuid]['avg_time'].values)
    m_length_list.append(m_length_df[m_length_df['uuid'] == uuid]['avg_time'].values)
    m_vel_list.append(m_vel_df[m_vel_df['uuid'] == uuid]['avg_time'].values)
    m_moseq_list.append(m_moseq_df[m_moseq_df['uuid'] == uuid]['avg_time'].values)
```

### Step 3: Feature Padding and Concatenation
All feature vectors were padded to ensure consistent dimensionality:

```python
# Create feature matrix
fingerprints = OrderedDict()
fingerprints['dist_to_center_px'] = np.vstack(m_pos_list)     # Shape: (n_mice, 99)
fingerprints['height_ave_mm'] = np.vstack(m_height_list)      # Shape: (n_mice, 99)
fingerprints['length_mm'] = np.vstack(m_length_list)         # Shape: (n_mice, 99)
fingerprints['velocity_2d_mm'] = np.vstack(m_vel_list)       # Shape: (n_mice, 99)
fingerprints['MoSeq'] = np.vstack(m_moseq_list)             # Shape: (n_mice, 99)

# Ensure consistent dimensionality
max_len = max([fingerprints[item].shape[1] for item in fingerprints])  # 99

# Pad if necessary
for item in fingerprints:
    if fingerprints[item].shape[1] < max_len:
        padding = np.zeros((fingerprints[item].shape[0], max_len - fingerprints[item].shape[1]))
        fingerprints[item] = np.hstack([fingerprints[item], padding])

# Final concatenation into 495-feature matrix
X = np.hstack([
    fingerprints['dist_to_center_px'],    # Features 0-98
    fingerprints['height_ave_mm'],        # Features 99-197
    fingerprints['length_mm'],            # Features 198-296
    fingerprints['velocity_2d_mm'],       # Features 297-395
    fingerprints['MoSeq']                 # Features 396-494
])
# Final shape: (n_mice, 495)
```

## Feature Matrix Specifications

### Dimensions
- **Input**: Multi-index DataFrame with 5 behavioral metrics
- **Output**: 495-dimensional feature matrix per mouse
- **Structure**: 5 metrics × 99 features = 495 total features

### Feature Organization
The 495 features are organized as follows:

| Feature Range | Behavioral Metric | Description |
|---------------|-------------------|-------------|
| 0-98 | `dist_to_center_px` | Spatial positioning features |
| 99-197 | `height_ave_mm` | Vertical behavior features |
| 198-296 | `length_mm` | Body length features |
| 297-395 | `velocity_2d_mm` | Movement velocity features |
| 396-494 | `MoSeq` | MoSeq behavioral fingerprints |

### Data Quality Assurance
1. **Consistent Dimensionality**: All metrics padded to 99 features
2. **No Missing Values**: Padding with zeros where necessary
3. **Standardization**: StandardScaler applied post-concatenation
4. **Validation**: Cross-checked dimensions across all experimental groups

## Group-Specific Processing

### Males (33 samples)
```python
males_df = summary_df[summary_df.index.get_level_values('group').str.contains('Male')]
# Process through feature matrix construction → 495 features
# Apply PCA → Optimal: 25 components
```

### Females (33 samples)  
```python
females_df = summary_df[summary_df.index.get_level_values('group').str.contains('Female')]
# Process through feature matrix construction → 495 features  
# Apply PCA → Optimal: 12 components
```

### Moms
```python
moms_df = summary_df[summary_df.index.get_level_values('group').str.contains('Mom')]
# Process through feature matrix construction → 495 features
# Apply PCA → Optimal: 8 components
```

## Preprocessing Pipeline

### 1. Label Encoding
```python
# Convert string labels to numeric for classification
from sklearn.preprocessing import LabelEncoder
le = LabelEncoder()
y_num = le.fit_transform(y)  # EE, LNB, NGH, SI → 0, 1, 2, 3
```

### 2. Feature Standardization
```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)  # Zero mean, unit variance
```

### 3. Principal Component Analysis
```python
from sklearn.decomposition import PCA
from sklearn.model_selection import GridSearchCV

# Hyperparameter optimization
param_grid = {
    'pca__n_components': [5, 8, 10, 12, 15, 0.90, 0.95],
    'clf__C': np.logspace(-2, 2, 7)
}
```

## Data Flow Summary

```
MoSeq Pipeline Output
        ↓
FingerprintSummary_full.csv (Multi-index)
        ↓
Group Stratification (Males/Females/Moms)
        ↓
Metric Extraction (5 metrics × 99 features each)
        ↓
Feature Matrix Construction (495 features)
        ↓
Standardization (StandardScaler)
        ↓
PCA + Classification Pipeline
        ↓
Optimal Embeddings (8/12/25 PCs by group)
```

## Usage for Reproducing Feature Matrix

### Loading Source Data
```python
import pandas as pd
import numpy as np
from collections import OrderedDict

# Load fingerprint data
summary_df = pd.read_csv('Fingerprint_Data/FingerprintSummary_full.csv', 
                         index_col=[0, 1], header=[0, 1])
range_dict = pd.read_csv('Fingerprint_Data/FingerprintRangeDict_full.csv')
```

### Extracting Specific Group
```python
# Example: Extract male offspring data
males_df = summary_df[summary_df.index.get_level_values('group').str.contains('Male')]

# Apply reformat_df function to each metric
# (See notebook for complete reformat_df implementation)
```

### Building Feature Matrix
```python
# After extracting all metric lists per mouse
X = np.hstack([
    np.vstack(position_list),    # 99 features
    np.vstack(height_list),      # 99 features  
    np.vstack(length_list),      # 99 features
    np.vstack(velocity_list),    # 99 features
    np.vstack(moseq_list)        # 99 features
])
# Result: (n_mice, 495) feature matrix
```

## Quality Control

### Dimension Verification
```python
# Verify consistent dimensions across metrics
for key, val in fingerprints.items():
    print(f"{key}: {val.shape}")  # Should all be (n_mice, 99)

print(f"Final matrix shape: {X.shape}")  # Should be (n_mice, 495)
```

### Data Integrity
- All feature vectors verified to have 99 dimensions
- Missing values handled through zero-padding
- Consistent ordering maintained across all metrics
- Group stratification preserved throughout processing

## Notes

- The 495-feature matrix serves as input to all PCA analyses
- Identical preprocessing ensures comparability across experimental groups
- Feature organization allows interpretation of principal component loadings
- Standardization applied after concatenation to preserve relative scaling within metrics

## Related Documentation

- `../PC_Embeddings/`: Final embedding results
- `../README.md`: Complete data processing overview
- See individual group READMEs for specific applications of this feature matrix
