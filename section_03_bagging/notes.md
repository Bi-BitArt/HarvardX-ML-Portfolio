# Section 3: Bagging


## Key Concepts from Video 1


### Drawbacks of decision tree

- When a tree is too shallow, it cannot divide thee input data into enough regions: **underfits**
- When a tree is too deep, it cuts the input space into too many regions and fits to the noise of the data: **overfits**

Therefore,  one issue with decision tree is determining the depth of the model to avoid underfitting / overfitting.

Plus, to capture a complex decision boundary, we must do axis-aligned splits (parallel to the coordinate axes (e.g. x,y,x)). As a result, we need to use a large tree or deep tree. However, those deep trees have high variance and are prone to overfitting.

For these reasons, decision tree models actually underperform in comparison to other classification or regression methods.


### Ensemble Learning #1

The intuition of **Ensemble Learning** is that it is a method to build a single model by training and aggregating multiple models: 

In short, this method is based on several models to produce one optimal predictive model.

1. Use the same training subset.
2. Each model is then made to predict on the test subset.
3. These predictions are aggregated and combined to give one final prediction.

**Average** of the prediction is considered for regression, and **Majority** is considered for a classification.

As each model here is independent, the chance of all of them overfitting or underfitting is low. Hence, the performance of the model by combining individual model is more robust than single-model prediction.


### Ensemble Learning #2

There are several types of ensemble learning: **bagging**, **boosting**, **stacking**, **blending**. In this section, we look into bagging. 

By wait,in practice we do not have different datasets or different models. So, "How can we generate more datasets?"


### Bootstrap

We want multiple models trained on different datasets. **Bootstrap** is the way we generate more datasets. 

- Definition: Process of **sampling with replacement** from the dataset to create new datasets.
  
- Example: From [A, B, C, D, E], one bootstrap sample could be [A, A, C, E, E].
  
- Purpose: To generate multiple slightly different datasets, so we can train multiple models and combine them (Bagging).
  
- Key Idea: "With replacement" means the same data point can appear multiple times, or not at all.

Bagging: short for **Bootstrap Aggregating**


### Process of Bagging

1. Bootstrap: Generate multiple samples of training data via bootstraping. We train a deep decision tree on each sample data.
2. Aggregating: for a given input, we output the averaged outputs of all the models for that input.


### Pros and Cons of Bagging


- Pros:
1. High expresiveness: by using deeper trees each model is able to approximate complex functions and decision boundaries.
2. Low variance: averaging the prediction of all the models reduces the variance in the final prediction.


- Cons:
1. Interpretability: one can no longer trace the "logic" of an averaged output through a series of decisions.
2. Aggregating shallow trees cannot properly fit: underfitting.
3. The final model still overfits if individual trees are deep or large.


## Key Concepts from Video 2


### OOB Error #1

Cross-validation is a standard way to estimate model performance, but a direct technique is also needed for Bagging.  

We introduce **Out-of-Bag error (OOB)**.  

Assuming that we start with the data set containing different animals: cat, horse, dog, cow, and rat. We first sample using bootstrap and generate three different samples.  

Since the bootstrap is sampling with replacement, some observations do not end up in the final samples obtained from bootstrapping.  

We call animals (=observations) which are excluded as **Out-of-Bag**. OOB method determines the prediction **error while being trained**. 


### OOB Error #2

#### Left observations and prediction
- When training models with bootstrapped samples, **some observations are left out** in each bootstrap dataset.  
- For any given (left out) observation, we collect predictions **only from the models that did not see this observation** during training. 

#### Computing the OOB Error
- And then, compare the **pointwise prediction** with the **true label** of the observation.  
- The proportion of mismatches across all observations = **OOB error**.  
- **Classification:** each tree votes for a class (0 or 1), and the **majority vote** is compared with the true label. The error ranges between 0 (perfect prediction) and 1 (all wrong).  
- **Regression:** we take the mean of the predictions and then compute the OOB error as the squared difference between the prediction and the true value (not necessarily restricted to 0–1).  

#### Difference of averaged prediction
In case of regression, we take the mean of the predictions and then compute the OOB error to be the square of the difference between the prediction and the true value.  

In case of classification on the other hand, it would be the **majority vote** that decides the predicted label, and then we compute the error by comparing it with the true label.  

---

### Summary of OOB method

1. For each data point, we average the predicted outputs (regression) or take a majority vote (classification). To do so we only use **the trees whose bootstrap training set excludes this certain point**. Then we obtain the **Point-wise Prediction**.  
2. We compute the error of this prediction (to compare with the true label). We call this value **Point-wise OOB error**.  
3. We average the PW OOB error over the full training set.  

---

### Why it matters

- While using the cross-validation technique, every validation set has already been seen in training by a few decision trees and hence there is a data leakage.
- OOB Error prevents leakage and yield a better model with lower variance or less overfitting.
- There is also lesse computational cost for OOB as compared to CV for bagging.

