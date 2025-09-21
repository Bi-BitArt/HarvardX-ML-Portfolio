### Experiment 5-1: Rule-based Hyperparameter Tuning (Multiclass, 5-class noisy dataset)

#### Setup
- **Dataset**: same as exp4-x
- **Model**: XGBoost 
- **Fixed settings**:  
  - subsample = 0.8  
  - colsample_bytree = 0.8  
  - random_state = 42  
  - eval_metric = "mlogloss"  
- **Rule-based candidates**: combinations of `max_depth (md)`, `n_estimators (ne)`, and `learning_rate (lr)` inspired by Exp3-1 (binary tuning).  

---

#### Results (sorted by ROC AUC)

| md | ne   | lr   | Accuracy | ROC AUC |
|----|------|------|----------|---------|
| 2  | 300  | 0.10 | 0.707    | **0.928** |
| 1  | 500  | 0.10 | 0.687    | **0.928** |
| 1  | 300  | 0.15 | 0.675    | 0.925 |
| 2  | 200  | 0.10 | 0.670    | 0.920 |
| 1  | 300  | 0.12 | 0.647    | 0.919 |
| 1  | 300  | 0.10 | 0.632    | 0.913 |
| 1  | 200  | 0.15 | 0.630    | 0.912 |
| 1  | 200  | 0.12 | 0.605    | 0.905 |
| 1  | 200  | 0.10 | 0.590    | 0.898 |

#### Reference (from exp3-1, 3-2)

| Setting        | AUC    | MD | NE  | LR   |
|----------------|--------|----|-----|------|
| Manual best    | 0.7781 | 1  | 200 | 0.12 |
| Automated best (Optuna) | 0.7788 | 1  | 121 | 0.19 |

#### Insight

From both the binary (Exp3) and multiclass (Exp5) experiments, a consistent trend emerged:

- **Depth (MD):** Shallow trees (1–2) produced the best generalization.  
- **Learning Rate (LR):** Stable around 0.1–0.2.  
- **Estimators (NE):** Strongly linked to the number of classes.  

| Task | Best NE range | Rule-of-thumb |
|------|---------------|----------------|
| Binary (2-class)   | 121–200 | ~ Class × 60–100 |
| Multiclass (5-class) | 300–500 | ~ Class × 60–100 |

**Rule:** Optimal `n_estimators` ≈ *Class × (60–100)*  

This suggests that while MD and LR remain relatively stable across tasks,  
**NE scales with the number of classes**, providing a practical guideline for tuning boosting models. 
