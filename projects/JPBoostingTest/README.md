# 📊 JPBoostingTest – Summary

## 1. Motivation  
- Inspired by *Salvador (2024): "Use of Boosting Algorithms in Household-Level Poverty Measurement"*.  
- Generated a synthetic Japanese household dataset to test whether boosting models can predict **latent household vulnerability**.  
- Goal: build a reproducible pipeline for analyzing vulnerability beyond simple income-based measures.  

## 2. Experiments Overview  
- [**Exp1:** Baseline XGBoost + feature selection, SHAP](projects/JPBoostingTest/exp1-1_basicboosting)  
- [**Exp2:** Income-only vs. income+saving models](projects/JPBoostingTest/exp2-1_income%20%26%20income%2Bsave)  
- [**Exp3:** Hyperparameter tuning – manual vs. Optuna](projects/JPBoostingTest/exp3-1_parameter%20tuning%20(over%20%26%20underfitting%20control))  
- [**Exp4:** Multiclass boosting and model comparison (CatBoost, LightGBM, AdaBoost)](projects/JPBoostingTest/exp4-1_multiclass%20xgboost%20(5%20classes))  
- [**Exp5:** Multiclass parameter tuning](projects/JPBoostingTest/exp5-1_parameter%20tuning%20%20with%20multiclass%20boosting)  

## 3. Cross-Experiment Insights  
- **Tree depth:** Best kept shallow (MD=1–2).  
- **Learning rate:** Stable at 0.1–0.2.  
- **Number of estimators:** Best ≈ *60–100 × number of classes*.  
- XGBoost baseline is strong, but **CatBoost shows higher robustness with noisy categorical data**.  
- AdaBoost is clearly outdated for this task.  

## 4. Takeaways  
- Rule-of-thumb hyperparameters were consistently observed across binary and multiclass tasks.  
- The workflow (baseline → feature selection → parameter tuning → multiclass expansion → model comparison) provides a **replicable pipeline** for future policy simulation.  
- This project shows how **machine learning can uncover latent household vulnerability**, aligning with poverty measurement research.
