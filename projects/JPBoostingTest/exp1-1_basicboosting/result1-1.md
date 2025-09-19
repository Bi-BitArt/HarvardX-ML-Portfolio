### Experiment 1: Baseline XGBoost Model

#### Setup
- **Dataset**: `household_vulnerability_japan.csv`  
  - 3,000 rows, 23 columns (22 features + 1 target: `vulnerable`)  
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
- **Accuracy**: 0.68  
- **ROC AUC**: 0.77  
- **Classification report**: shows reasonably balanced precision/recall across vulnerable and non-vulnerable households (full output omitted here).  

#### Interpretation
- The baseline XGBoost achieves **solid discriminatory power** (AUC > 0.75), indicating that socio-economic vulnerability patterns are detectable.  
- Accuracy (≈0.68) is modest but reasonable given the class imbalance typical in household-level data.  
- This experiment provides a **foundation** for further work on:
  - Feature selection (Exp1-2)  
  - Model interpretability (Exp1-3, SHAP analysis)
  - Parameter tuning for over/underfitting control (Exp3-1)  
