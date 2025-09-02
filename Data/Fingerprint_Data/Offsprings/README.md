# Offsprings Fingerprint Data

This directory contains the raw fingerprint data from the MoSeq behavioral analysis pipeline trained specifically on offspring mice.

## Overview

The MoSeq (Motion Sequencing) model for offspring mice was trained exclusively on behavioral data from offspring (both male and female) to capture population-specific movement patterns and behavioral repertoires. This approach ensures that the extracted features are optimized for offspring behavioral analysis.

## Files

### `FingerprintSummary_full.csv.zip`
Compressed CSV file containing the multi-index behavioral fingerprint data for offspring mice.

**Data Structure**:
```
Index Levels:
- Level 0: Group identifier (Male_[Condition], Female_[Condition])
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

### Offspring Behavioral Focus
- **Optimized Feature Extraction**: Model trained specifically on offspring behavioral patterns
- **Reduced Cross-Population Noise**: Eliminates maternal-specific movement artifacts
- **Enhanced Sensitivity**: Better detection of condition effects in offspring behavior
- **Developmental Relevance**: Features capture developmentally-appropriate behavioral repertoires

### Sex Stratification Within Offspring
While the MoSeq model was trained on all offspring data, subsequent analysis stratifies by sex:
- **Males**: 33 samples → 25 optimal PCs
- **Females**: 33 samples → 12 optimal PCs

### Environmental Conditions
Offspring mice were exposed to:
- **EE**: Enriched Environment
- **LNB**: Limited Nesting and Bedding
- **NGH**: Normal Growth Housing
- **SI**: Social Isolation

## Processing Pipeline

### From Fingerprint to Sex-Specific Embeddings

1. **Source**: Population-specific MoSeq model output (offspring-only training)
2. **Sex Stratification**: Separate processing for males and females
3. **Feature Extraction**: 5 behavioral metrics × 99 features = 495 total (per sex)
4. **Standardization**: StandardScaler normalization (per sex)
5. **PCA Optimization**: GridSearchCV → Sex-specific optimal components
6. **Final Output**: 
   - Males: 25-dimensional embeddings
   - Females: 12-dimensional embeddings

### Feature Matrix Construction Per Sex

#### Males
```python
# Load offspring-specific data
summary_df = pd.read_csv('FingerprintSummary_full.csv.zip', 
                         index_col=[0, 1], header=[0, 1])

# Extract male offspring data only
males_df = summary_df[summary_df.index.get_level_values('group').str.contains('Male')]

# Process through 495-feature matrix construction
# Apply sex-specific PCA optimization → 25 PCs
```

#### Females  
```python
# Extract female offspring data only
females_df = summary_df[summary_df.index.get_level_values('group').str.contains('Female')]

# Process through 495-feature matrix construction  
# Apply sex-specific PCA optimization → 12 PCs
```

## Data Quality

### Population Specificity
- **Training Data**: Offspring mice only (both sexes)
- **Feature Relevance**: Optimized for offspring behavioral patterns
- **Model Performance**: Enhanced by population-specific training

### Technical Specifications
- **Format**: Multi-index DataFrame (compressed CSV)
- **Dimensions**: 66 total offspring (33 males + 33 females) × 5 metrics × 99 features
- **Processing**: Identical methodology to maternal data for comparability
- **Output**: Sex-specific optimal embeddings (25 PCs males, 12 PCs females)

## Sex-Specific Results

### Male Offspring (25 PCs)
- **Sample Size**: 33 animals
- **Classification Accuracy**: 75.8% balanced accuracy
- **Confusion Matrix Performance**:
  - EE: 83% recall, 100% precision
  - LNB: 71% recall, 83% precision  
  - NGH: 85% recall, 69% precision
  - SI: 57% recall, 67% precision

### Female Offspring (12 PCs)
- **Sample Size**: 33 animals
- **Classification Performance**: [To be specified]
- **Confusion Matrix Performance**:
  - EE: 67% recall
  - LNB: 29% recall (71% confused with NGH)
  - NGH: 85% recall  
  - SI: 86% recall (excellent separation)

## Usage

### Loading Offspring-Specific Data
```python
import pandas as pd
import numpy as np

# Load offspring-specific fingerprint data
df_offspring = pd.read_csv('FingerprintSummary_full.csv.zip', 
                           index_col=[0, 1], header=[0, 1])

# Verify offspring-specific groups
groups = df_offspring.index.get_level_values('group').unique()
print("Available groups:", groups)
# Expected: Male_EE, Male_LNB, Male_NGH, Male_SI, Female_EE, Female_LNB, Female_NGH, Female_SI

# Stratify by sex
males_df = df_offspring[df_offspring.index.get_level_values('group').str.contains('Male')]
females_df = df_offspring[df_offspring.index.get_level_values('group').str.contains('Female')]

print(f"Males: {len(males_df)} entries")
print(f"Females: {len(females_df)} entries")
```

### Feature Matrix Construction
See the main `Fingerprint_Data/README.md` for complete code examples of:
- Multi-index data reformatting (`reformat_df` function)
- Per-mouse feature aggregation
- 495-feature matrix construction
- Sex-specific standardization and PCA application

## Comparison with Moms Data

### Key Differences
- **Training Population**: Offspring vs. Maternal mice
- **Behavioral Repertoire**: Developmentally-appropriate vs. maternal patterns
- **Model Optimization**: Population-specific feature extraction
- **Final Dimensions**: Sex-stratified (12/25 PCs) vs. unified (8 PCs)

### Methodological Consistency
- **Same Pipeline**: Identical feature engineering and PCA methodology
- **Same Metrics**: 5 behavioral measures across both populations
- **Same Validation**: 5-fold stratified cross-validation
- **Comparable Results**: Direct comparison possible despite population-specific training

## Relationship to PC Embeddings

This fingerprint data is processed through the feature engineering pipeline to produce:
- **Male Embeddings**: `../../PC_Embeddings/Offsprings/Male/males_final_data.csv`
- **Female Embeddings**: `../../PC_Embeddings/Offsprings/Female/females_final_data.csv`

## Related Documentation

- `../Moms/README.md`: Maternal-specific MoSeq model data
- `../README.md`: Complete fingerprint processing methodology
- `../../PC_Embeddings/Offsprings/`: Final sex-specific embedding documentation
