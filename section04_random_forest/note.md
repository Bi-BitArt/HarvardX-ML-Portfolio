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
