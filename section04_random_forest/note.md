# Section 4: Random_Forest

## Preparation knowledge

### **Variance**

Variance in machine learning means the **instability of model predictions** when the training data changes slightly.

- A deep decision tree has low bias but high variance — small changes in data can cause large changes in the tree structure and predictions.

- Bagging reduces variance by averaging predictions across many trees.


## Key Concepts from Video 1

### A drawback in bagging method

In short, while bagging is a powerful method for overcoming high variance and overfitting, we should still be careful about the possibility that if one feature is dominant out of the others, the ensemble of trees in bagging tend to be highly correlated.

### Random Forests

**Random Forest** is a modified form of bagging that creates ensembles of **independent** decision trees.

1. Bootstrap sampling: 
Start with a dataset that contains multiple predictors (e.g., 5 predictors).
Just like in bagging, we first create bootstrap samples of the dataset.

2. Random feature selection at each node: 
Instead of searching for the best split across all predictors,
Random Forest selects the best split from a randomly chosen **subset of predictors from the full set of predictors at each node.
This reduces correlation between trees and increases diversity in the ensemble.

3. Grow multiple independent trees: 
Repeat steps 1 and 2 to grow many different decision trees.
Each tree is trained on a bootstrap sample, with random subsets of features considered at each split.

4. Aggregate predictions: 
Finally, combine predictions from all trees:

For classification: use majority voting.

For regression: take the average of predictions.


### Difference Between Bagging and Random Forest

**Bagging:**
- Uses **all features** in order to "search" for the best prediction when splitting at each node.

- Trees may become highly correlated if one strong feature dominates.  
- Variance is reduced through averaging, but correlation among trees can still limit performance.  

**Random Forest:**
- At each split, considers only a **random subset of features**.  
- Adds extra randomness, which decorrelates the trees.  
- By lowering correlation, Random Forest achieves **better generalization and accuracy** compared to plain bagging.  

**Key Idea:**  
Random Forest = Bagging + Random Feature Selection  

👉 In short, Bagging searches the best split using **all features**,  
while Random Forest searches it using **only a random subset of features**,  
which reduces correlation among trees.


### Hyper-parameters

Random forest models have multiple **hyper-parameters to tune**: 

1. The **number of predictors** to randomly select at each split.
2. The **total number of trees** in the ensemble.
3. The **stopping criteria** - maximum depth, minimum leaf node size, etc. 

However, there are standard (default) values for each of the random forest hyper-parameters, recommended by long time practitioners. Basically, for the number of trees, we use OOB.


## Key Concepts from Video 2


### Variable Importance for RF

1. Mean Decrease in Impurity
2. Permutation importance
3. LIME, SHAP values

### MDI (Mean Decrease in Impurity)

Decision trees make splits that maximize the decrease in impurity. By calculating the mean decrease in impurity for each feature across all trees, we arrive at the importance for a bagging or random forest model.

(The feature with the highest MDI score is usually considered the most important, but due to bias and correlation issues, it should not be taken as the only optimal choice.)

### Summary: MDI

**Goal:** Measure how much each feature contributes to reducing impurity across all trees in a Random Forest.

1. Calculate Node-Level Decrease
- For each split in the decision tree, compute how much the impurity (e.g., Gini, entropy, MSE) decreases.  
- Do this for every feature used in a split.

2. Aggregate per Feature
- For a given feature, sum up all impurity decreases across all nodes where it was used.  
- This gives the **importance score** for that feature in one tree.

3. Normalize
- Normalize the importance scores so that all features together sum to 1.  
- Ensures comparability across features.

4. Average Across Trees
- In Random Forests, compute the average feature importance over all trees.  
- Final output: **Mean Decrease in Impurity (MDI)** for each feature.


## Key Concepts from Video 3


### What is Permutation Importance?  
Permutation Importance is a technique to evaluate **feature importance** in a trained model (such as Random Forests).  
The key idea is: **if shuffling (permuting) a feature’s values decreases the model’s accuracy, then that feature was important.**  

This provides an intuitive way to measure how much each predictor contributes to the model’s performance.  


### Step-by-Step Process

1. **Baseline Accuracy**  
   - Record the validation accuracy (or OOB accuracy) of the original model.  
   - This serves as the reference point.  

