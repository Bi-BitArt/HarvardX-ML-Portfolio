# JPBoostingTest – Summary
*This project demonstrates how machine learning can move beyond descriptive poverty measurement to simulate and evaluate household vulnerability and policy effectiveness under economic shocks.*

## 1. Motivation  
- Inspired by *Salvador (2024): "Use of Boosting Algorithms in Household-Level Poverty Measurement:  
  A Machine Learning Approach to Predict and Classify Household Wealth Quintiles in the Philippines"*.
- Generated a synthetic Japanese household dataset to test whether boosting models can predict **latent household vulnerability**.  
- The goal is to build a reproducible pipeline for analyzing vulnerability beyond simple income-based measures.  

## 2. Experiments Overview  
- [**Exp1:** Baseline XGBoost](EXP1_BasicBoosting)  
- [**Exp2:** Logistic regression benchmark](EXP2_LogisticRegression)  
- [**Exp3:** Parameter tuning (over & underfitting control)](EXP3_Parameters)  
- [**Exp4:** Multiclass XGBoost (5 classes)](EXP4_Multiclass)  
- [**Exp5:** Parameter tuning with multiclass boosting](EXP5_Parameters2)  
- [**Exp6:** Scenario simulations (shock & policy)](EXP6_Scenarios)  

## 3. Insights 

### 3.1 Features Insights

- **Core predictors (kept after feature selection):** income, employment status, health, repayment history, savings, and household/demographic context (foreign-born, multigen households, child/eldercare costs).  
- **Dropped factors:** housing conditions, financial literacy, digital access — less direct signals of vulnerability in this dataset.  
- **SHAP analysis:**  
  - Vulnerability rises with **illness, foreign-born status, high care costs, and late payments**.  
  - Vulnerability decreases with **higher income, savings, and stable employment**.  

Overall, vulnerability is **multi-dimensional**, driven by both financial capacity and non-financial social/health factors.

### 3.2 Cross-Experiment Insights  

- **Boosting outperformed conventional models:**  
  Income-based baselines (ROC AUC ≈ 0.65) were clearly surpassed by multi-dimensional boosting models (ROC AUC ≈ 0.77+).  

- **Stable hyperparameters:**  
  Shallow trees (depth 1–2) and moderate learning rates (0.1–0.2) yielded consistently strong results across experiments.  

- **Algorithm robustness:**  
  While XGBoost performed well, **CatBoost** proved more robust to categorical noise, confirming its advantage in socio-economic data.

---

# Scenario Experiments – Shock & Policy Simulations  

⚡ **This experiment (Exp6) presents the core analysis — examining how household vulnerability evolves under economic shocks and policy responses.**

It consists of three stages:  
(1) simulating external shocks,  
(2) evaluating different policy responses, and  
(3) implementing latent vulnerability–based policies — forming the basis of the project’s key findings.

---

## 🔹 Exp6-1: External Shock Simulation  
- **Design:**  
  - A simulated COVID-19 economic shock was applied to test model robustness.  
  - **Without retraining the model**, the pre-shock classifier was applied to post-shock data to evaluate **how well it could predict new vulnerability levels**.  
  - The external shock affected income, employment, and sector-specific variables, following evidence from the JILPT COVID-19 survey.  

- **Findings:**  
  - Mean household income fell ~20%.  
  - Employment precarization +16%.  
  - ROC AUC dropped from **0.911 → 0.872 (Δ −0.039)**.  
  - Class transition matrices showed substantial shifts toward more vulnerable classes.
---

## 🔹 Exp6-2: Policy Intervention Scenarios  
- **Tested policies:**  
  1. **Uniform ¥100,000 cash transfer** (Japan’s 2020 policy).  
  2. **Targeted cash transfer** (bottom 30% by income).  
  3. **Budget-neutral targeted transfer** (same total budget as the universal policy, concentrated in the bottom 30%).
 
- **Efficacy prediction**
  - Again, **without retraining the model**, the pre-shock classifier was applied to post-intervention data to evaluate **how well it could predict the effectiveness of the intervention**.  

