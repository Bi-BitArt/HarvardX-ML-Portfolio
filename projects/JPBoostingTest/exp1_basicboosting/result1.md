### Experiment 1: Baseline Model

#### Setup

Data: household_vulnerability_japan.csv (3,000 rows, 23 features)

Split: 80% train / 20% test (stratified)

Model: XGBoost (binary classification)

n_estimators=100

learning_rate=0.1

max_depth=3

subsample=0.8

colsample_bytree=0.8

scale_pos_weight = (negatives / positives)

eval_metric="logloss"



#### Results

Accuracy: 0.68

ROC AUC: 0.77

Classification report shows balanced performance between vulnerable and non-vulnerable groups (details omitted here).


#### Interpretation

Baseline XGBoost already achieves solid predictive power (AUC > 0.75).

Accuracy is modest (≈0.68) but acceptable for an imbalanced, socio-economic style dataset.

This provides a solid foundation for feature selection, parameter tuning, and explainability in later experiments.
