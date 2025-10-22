### Exp6-1: External Shock Scenario

#### Setup
Shock design was informed by the **Survey on the Impact of the COVID-19 Pandemic on Work and Daily Life** (Japan Institute for Labour Policy and Training, JILPT, Waves 1–7).  
See: [JILPT 2023 report (in Japanese)](https://www.jil.go.jp/institute/research/2023/229.html)

## External Shock Simulation

We simulated a COVID-type external shock by applying sector-specific income reductions, higher interest rates (+2pp), and shifts into more precarious employment categories. This design follows empirical evidence on how different groups were disproportionately affected during the pandemic.

---

### Variable Impact under Shock

- **annual_income:** mean −20.8%, median −20.2%  
- **By sector:** worst = 0 (−25.0%), best = 1 (−5.0%)  
- **employment_status:** 16.2% shifted to more precarious categories  
- **savings_rate:** average change +0.0 pp  
- **late_payment_12m:** average change +0.0 pp  
- **childcare_cost_share:** average change +0.0 pp  
- **eldercare_cost_share:** average change +0.0 pp  
- **By region:** worst hit region = 1 (−21.2%)  
- **foreign_born:** 17.9% experienced worsening employment status  
- **multigen_household:** income change non-multigen = −21.2%, multigen = −20.3%  

---

### Metrics

- **Baseline ROC AUC (macro OVR):** 0.911  
- **After Shock ROC AUC (macro OVR):** 0.872  
- **Δ AUC:** −0.039  

The external shock led to a notable decline in predictive accuracy, indicating that the model became less calibrated under stressed conditions.

---

### Transition Matrix

Class transitions before vs. after shock (predicted classes):

![Class Transition Matrix](classtransitionmatrix.png)

---

### Key Findings

- The shock predominantly shifted lower-to-middle classes (0–2) into higher vulnerability tiers.  
- Classes 1 and 2 experienced the largest upward transitions, highlighting the fragility of middle-income households under stress.  
- Nearly one-third of Class 3 moved into the most vulnerable group (Class 4), showing that even relatively stable households were not immune.  
- Overall model performance declined (ΔAUC −0.039), suggesting that predictive calibration weakens when the system is placed under external stress.
