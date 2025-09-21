### Experiment 4-1: Baseline Multiclass XGBoost

#### Dataset
- **Name**: `household_vulnerability_japan_multiclass_noise.csv`
- **Size**: 3,000 rows, 23 columns (22 features + 1 target)
- **Target**: `vulnerability_class` (5 classes: 0 = Very Low, 1 = Low, 2 = Medium, 3 = High, 4 = Very High)
- **Generation process**:
  - Divided into **5 vulnerability levels** using fixed thresholds.
  - Added **two types of noise**:  
    1. **Random perturbations** in continuous variables (±10%).  
    2. **Random missingness** (~10%) in non-target features.  
  - Ensured **target column contains no missing values**.

- **Split**: 80% train / 20% test (stratified by target)
  
- **Model**: XGBoost (binary classification)  
  - n_estimators = 100  
  - learning_rate = 0.1  
  - max_depth = 3  
  - subsample = 0.8  
  - colsample_bytree = 0.8  
  - scale_pos_weight = (negatives / positives)  
  - eval_metric = "logloss"  

#### Results
**Overall metrics**:  
- Accuracy: **0.66**  
- Macro avg F1: **0.57**  
- ROC AUC (macro OVR): **0.91**  

**Per-class performance**:  
- Class 0 (Very Low): Precision 0.82 / Recall 0.69 / F1 = 0.75  
- Class 1 (Low): Precision 0.57 / Recall 0.27 / F1 = 0.37  
- Class 2 (Medium): Precision 0.65 / Recall 0.96 / F1 = 0.78  
- Class 3 (High): Precision 0.47 / Recall 0.23 / F1 = 0.31  
- Class 4 (Very High): Precision 0.86 / Recall 0.55 / F1 = 0.67  

#### Interpretation
- **Strong ranking ability**: AUC = 0.91 indicates that the model effectively separates households across vulnerability levels.  
- **Low classification accuracy**: 0.66 overall, reflecting the difficulty of exact 5-class labeling under noisy conditions.  
- **Class imbalance effects**:  
  - Medium vulnerability (Class 2) is predicted very well (Recall 0.96).  
  - Low (Class 1) and High (Class 3) vulnerability are much harder to classify (F1 < 0.40).  
- This mirrors real-world patterns: **middle groups are easier to identify**, while distinguishing borderline classes is more difficult.  
