# Exercise #1 Bagging vs Random Forest 

The goal of this exercise is to investigate the correlation between randomly selected trees from Bagging and Random Forest. 

## Bagging Implementation

---
### Sheet4: Assigning Predictors and Response Variables

In this sheet, we separate the **predictor variables (X)** from the **response variable (y)**.

`X = df.drop("Outcome", axis=1)`  
`y = df["Outcome"]`  

- `axis=1` ensures that we are dropping a **column** (not rows).

### Understanding `df.drop`

When defining predictors (**X**) and the response (**y**), we use `df.drop` to exclude the target column:

- **X (predictors):**  
  `X = df.drop("Outcome", axis=1)`  
  - `"Outcome"` is the target variable, so we remove it from the DataFrame.  
  - `axis=1` tells pandas to drop a **column** (instead of a row).  
  - Result: `X` contains all predictor columns **except** `"Outcome": axis=1`.

- **y (response):**  
  `y = df["Outcome"]`  
  - We directly select the `"Outcome"` column as the response variable.  
  - Result: `y` contains only the target labels.  

**Key Idea:**  
`df.drop("Outcome", axis=1)` ensures predictors exclude the target.

---
### Review: train_test_split

`train_test_split` is a utility function in scikit-learn for splitting data into training and testing (or validation) sets.  
This allows us to evaluate model performance on unseen data.

`X_train, X_val, y_train,y_val = train_test_split(X, y, train_size=0.8, random_state=144)`

---
### Review: Bagging Classifier

`BaggingClassifier( estimator=basemodel, n_estimators=n_estimators, random_state=random_state )`

- `estimator=basemodel` → base learner is the Decision Tree defined earlier.  
- `n_estimators=n_estimators` → number of trees (here, 1000).  
- `random_state=random_state` → reproducibility of sampling and training.  
- Bagging (Bootstrap Aggregating) reduces variance by combining many trees trained on different bootstrap samples.  

---

`bagging.fit(X_train, y_train)`

- Each estimator: draws a bootstrap sample, trains a Decision Tree (`max_depth=20`).  
- After this, the ensemble can predict with `.predict()` or `.predict_proba()`.  

---
### Sheet9 Validation Accuracy with Bagging

`predictions = bagging.predict(X_val)`

Explanation

- The output predictions is an array of predicted class labels for each row in X_val.

---

`acc_bag = round(accuracy_score(predictions, y_val), 2)`

Explanation

- Computes the accuracy score between predicted labels and the true labels of the validation set.

- The round(..., 2) part rounds the accuracy to 2 decimal places for readability.

- The result is stored in acc_bag.

---
## Random Forest Implementation

### Review: RandomForestClassifier

`RandomForestClassifier( max_depth=max_depth, n_estimators=1000, random_state=random_state )`

**Key parameters**  
- `max_depth=max_depth` → limits the depth of each Decision Tree (here, 20).  
- `n_estimators=1000` → the number of Decision Trees in the forest.  
- `random_state=random_state` → ensures reproducibility.  
  - Using a variable (`random_state`) avoids redundancy; change the value once and it propagates everywhere.  

**Note on "estimator"**  
- In ensembles like **BaggingClassifier**, `estimator` specifies the base model (e.g., Decision Tree).  
- In `RandomForestClassifier`, you do **not** need to pass `estimator` explicitly because it is always a Decision Tree.  
- So here, each of the 1000 trees is automatically a `DecisionTreeClassifier`.

---
### Sheet18 Predictions and Accuracy

`predictions = random_forest.predict(X_val)`

- Calls the .predict() method of the trained RandomForestClassifier.

- Takes the validation feature set X_val as input.

- Produces an array of predicted class labels, stored in predictions.


1. random_forest has already been trained with fit(X_train, y_train).

2. .predict(X_val) uses the learned decision rules (majority voting across all trees) to assign labels to the unseen validation data.

3. The output is a NumPy array (or similar structure), where each element corresponds to the predicted label for one sample in X_val.

---

`acc_rf = round(accuracy_score(predictions, y_val), 2)`

- Computes the accuracy of the Random Forest model’s predictions on the validation set.

- Stores the result in the variable acc_rf, rounded to 2 decimal places.

1. accuracy_score(predictions, y_val) calculates the fraction of correctly predicted labels.

Normally, the function is called as accuracy_score(y_true, y_pred): so the arguments should be accuracy_score(y_val, predictions).

2. The result is a floating-point number between 0 and 1 (e.g., 0.87).

3. round(..., 2) limits the value to two decimal places (e.g., 0.87 instead of 0.87321).

4. The final accuracy value is saved as acc_rf.

---
# Exercise #2  Hyperparameter Tuning

### Sheet1 Vanilla Random Forest

`vanilla_rf = RandomForestClassifier(random_state=seed)`

- Defines a Random Forest classifier with default parameters and sets the random seed for reproducibility.  

`vanilla_rf.fit(X_train, y_train)`

- Fits the model on the training data.  

`y_proba = vanilla_rf.predict_proba(X_test)[:, 1]`

- Uses the trained model to predict class probabilities on the test set.  
- Selects the probability for the positive class (index 1).  

`auc = np.round(roc_auc_score(y_test, y_proba), 2)`

- Computes the ROC AUC score comparing the true labels (`y_test`) with the predicted probabilities.  
- Rounds the score to 2 decimal places.

---
### Sheet18 Hyperparameter Tuning in Random Forest

**Goal**  
Improve the performance of Random Forest by adjusting its key hyperparameters beyond the default settings.

---