2. **Permute One Feature**  
   - Randomly shuffle the values of a selected predictor column in the validation/OOB dataset.  
   - This breaks the relationship between that feature and the target.  

3. **Recalculate Accuracy**  
   - Measure the model’s validation/OOB accuracy again using the permuted dataset.  

4. **Compute Importance**  
   - The difference between the original accuracy and the permuted accuracy = **importance score** for that feature.  
   - Larger drop = more important feature.  

5. **Repeat for All Features**  
   - Apply steps 2–4 to each predictor in turn.  
   - This produces an importance ranking across all features.


### Example Intuition
- If permuting **Height** reduces accuracy from 0.88 → 0.70, Height is very important.  
- If permuting **Weight** reduces accuracy from 0.88 → 0.87, Weight is much less important.  

Thus, PI quantifies **how much each predictor directly affects prediction accuracy**.


### Advantages
- **Model-agnostic**: Works for any trained model, not just tree-based methods.  
- **Intuitive**: Directly measures impact on predictive performance.  
- **Complementary**: Provides a different perspective compared to impurity-based measures (e.g., MDI).  


### Limitations
- **Correlated features**: If two predictors are strongly correlated, shuffling one may not reduce accuracy much (since the other still carries the signal). This can lead to underestimating their importance.  
- **Computational cost**: Requires repeated evaluation of the model, which can be slow for large datasets.  


### Why It Matters
- Helps us understand **which predictors the model relies on most**.  
- Acts as a **diagnostic tool** for feature relevance.  
- Can inform **feature selection** or help identify **data leakage**.  


### **Key Idea:**  
Permutation Importance measures feature importance by **destroying the feature’s information (via shuffling) and observing how much the model’s accuracy drops.**


## Key Concepts from Video 4


### Missing Data

Real-world datasets most often having missing values. When dealing with missing data, we can impute as usual. However, there is another strategy for handling missing data in trees: called **surrogate splits**.

Basically, we find alternative splits, which we refer to as surrogate splits that can be used during prediciton.

### Surrogate Splits

1. **Regular Split First**  
   - The tree algorithm chooses the best predictor and threshold for the main split (as usual).  

2. **Find Surrogates**  
   - If a value is missing for the main split feature, the algorithm looks for other predictors that best **mimic** the split decision.  
   - These backup splits are called **surrogate splits**.  

3. **Use in Prediction**  
   - During prediction, if the primary splitting feature is missing for a data point, the tree will fall back to the surrogate split(s).  
   - This ensures the model can still classify/regress the observation without discarding it.  

### Why Surrogate Splits Matter
- Allow trees to handle missing data **without imputation**.  
- Preserve information that might otherwise be lost.  
- Especially useful when missingness is frequent or systematic.  

**Key Idea:**  
Surrogate splits act as **backup rules** that approximate the main split, so the tree can continue making decisions even when some values are missing.  
If the value needed for the main split is missing, the model automatically falls back on a surrogate split (a similar rule based on another feature) to decide the path.


## Key Concepts from Video 5


### Imbalanced Data

- Accuracy is a great measure, but only when you have balanced datasets (false negative and false positive counts are close and have similar costs).

In case of imbalanced datasets, **F1** os a better option.

`F1 = 2 (Recall * Precision) / (Recall + Precision)`

- IF the costs and false negatives & false positives are different, the ROC curve allows us to find the classification threshold that gives the best trade-off between FP rate and TP rate, which we need in this time.

We summarize the ROC by computing the Area Under the ROC curve (AUC).


### Dealing with Imbalanced Classes

There are three main ways of dealing with imbalanced classes: undersampling, oversampling, and class weighting.

1. Undersampling

#### Random Sampling

Randomly sample from majority class with or without replacement.

#### Near Miss

Select data points by using simple heuristics like finding samples from which the average distance to some data points of miority class is smallest.

Reduce the number of samples in the majority class.

Pros: Faster, smaller dataset.

Cons: May lose useful information.


2. Oversampling

Increase the number of samples in the minority class (e.g., duplicating or generating synthetic data like SMOTE).

Pros: Keeps all original data.

Cons: Risk of overfitting if data is just duplicated.


3. Class Weighting

Assign higher weight (importance) to the minority class in the model’s loss function.

Pros: No change to dataset size.

Cons: Might make model sensitive to noise in minority class.


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
