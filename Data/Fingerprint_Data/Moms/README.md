# Moms Fingerprint Data

This directory contains the raw fingerprint data from the MoSeq behavioral analysis pipeline trained specifically on maternal mice.

## Overview

The MoSeq (Motion Sequencing) model for maternal mice was trained exclusively on behavioral data from mothers to capture population-specific movement patterns and behavioral repertoires. This approach ensures that the extracted features are optimized for maternal behavioral analysis.

## Files

### `FingerprintSummary_full.csv.zip`
Compressed CSV file containing the multi-index behavioral fingerprint data for maternal mice.

**Data Structure**:
```
Index Levels:
- Level 0: Group identifier (Mom_[Condition])
- Level 1: Mouse UUID

Column Levels:  
- Level 0: Behavioral metric type
- Level 1: Feature index (0-98 for each metric)

Behavioral Metrics (5 types × 99 features = 495 total):
- dist_to_center_px: Spatial positioning features
- height_ave_mm: Vertical behavior features  
- length_mm: Body length features
- velocity_2d_mm: Movement velocity features
- MoSeq: MoSeq-specific behavioral fingerprints
```

## Population-Specific Model Benefits

### Maternal Behavioral Focus
- **Optimized Feature Extraction**: Model trained specifically on maternal behavioral patterns
- **Reduced Cross-Population Noise**: Eliminates offspring-specific movement artifacts
- **Enhanced Sensitivity**: Better detection of condition effects in maternal behavior
- **Biological Relevance**: Features capture maternally-relevant behavioral repertoires

### Environmental Conditions
Maternal mice were exposed to:
- **EE**: Enriched Environment
- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]
- **SI**: Social Isolation

## Processing Pipeline

### From Fingerprint to 8-PC Embeddings

1. **Source**: Population-specific MoSeq model output
2. **Feature Extraction**: 5 behavioral metrics × 99 features = 495 total
3. **Standardization**: StandardScaler normalization
4. **PCA Optimization**: GridSearchCV → 8 optimal components
5. **Final Output**: 8-dimensional embeddings (`../PC_Embeddings/Moms/`)

### Feature Matrix Construction
```python
# Load mom-specific data
summary_df = pd.read_csv('FingerprintSummary_full.csv.zip', 
                         index_col=[0, 1], header=[0, 1])

# Extract maternal data only
moms_df = summary_df[summary_df.index.get_level_values('group').str.contains('Mom')]

# Process through 495-feature matrix construction
# (See ../README.md for complete methodology)
```

## Data Quality

### Population Specificity
- **Training Data**: Maternal mice only
- **Feature Relevance**: Optimized for maternal behavioral patterns
- **Model Performance**: Enhanced by population-specific training

### Technical Specifications
- **Format**: Multi-index DataFrame (compressed CSV)
- **Dimensions**: Variable number of mothers × 5 metrics × 99 features
- **Processing**: Identical methodology to offspring data for comparability
- **Output**: 8-dimensional optimal embeddings

## Usage

### Loading Mom-Specific Data
```python
import pandas as pd

# Load compressed fingerprint data
df_moms = pd.read_csv('FingerprintSummary_full.csv.zip', 
                      index_col=[0, 1], header=[0, 1])

# Verify mom-specific groups
groups = df_moms.index.get_level_values('group').unique()
print("Available groups:", groups)
# Expected: Mom_EE, Mom_LNB, Mom_NGH, Mom_SI
```

### Feature Matrix Construction
See the main `Fingerprint_Data/README.md` for complete code examples of:
- Multi-index data reformatting
- Per-mouse feature aggregation  
- 495-feature matrix construction
- Standardization and PCA application

## Relationship to PC Embeddings

This fingerprint data is processed through the feature engineering pipeline to produce:
- **Final Embeddings**: `../PC_Embeddings/Moms/moms_PCA8_embedding.csv.gz`
- **Optimal Dimensions**: 8 principal components
- **Performance**: [To be specified when available]

## Related Documentation

- `../Offsprings/README.md`: Offspring-specific MoSeq model data
- `../README.md`: Complete fingerprint processing methodology
- `../../PC_Embeddings/Moms/README.md`: Final embedding documentation
