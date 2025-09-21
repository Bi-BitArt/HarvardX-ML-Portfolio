### Experiment 1-2: Feature Selection (SelectFromModel)

#### Performance
- **Accuracy:** 0.67 (vs. 0.68 baseline)  
- **ROC AUC:** 0.75 (vs. 0.77 baseline)  
→ Performance dropped only slightly, essentially maintained.  

#### Features
- **Kept (n=11):**  
  annual_income, employment_status, severe_illness, region, late_payment_12m,  
  savings_rate, sector, foreign_born, multigen_household, childcare_cost_share, eldercare_cost_share  

- **Dropped (n=11):**  
  debt_to_income, mortgage_ltv, financial_literacy, housing_age_years, side_income,  
  housing_type, insurance_cover, commute_time, utilities_cost_share, education_head, digital_access  

#### Top 5 Features by Importance (baseline model)
1. annual_income (0.145)  
2. employment_status (0.105)  
3. severe_illness (0.080)  
4. region (0.063)  
5. late_payment_12m (0.053)  

#### Interpretation
- The model emphasized **income, employment, health, region, and repayment history** as the strongest signals of vulnerability.  
- Indirect factors such as housing conditions or financial literacy were dropped.  
- This demonstrates that a **smaller, core set of features** can preserve predictive performance while improving interpretability and efficiency.
