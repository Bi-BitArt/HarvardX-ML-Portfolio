### Experiment 2-1: Simple Logistic Models (Income-only, Income+Savings)

#### Results

- **Income-only Model**  
  - Accuracy: **0.600**  
  - ROC AUC: **0.645**

- **Income + Savings Model**  
  - Accuracy: **0.613**  
  - ROC AUC: **0.661**

- **Feature-selected XGBoost (Exp1-2)**  
  - Accuracy: **0.67** (vs. 0.68 baseline with all features)  
  - ROC AUC: **0.75** (vs. 0.77 baseline with all features)  

#### Comparison

| Model                        | Accuracy | ROC AUC |
|------------------------------|----------|---------|
| Income-only Logistic         | 0.60     | 0.65    |
| Income + Savings Logistic    | 0.61     | 0.66    |
| XGBoost (11 features, Exp1-2)| 0.67     | 0.75    |

#### Interpretation
- These models are **Logistic Regression classifiers**, using a linear combination of features with a sigmoid link function.  
- Adding savings to income yields only a small improvement (AUC 0.65 → 0.66).  
- The feature-selected XGBoost substantially outperforms both logistic models, even with fewer variables.  
- This confirms that **household vulnerability cannot be explained by simple financial metrics alone**. Multi-dimensional features are essential for predictive accuracy.
