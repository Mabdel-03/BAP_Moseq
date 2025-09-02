# MoSeq Behavioral Data

This directory contains principal component embeddings derived from Motion Sequencing (MoSeq) behavioral analysis of mice under different environmental conditions.

## Data Processing Pipeline

### Source Data
The embeddings were created from MoSeq pipeline output, specifically from fingerprint summary data containing multiple behavioral metrics per mouse.

### Feature Aggregation
Five key behavioral metrics were extracted and combined:

1. **dist_to_center_px**: Spatial positioning relative to arena center
2. **height_ave_mm**: Average height measurements during behavior
3. **length_mm**: Body length measurements
4. **velocity_2d_mm**: Two-dimensional velocity metrics
5. **MoSeq**: MoSeq-specific behavioral fingerprints

### Processing Steps
1. **Data Reformatting**: Multi-index summary data was reformatted into single-index DataFrames
2. **Feature Padding**: All feature vectors were padded to ensure consistent dimensionality (99 features each)
3. **Feature Concatenation**: Combined all metrics into a unified feature matrix (495 total features)
4. **Standardization**: Applied StandardScaler normalization
5. **Dimensionality Reduction**: Used PCA with hyperparameter optimization via GridSearchCV

## Experimental Groups

### Moms
- Maternal mice exposed to different environmental conditions
- Embedding: 8 principal components
- File: `PC_Embeddings/Moms/moms_PCA8_embedding.csv.gz`

### Offsprings
Offspring mice stratified by sex:

#### Males
- Male offspring mice
- Embedding: 25 principal components  
- File: `PC_Embeddings/Offsprings/Male/offspring_PCA25_embedding.csv.gz`
- Classification accuracy: ~77.5% balanced accuracy

#### Females  
- Female offspring mice
- Embedding: 25 principal components
- File: `PC_Embeddings/Offsprings/Female/offspring_PCA25_embedding.csv.gz`

## Environmental Conditions

- **EE**: Enriched Environment
- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]
- **SI**: Social Isolation

## Data Format

Each embedding file contains:
- `uuid`: Unique mouse identifier
- `category`: Environmental condition (EE, LNB, NGH, SI)
- `PC1, PC2, ..., PCn`: Principal component coordinates

## Technical Details

- **Preprocessing**: StandardScaler normalization
- **PCA Parameters**: Optimized via GridSearchCV (components: 5, 8, 10, 12, 15, or 90%/95% variance)
- **Classification**: Multinomial logistic regression with L2 regularization
- **Validation**: 5-fold stratified cross-validation
- **Optimization Metric**: Balanced accuracy

## File Formats

- **CSV.gz**: Compressed CSV files for easy sharing and analysis
- **PKL**: Pickle files for faster loading in Python
- **Joblib**: Trained model files for reproducibility