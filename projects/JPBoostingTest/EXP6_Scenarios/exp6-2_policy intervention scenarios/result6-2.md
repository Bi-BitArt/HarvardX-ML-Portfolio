### Exp6-2: Policy Intervention Scenarios

#### Setup
We simulate policy interventions under an external shock.  
Reference: Japan’s **¥100,000 uniform cash transfer (2020 Special Cash Payments)**, plus two targeting variants.

---

#### Policies
1. **Uniform Cash (Universal ¥100,000)** — all households receive ¥100,000.  
2. **Targeted Cash (Bottom 30%)** — only the lowest 30% by income receive ¥100,000.  
3. **Budget-Neutral Targeted (30%)** — same total budget as universal, concentrated on bottom 30% (≈¥333,000 each).  

---

#### Metrics
- **Baseline ROC AUC (macro OVR):** 0.911  

| Policy                         | ROC AUC | Δ AUC   |
|--------------------------------|---------|---------|
| Uniform Cash (Universal)       | 0.714   | −0.197  |
| Targeted (Bottom 30%)          | 0.766   | −0.144  |
| Budget-Neutral Targeted (30%)  | 0.788   | −0.123  |

---

#### Transition Matrices
![Policy Heatmaps](3policyheatmap.png)

---

#### Key Findings
- **Uniform Cash (Japan 2020 policy):**  
  - Maximized equity, but predictive accuracy collapsed (ΔAUC −0.197).  
  - **Class 3 disappeared entirely** — households skipped over the mid-tier and polarized into safer (Class 2) or more vulnerable (Class 4) groups.  
  - This shows that one-off universal transfers may unintentionally **erode the middle ground** and exacerbate instability.  

- **Targeted Cash (Bottom 30%):**  
  - Protected some mid-tier households (Class 2).  
  - However, higher classes (Class 3–4) were insufficiently protected.  

- **Budget-Neutral Targeted (30%):**  
  - Delivered the smallest decline in predictive accuracy (ΔAUC −0.123).  
  - **36% of Class 3 improved to Class 2**, showing that concentrated aid can stabilize borderline households.  
  - At the same time, **35% of Class 3 still escalated into Class 4**, showing a paradox: the same policy both rescued many and pushed others into deeper vulnerability.  
  - **No Class 4 households improved**, confirming that the most vulnerable remain trapped without deeper structural support.  
  - Taken together, this means that while **budget concentration helps those on the edge (Class 3)**, it also reveals their instability. One-off transfers act only as a temporary buffer — they cannot solve long-term or entrenched vulnerability.  

---

#### Interpretation
- **Trade-off:** equity vs. efficiency vs. protection.  
- **Universal transfer:** fair but inefficient; even risks erasing the middle tier (Class 3).  
- **Targeting:** improves efficiency, but fragile Class 3 and entrenched Class 4 remain critical weak points.  
- **Insight:** one-time cash helps the brink households, but **layered, sustained support** is essential for lasting protection.
