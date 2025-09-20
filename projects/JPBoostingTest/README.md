# JPBoostingTest – Summary

## 1. Motivation  
- Inspired by *Salvador (2021): "Use of Boosting Algorithms in Household-Level Poverty Measurement"*.  
- Generated a synthetic Japanese household dataset to test whether boosting models can predict **latent household vulnerability**.  
- Goal: build a reproducible pipeline for analyzing vulnerability beyond simple income-based measures.  

## 2. Experiments Overview  
- [**Exp1-1:** Baseline XGBoost](exp1-1_basicboosting)  
- [**Exp1-2:** Feature selection](exp1-2_featureselection)  
- [**Exp1-3:** SHAP analysis](exp1-3_shap%20analysis)  
- [**Exp2-1:** Income-only vs. income+saving](exp2-1_income%20%26%20income%2Bsave)  
- [**Exp3-1:** Parameter tuning (over & underfitting control)](exp3-1_parameter%20tuning%20(over%20%26%20underfitting%20control))  
- [**Exp3-2:** Automated hyperparameter search (Optuna)](exp3-2_automated%20hyperparameter%20search%20(optuna))  
- [**Exp4-1:** Multiclass XGBoost (5 classes)](exp4-1_multiclass%20xgboost%20(5%20classes))  
- [**Exp4-2:** Model comparison with alternative boosting models](exp4-2_model%20comparison%20with%20alternative%20boosting%20models)  
- [**Exp5-1:** Parameter tuning with multiclass boosting](exp5-1_parameter%20tuning%20%20with%20multiclass%20boosting)  

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
