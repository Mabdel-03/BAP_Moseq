# MoSeq Behavioral Data

This directory contains principal component embeddings derived from Motion Sequencing (MoSeq) behavioral analysis of mice under different environmental conditions.

## Data Processing Pipeline

### Source Data
The embeddings were created from **population-specific MoSeq pipeline outputs**. Separate MoSeq models were trained for Moms and Offsprings to optimize feature extraction for each population's behavioral repertoire. The raw fingerprint data and detailed processing documentation can be found in the `Fingerprint_Data/` directory with population-specific subdirectories.

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

## Directory Structure

### Fingerprint_Data
- **Purpose**: Population-specific MoSeq fingerprint data and 495-feature matrix documentation
- **Structure**: Separate subdirectories for Moms and Offsprings models
- **Key Subdirectories**:
  - `Moms/`: Fingerprint data from MoSeq model trained on maternal mice only
  - `Offsprings/`: Fingerprint data from MoSeq model trained on offspring mice only
- **Benefits**: Population-specific optimization reduces noise and improves feature relevance

## Experimental Groups

### Moms
- Maternal mice exposed to different environmental conditions
- Embedding: 8 principal components
- File: `PC_Embeddings/Moms/moms_PCA8_embedding.csv.gz`

### Offsprings
Offspring mice stratified by sex:

#### Males
- Male offspring mice
- Sample size: 33 animals
- Embedding: 25 principal components (optimal)
- File: `PC_Embeddings/Offsprings/Male/males_final_data.csv`
- Classification accuracy: 75.8% balanced accuracy

#### Females  
- Female offspring mice
- Sample size: 33 animals
- Embedding: 12 principal components (optimal)
- File: `PC_Embeddings/Offsprings/Female/females_final_data.csv`

## Environmental Conditions

- **EE**: Enriched Environment
- **LNB**: [Condition description needed]
- **NGH**: [Condition description needed]
- **SI**: Social Isolation

## Data Format

Each embedding file contains:
- `uuid`: Unique mouse identifier (UUID format)
- `moseq_id`: BAP group identifier (session-based format)
- `category`: Environmental condition (EE, LNB, NGH, SI)
- `PC1, PC2, ..., PCn`: Principal component coordinates

### Dual Identifier System
- **uuid**: Universal unique identifier for cross-study compatibility
- **moseq_id**: BAP group-specific identifier following session naming convention

## Technical Details

- **Preprocessing**: StandardScaler normalization
- **PCA Parameters**: Optimized via GridSearchCV (components: 5, 8, 10, 12, 15, or 90%/95% variance)
- **Classification**: Multinomial logistic regression with L2 regularization
- **Validation**: 5-fold stratified cross-validation
- **Optimization Metric**: Balanced accuracy

## File Formats

- **CSV**: Uncompressed CSV files for easy sharing and analysis
- **PKL**: Pickle files for faster loading in Python (when available)
- **Joblib**: Trained model files for reproducibility

## Important Notes

- **Optimal PC counts vary by group**: Males use 25 PCs, Females use 12 PCs for best classification
- **Dual identifiers**: Each mouse has both `uuid` (universal) and `moseq_id` (BAP group format)
- **Sample sizes**: 33 animals per offspring group