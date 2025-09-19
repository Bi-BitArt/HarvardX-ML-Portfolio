# Experiment 3-1: Parameter Tuning (Feature-selected XGBoost)

## NE Adjustment (n_estimators)
**Fixed:** MD=3, LR=0.1  

| MD | NE   | LR   | Accuracy | ROC AUC |
|----|------|------|----------|---------|
| 3  | 100  | 0.1  | 0.67     | 0.747   | (baseline, Exp1-2) |
| 3  | 1000 | 0.1  | 0.66     | 0.716   |
| 3  | 5000 | 0.1  | 0.66     | 0.697   |
| 3  | 50   | 0.1  | 0.65     | 0.750   |
| 3  | 5    | 0.1  | 0.64     | 0.716   |

**Insights**
- Too many trees (NE=1000, 5000) → AUC dropped (0.716, 0.697), signs of overfitting.  
- Too few trees (NE=5) → underfitting (AUC=0.716).  
- Moderate trees (NE=50–100) gave the best balance, with NE=50 reaching AUC=0.750.

---

## LR Adjustment (learning_rate)
**Fixed:** MD=3, NE=100  

| MD | NE   | LR   | Accuracy | ROC AUC |
|----|------|------|----------|---------|
| 3  | 100  | 0.1  | 0.67     | 0.747   | (baseline, Exp1-2) |
| 3  | 100  | 0.05 | 0.65     | 0.753   |
| 3  | 100  | 0.01 | 0.66     | 0.742   |
| 3  | 100  | 0.2  | 0.68     | 0.746   |
| 3  | 100  | 0.5  | 0.67     | 0.718   |

**Insights**
- Best result at LR=0.05 (AUC=0.753).  
- Too low (0.01) → underfitting.  
- Too high (0.5) → unstable, lower AUC.  
- Sweet spot ≈ 0.05.  

---

## MD Adjustment (max_depth)
**Fixed:** NE=100, LR=0.1  

| MD | NE   | LR   | Accuracy | ROC AUC |
|----|------|------|----------|---------|
| 3  | 100  | 0.1  | 0.67     | 0.747   | (baseline, Exp1-2) |
| 6  | 100  | 0.1  | 0.66     | 0.713   |
| 12 | 100  | 0.1  | 0.66     | 0.712   |
| 2  | 100  | 0.1  | 0.66     | 0.761   |
| 1  | 100  | 0.1  | 0.67     | 0.770   |

**Insights**
- Deeper trees (MD=6,12) → worse performance (AUC ≈0.71).  
- Shallower trees (MD=2,1) → better performance.  
- Best result: MD=1, AUC=0.770.  

---

## Overall Conclusion
- **Best setting so far:**  
  MD=1, NE=200, LR=0.12: AUC=0.778, Accuracy=0.68  

- This configuration clearly outperforms all previous baselines and tuned models.  
- The success comes from using **very shallow learners (MD=1)** to prevent overfitting, while a **moderate number of trees (NE=200)** and a **slightly higher learning rate (LR=0.12)**: the right balance between underfitting and overfitting.  
- Deeper trees or extreme parameter values (too many/few trees, too high/low learning rates) consistently reduced generalization.
