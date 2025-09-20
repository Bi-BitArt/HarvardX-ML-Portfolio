### Experiment 4-2: Model comparison with alternative boosting models

All models were evaluated under the **same fixed hyperparameters**:  
- n_estimators = 100  
- learning_rate = 0.1  
- max_depth = 3  

Dataset: synthetic 5-class household vulnerability dataset with injected noise (missing values and random perturbations).  

#### Results

| Model                | Accuracy | Macro F1 | ROC AUC (macro OVR) | Notes |
|-----------------------|----------|----------|----------------------|-------|
| **XGBoost**          | 0.66     | 0.57     | 0.91                 | Baseline reference |
| **CatBoost**         | 0.75     | 0.70     | 0.94                 | Best overall, but slower |
| **LightGBM**         | 0.71     | 0.64     | 0.92                 | Fast and balanced |
| **AdaBoost**         | 0.48     | 0.18| 0.76                 | Weak on this dataset; but featured in HarvardX|
| **GradientBoosting** | 0.66     | 0.59     | 0.91                 | Classical scikit-learn implementation |



#### Observations
- **CatBoost** delivered the best results, outperforming others in both accuracy and ROC AUC, though with longer training time.  
- **LightGBM** achieved a good trade-off between speed and performance, making it suitable for larger-scale datasets.  
- **XGBoost** provided a solid baseline, consistent with previous experiments.  
- **GradientBoosting (scikit-learn)** matched XGBoost in performance but is generally slower and less optimized.  
- **AdaBoost** struggled in the noisy 5-class setting, which is expected since it was originally designed for simpler binary tasks.  

> **Note:** AdaBoost is included mainly for pedagogical reasons, as it was the central boosting algorithm taught in the HarvardX/edX Machine Learning course.