- **Findings:**  
  1. **Universal cash:** Maximized equity but produced no major improvements among vulnerable groups (Classes 3–4).  
  2. **Targeted cash:** Had minimal impact on lower-middle households (Class 3) and only slight improvement in Class 4.  
  3. **Budget-neutral targeted:** Most effective overall, but the aggregate impact remained limited despite the concentrated transfer.
  - ROC AUC slightly dropped from **0.911 → 0.878 (Δ −0.033)** on average.  

---

## 🔹 Exp6-3: Latent Vulnerability Policy (LVP)  
- **Design:**  
  - Households were clustered by income and savings to identify the most financially fragile segment.  
  - Feature analysis revealed **latent vulnerabilities** such as foreign-born status, low financial literacy, high debt-to-income ratios, and precarious housing.  
  - The total budget (equal to the universal transfer) was **redistributed disproportionately** to those with higher latent vulnerability scores — embodying a budget-neutral yet need-sensitive policy logic.


### Comparative Feature Importance (Non-shock vs. Shock Contexts)
The following comparison illustrates how predictive features shift from financial indicators in stable conditions to structural disadvantages under stress.

| Context                | Key Predictors (Exp1–2, Non-shock)                          | Key Vulnerabilities (Exp6-3, Shock)                          |
|------------------------|--------------------------------------------------------------|-------------------------------------------------------------|
| **Financial capacity** | Income, savings, repayment history (late payments)           | High debt-to-income ratio                                   |
| **Employment/health**  | Employment status, severe illness                            | Employment precarization (within clusters)                  |
| **Household context**  | Foreign-born status, multigen households, childcare/eldercare costs | Foreign-born status, precarious housing                     |
| **Structural factors** | *(Dropped in baseline: financial literacy, digital access)*   | Low financial literacy, limited digital access               |

- **Non-shock (Exp1–2):** financial and health variables dominated prediction.  
- **Shock (Exp6-3):** structural factors — literacy, digital access, housing — became decisive under stress.  

### Results  
  - The policy produced strong stabilization within vulnerable clusters.  
  - ROC AUC declined moderately (**0.911 → 0.865, Δ −0.05**), reflecting a loss of separability as inequality decreased.  
  - This indicates not a degradation of model quality, but rather the **blurring of class boundaries** as vulnerable households improved.

### Insights  
- LVP demonstrates a **trade-off between fairness and predictability**: as redistribution becomes more equitable, predictive separability declines.  
- The results confirm that **predictors are context-dependent**:  
  - In stable periods, financial and health factors dominate.  
  - Under stress, *structural disadvantages* — literacy, housing, digital access — emerge as decisive drivers of vulnerability.  
- These findings imply that **structural reforms combined with financial transfers** are essential for sustained household resilience, because latent vulnerabilities tend to fully manifest only during economic shocks.
  
---

### Transition Matrix under LVP  

![LVPheatmaps2](https://raw.githubusercontent.com/Bi-BitArt/HarvardX-ML-Portfolio/main/projects/JPBoostingTest/EXP6_Scenarios/exp6-3_latent%20vulnerability%20policy/LVPheatmaps2.png)

*Heatmap of class transitions (0 = least vulnerable, 4 = most vulnerable).  
Rows: predicted class after **shock**, Columns: predicted class after **policy**.*

*Example: Uniform Cash (Universal) : Class 4 after the shock (row = 4).*
- 1.4% improved to Class 2.
- 10.1% improved to Class 3.
- 88.4% remained unchanged.

---

## Cross-Experiment Insights (Exp6)  

- **Shock sensitivity:**  
  Under economic shocks, model performance declines and calibration (stability of accuracy) becomes fragile.  
  However, the drop (Δ −0.039, about 4%) remains within a reasonable range.  
- **Latent vulnerability:**  
  By going beyond income-based measures, the model uncovers previously hidden risk factors and achieves significant improvements among vulnerable groups even under the same budget.  
  At the same time, class boundaries become blurred, leading to a moderate loss in predictive precision.  
- **Context shift:**  
  Predictive features differ between non-shock and shock settings.  
  Structural disadvantages that seem minor in stable periods — such as **low literacy, poor housing, and limited digital access** — emerge as decisive sources of vulnerability under stress.