**Key Tunable Hyperparameters**

- `n_estimators`  
  Number of trees in the forest. More trees reduce variance but increase training time.  

- `min_samples_leaf`  
  Minimum number of samples required at a leaf node. Larger values make trees less complex, reducing overfitting.  

- `max_features`  
  Number of features to consider at each split.  

- `oob_score=True`  
  Enables Out-of-Bag (OOB) score as an internal validation measure, using samples not included in each bootstrap.

---
# Exercise #3  Feature Importance

### Sheet5 Decision Tree Permutation Importance

`tree_result = permutation_importance(tree, X, y, random_state=random_state)`

- Calls `permutation_importance` from `sklearn.inspection`.  
- Uses the trained Decision Tree (`tree`) to evaluate feature importance.  
- The output `tree_result` contains:  
  - `importances_mean`: average importance score for each feature.  
  - `importances_std`: standard deviation across the shuffles.  
  - `importances`: raw scores for each shuffle.- Calls

### Insight

- **Single Tree**
  - Certain features (e.g., ChestPain, Ca) appear highly dominant.  
  - Because the model relies on a single tree, the features chosen for splits can be overemphasized.  
  - Other features tend to be undervalued, showing little contribution.  

- **Random Forest**
  - Trained as an ensemble of many trees, importance values are distributed more evenly.  
  - Features that looked extremely strong in the single tree are toned down, while other variables gain moderate contributions.  
  - Produces more stable and generalizable importance estimates.  
**Key Insight**  
- A single decision tree tends to **overstate the role of a few splitting features**, while Random Forest averages across trees to provide a **more balanced and reliable view of feature importance**.  
- Therefore, when interpreting feature importance, Random Forest is generally the more trustworthy choice.
 
---
# Exercise #4 Random Forest with Class Imbalance

**Goal**  
Investigate the performance of Random Forest on a dataset with class imbalance, and apply different correction strategies to improve it. Finally, compare models using F1-score and ROC AUC.

### Sheet3 Review: F1-score and AUC

`f_score = f1_score(y_val, predictions)`  
- Computes the **F1-score**, which is the harmonic mean of precision and recall, useful for imbalanced datasets.
- Inputs:  
  - `y_val`: the true labels of the validation set.  
  - `predictions`: the predicted labels from the model.  

`score1 = round(f_score, 2)`  
- Rounds the F1-score to two decimal places. (e.g. 0.8461538... to 0.85 )
- Stores the result in `score1` for reporting.  

---

`auc_score = roc_auc_score(y_val, vanilla_rf.predict_proba(X_val)[:, 1])`  
- Computes the **ROC AUC (Area Under the Curve)** score.  
- `vanilla_rf.predict_proba(X_val)[:, 1]` extracts the predicted probabilities for the positive class.  
- Measures the model’s ability to rank positive samples higher than negative ones, independent of a classification threshold.  

`auc1 = round(auc_score, 2)`  
- Rounds the AUC score to two decimal places.  
- Stores the result in `auc1` for reporting.

---
### Sheet 10 Upsampling

`sm = SMOTE(random_state=2)`
- Initializes SMOTE (Synthetic Minority Oversampling Technique).

`X_train_res, y_train_res = sm.fit_resample(X_train, np.ravel(y_train))`
- Applies SMOTE to the **training** data to balance classes by creating synthetic minority samples.
- `np.ravel(y_train)` flattens the target to a 1D array (some resamplers require this).
- Outputs the resampled features `X_train_res` and labels `y_train_res` for subsequent model training.

---
### Sheet 11 Downsampling

`rs = RandomUnderSampler(random_state=2)`  
- Initializes a RandomUnderSampler with a fixed random state for reproducibility.  
- Randomly reduces the number of majority class samples until both classes are balanced.  

`X_train_res, y_train_res = rs.fit_resample(X_train, np.ravel(y_train))`  
- Applies downsampling to the training data.  
- `np.ravel(y_train)` flattens the target array into 1D, which is required by some resamplers.  
- Produces a balanced dataset where the majority class has been reduced in size.  
- Outputs `X_train_res` (features) and `y_train_res` (labels) that can be used to train a Random Forest.  

**Key Idea**  
- Unlike SMOTE, which **adds synthetic samples**, downsampling reduces the size of the majority class.  
- This helps the classifier pay more attention to the minority class, but at the cost of losing information from discarded samples.

---
## Result & Insights

| Strategy                          | F1 Score | AUC Score |
|-----------------------------------|----------|-----------|
| Random Forest — No imbalance corr | 0.51     | 0.80      |
| Random Forest — Balanced Subsample| 0.45     | 0.73      |
| Random Forest — Upsampling (SMOTE)| 0.54     | 0.74      |
| Random Forest — Downsampling      | **0.64** | 0.77      |

---

### Insights
- **Vanilla RF** → Achieved the highest AUC (0.80), but only moderate F1 (0.51). The model tends to misclassify minority class samples.  
- **Balanced Subsample** → Both F1 (0.45) and AUC (0.73) dropped. Simple class weighting did not improve performance here.  
- **Upsampling (SMOTE)** → Slightly improved F1 (0.54) but reduced AUC (0.74). This helps recall minority cases but hurts ranking ability.  
- **Downsampling** → Achieved the best F1 (0.64), with a reasonable AUC (0.77). It provided the best balance between precision and recall.  

**Conclusion**  
- If the goal is to reduce minority class misclassification (**F1-focused**), **Downsampling** is the best strategy.  
- If the goal is better ranking performance (**AUC-focused**), **Vanilla RF** remains stronger.