---

### Improving on Bagging

- In practice, the trees in Bagging are likely to be highly correlated. If there is an exceptionally strong predictor in the training set amongs model predictors, the greedy algorithm ensures that most of the models in the ensemble will choose to split on this "exceptionally strong predictor" in early phases.
- However, we hope that each tree in the ensemble is independently and identically distributed.



# Exercise 3-1: Bagging with Regression

The goal of this exercise is to understand how **Bagging (Bootstrap Aggregating)** can improve prediction stability and accuracy by using **Decision Tree Regressors** as base learners.  

---
### Sheet2 Splitting Data into Train and Test Sets

`x_train, x_test, y_train, y_test = train_test_split(x, y, train_size=0.8, random_state=102)`

Splits the dataset into training (80%) and test (20%).

---
### Sheet6 Defining and Fitting a Bagging Regressor

`model = BaggingRegressor(
    estimator=DecisionTreeRegressor(max_depth=max_depth, random_state=0),
    n_estimators=num_bootstraps,
    bootstrap=True,
    random_state=0
)`

`model.fit(x_train, y_train)`

BaggingRegressor: Combines multiple Decision Trees into one ensemble.

max_depth=3: Limits complexity of each tree.

n_estimators=30: Number of bootstraps/trees.

bootstrap=True: Sampling with replacement to create different training subsets.

fit(): Trains the ensemble on training data.

---
### Sheet8 Test MSE of a Single Estimator

`y_pred1 = model.estimators_[0].predict(x_test)
print("The test MSE of one estimator in the model is", round(mean_squared_error(y_test, y_pred1), 2))`

model.estimators_[0]: Accesses the first trained tree.

predict(): Gets predictions from this single tree.

mean_squared_error(): Compares predictions with y_test.

Usually higher error compared to the full bagging model.

---
### Sheet9 Test MSE of the Full Bagging Model

`y_pred = model.predict(x_test)
print("The test MSE of the model is", round(mean_squared_error(y_test, y_pred), 2))`

Uses all trees combined for prediction.

Final prediction = average of all individual trees’ predictions.

Generally achieves lower error than a single tree.

---
### Insights

Variance Reduction: Bagging reduces variance by averaging multiple noisy trees.

OOB vs Cross-validation: OOB can also estimate error without needing a separate validation set.

Tradeoff: More estimators → lower variance, but higher computation cost.

# Exercise 3-2: Bagging Classification with Decision Boundary


---
### Sheet2 Defining Predictors and Response Variables

`X = df[["latitude", "longitude"]].values`  

Extracts the **latitude** and **longitude** columns from the DataFrame and stores them as a NumPy array.  

These serve as the **predictor variables** (features).

`y = df["land_type"].values`  

Extracts the **land_type** column from the DataFrame and stores it as a NumPy array.  

This is the **response variable** (target label).  

**Note:** The dataset was originally referred to as `agriland`, but in my case the DataFrame was named `df`.  
Consistency in variable naming is important to avoid errors.

---
### Sheet6 Evaluating Single Decision Tree Accuracy 


1. Prediction
   
`prediction = clf.predict(X_test)`

- uses the trained decision tree (clf)

- takes X_test as input (latitude & longitude)

- **outputs predicted class labels for each test point**


2. Accuracy

`single_acc = accuracy_score(y_test, prediction)`

- compares predicted labels with true labels (y_test)

- **returns proportion of correct predictions (0–1 range)**

- multiply by 100 to show as a percentage

In short: predict with the trained model, then check how many predictions match the ground truth.


---
### Sheet: Implementing Bagging Prediction Function  

#### Function: `prediction_by_bagging()`  

This function manually implements the **Bagging (Bootstrap Aggregation)** procedure for classification.  



#### 1. Bootstrap Sampling  

`resample_indexes = np.random.choice(np.arange(y_train.shape[0]), size=y_train.shape[0])`
X_boot = X_train[resample_indexes]
y_boot = y_train[resample_indexes]

Randomly samples indices with replacement to create a new training subset.

Ensures both predictors (X_boot) and labels (y_boot) stay aligned.



---

#### 2. Base Learner Training

`clf = DecisionTreeClassifier(max_depth=max_depth, random_state=44)`
`clf.fit(X_boot, y_boot)`

Trains a Decision Tree classifier on each bootstrap sample.

Fixed random_state=44 for reproducibility.



---

#### 3. Prediction on Evaluation Set

`pred = clf.predict(X_to_evaluate)`
predictions.append(pred)

Each tree makes predictions on the same evaluation data.

Predictions are stored for later aggregation.



---

#### 4. Aggregation (Majority Voting)

`predictions = np.array(predictions)`
`average_prediction = (predictions.mean(axis=0) > 0.5).astype(int)`

Converts predictions into an array shaped (num_bootstraps, n_samples).

Takes the mean across all trees.

Applies a threshold at 0.5 → class 1 if >0.5, otherwise class 0.


  
