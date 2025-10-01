# JPBoostingTest – Summary
*Testing household vulnerability and policy effects under economic shocks with machine learning.*

## 1. Motivation  
- Inspired by *Salvador (2024): "Use of Boosting Algorithms in Household-Level Poverty Measurement:  
  A Machine Learning Approach to Predict and Classify Household Wealth Quintiles in the Philippines"*.
- Generated a synthetic Japanese household dataset to test whether boosting models can predict **latent household vulnerability**.  
- Goal: build a reproducible pipeline for analyzing vulnerability beyond simple income-based measures.  

## 2. Experiments Overview  
- [**Exp1:** Baseline XGBoost](EXP1_BasicBoosting)  
- [**Exp2:** Logistic regression benchmark](EXP2_LogisticRegression)  
- [**Exp3:** Parameter tuning (over & underfitting control)](EXP3_Parameters)  
- [**Exp4:** Multiclass XGBoost (5 classes)](EXP4_Multiclass)  
- [**Exp5:** Parameter tuning with multiclass boosting](EXP5_Parameters2)  
- [**Exp6:** Scenario simulations (shock & policy)](EXP6_Scenarios)  

## 3. Insights 

### 3.1 Features Insights

- **Core predictors (kept after feature selection):** income, employment status, health (severe illness), repayment history (late payments), savings, and household/demographic context (foreign-born, multigen households, childcare/eldercare costs).  
- **Dropped factors:** housing conditions, financial literacy, digital access — less direct signals of vulnerability in this dataset.  
- **SHAP analysis:**  
  - Vulnerability rises with **illness, foreign-born status, high care costs, and late payments**.  
  - Vulnerability decreases with **higher income, savings, and stable employment**.  
  - Some effects (e.g., multigen households, region, sector) are mixed and context-dependent.  

Overall, vulnerability is **multi-dimensional**, driven by both financial capacity and non-financial social/health factors.

### 3.2 Cross-Experiment Insights  
- **Baseline comparison:** Simple income-based models (income-only or income+savings) reached AUC ≈ 0.64–0.66, while boosting with richer features achieved ≈ 0.77+.  
  → This highlights the clear added value of multi-dimensional ML approaches over conventional indicators.  
- **Tree depth:** Best kept shallow (MD=1–2).  
- **Learning rate:** Stable at 0.1–0.2.  
- **Number of estimators:** Best ≈ *60–100 × number of classes*.  
- XGBoost baseline is strong, but **CatBoost shows higher robustness with noisy categorical data**.  
- AdaBoost is clearly outdated for this task.  

### 3.3 Takeaways 
- Rule-of-thumb hyperparameters were consistently observed across binary and multiclass tasks.  
- The workflow (baseline → feature selection → parameter tuning → multiclass expansion → model comparison) provides a **replicable pipeline** for future policy simulation.  
- This project shows how **machine learning can uncover latent household vulnerability**, aligning with poverty measurement research.  

---

# EXP6: Scenario Experiments – Stress Tests & Policy Simulations 

⚡ **This is the CORE HIGHLIGHT — testing how vulnerability changes under shocks and policies.**

*Exp6 explores three stages: (1) shocks, (2) policy responses, and (3) latent vulnerability interventions, yielding the project’s most important insights*

---

## 🔹 Exp6-1: External Shock Simulation  
- **Design:** Applied an external shock to income, employment, and sector-specific variables, based on JILPT COVID-19 survey evidence.  
- **Findings:**  
  - Mean household income fell ~20%.  
  - Employment precarization +16%.  
  - ROC AUC dropped from **0.911 → 0.872 (Δ −0.039)**.  
  - Class transition matrices showed strong upward mobility into more vulnerable classes.  

---

## 🔹 Exp6-2: Policy Intervention Scenarios  
- **Tested policies:**  
  1. **Uniform ¥100,000 cash transfer** (Japan’s 2020 policy).  
  2. **Targeted cash transfer** (bottom 30% by income).  
  3. **Budget-neutral targeted transfer** (same budget, concentrated in bottom 30%).  
- **Findings:**  
  - **Universal cash:** maximized equity but destabilized classes, erased Class 3.  
  - **Targeted cash:** moderate efficiency gains, left higher classes unprotected.  
  - **Budget-neutral targeted:** most efficient, but paradoxical:  
    - **36% of Class 3 improved → Class 2, yet 35% fell → Class 4.**  
    - No Class 4 households improved → one-off transfers insufficient.  

---

## 🔹 Exp6-3: Latent Vulnerability Policy (LVP)  
- **Design:** Clustered households by income & savings → identified vulnerable cluster.  
  - Feature analysis showed **latent vulnerabilities**: foreign-born, low financial literacy, high debt-to-income, precarious housing.  
  - Policy combined **cash + structural tweaks** (debt relief, literacy, digital access).  
- **Results:**  
  - Strong stabilization in vulnerable clusters.  
  - ROC AUC dropped sharply (**0.911 → 0.629, Δ −0.282**).  
  - Interpretation: not “worse,” but **boundaries blurred as inequality reduced**.  
- **Insight:**  
  - LVP highlights the **cost of fairness**: more equitable outcomes reduce predictive separability.  
  - Shows that **structural policies + financial transfers** are necessary for long-term protection.  

---

### Transition Matrix under LVP  

![LVPheatmaps](https://raw.githubusercontent.com/Bi-BitArt/HarvardX-ML-Portfolio/main/projects/JPBoostingTest/EXP6_Scenarios/exp6-3_latent%20vulnerability%20policy/LVPheatmaps.png)

*Heatmap of class transitions (0 = least vulnerable, 4 = most vulnerable).  
Rows: predicted class after **shock**, Columns: predicted class after **policy**.  

*Example: Budget-neutral: Class 2 after the shock (row = 2).*
- 14.8% improved to Class 0.
- 6.3% improved to Class 1.
- 78.9% stayed in Class 2.
- 0% fell into worse classes.

---

## Cross-Experiment Insights (Exp6)  
- **Shock sensitivity:** Vulnerability models degrade under stress; calibration is fragile.  
- **Policy paradox:** Targeting improves efficiency, but risks eroding stability in mid-tier classes.  
- **Latent vulnerability:** Going beyond income reveals hidden risk factors, but blurs class distinctions.  
- **Future outlook:** With richer data and advanced models (deep learning, causal inference), ROC AUC can be recovered while maintaining fairness.
