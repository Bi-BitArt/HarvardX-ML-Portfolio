# Exercise: Regression with Boosting

### Introduction

The goal of this exercise is to understand boosting step by step and then compare it against bagging.

- Part A: Build a gradient boosting model from scratch using decision stumps.

Fit one tree on the data, calculate residuals, fit another tree on residuals, and combine them.

Visualize how each stage corrects the mistakes of the previous model.


- Part B: Compare Gradient Boosting with Bagging.

Train both models on the airquality.csv dataset.

Visualize their predictions and compute their test MSE.

See how boosting reduces bias by sequentially correcting errors, while bagging reduces variance by averaging across bootstraps.

---

By the end, we should clearly see the different philosophies:

Bagging = parallel averaging of complex trees (variance reduction).

Boosting = sequential correction with simple trees (bias reduction).

---
### Sheet3: Handling Missing Values and Quick Data Check

`df = df[df.Ozone.notna()]`
`df.head()`

- Handling Missing Values:
The dataset airquality.csv contains missing values in the Ozone column.
Since decision trees (and most models in scikit-learn) cannot handle NaN values directly, we filter them out using notna().

- Quick Data Check:
Using df.head() allows us to quickly preview the cleaned dataset and confirm that the predictor (Ozone) and response (Temp) columns are intact.

- Takeaway:
Preprocessing, especially handling missing data is a critical first step before fitting models. Even simple filters like notna() can make the dataset ready for analysis.

---
### Sheet4: Defining Predictor and Response Variables

`x, y = df['Ozone'].values, df['Temp'].values`
Extracts the Ozone column as the predictor (x) and the Temp column as the response (y), converting them from pandas Series to NumPy arrays.

`x, y = list(zip(*sorted(zip(x,y))))`
Creates pairs of (Ozone, Temp), sorts them by Ozone, and then unpacks back into x and y. This ensures the data is ordered by the predictor for cleaner plotting.

`x, y = np.array(x).reshape(-1,1),np.array(y)`
Converts both back to NumPy arrays. x is reshaped into a 2D array (n_samples, n_features) as required by scikit-learn, while y stays 1D.

---
### Sheet5: First Decision Tree Stump

`basemodel = DecisionTreeRegressor(max_depth=1)`
Initializes a decision tree stump (a tree with only one split).

`basemodel.fit(x, y)`
Fits the stump on the entire dataset.

`y_pred = basemodel.predict(x)`
Generates predictions for all data points using the fitted stump.

---
### Sheet7: Calculating Residuals

`residuals = y - y_pred`
Computes the residuals as the difference between the true values (y) and the predictions (y_pred).

---
### Sheet9: Fitting a Tree on Residuals

`dtr = DecisionTreeRegressor(max_depth=1)`
Initializes another decision tree stump to model the residuals.

A stump (depth=1) makes exactly one split, producing two constant regions.

Keeping the learner weak (high bias, low variance) is intentional in boosting; we’ll add many small corrections rather than one complex model.


`dtr.fit(x, residuals)`
Fits the stump on the residuals instead of the original target.

The stump chooses a threshold on x that minimizes the sum of squared residuals within the two regions.

Each region’s prediction is the mean residual in that region, i.e., the constant amount needed to lift/lower the current model there.

Intuition: positive residuals → the model under-predicts (needs to go up); negative residuals → over-predicts (needs to go down).


`y_pred_residuals = dtr.predict(x)`
Predicts the residuals across all data points.

This is a piecewise-constant correction function: the amount to add/subtract in each region.

With MSE loss, the negative gradient w.r.t. predictions is proportional to the residuals, so this stump approximates a gradient step in function space.


Next Step:
After this, we form the boosted prediction
y_pred_new = y_pred + λ * y_pred_residuals,
where λ (learning rate) controls how strongly we apply this correction.

---
### Sheet11: Updating Predictions with Learning Rate

`lambda_ = 0.5`
Sets the learning rate λ, which controls how much of the residual correction to add. (Since Python does not allow using the Greek letter λ (and lambda is a reserved keyword), we explicitly define a variable lambda_ to represent the learning rate in code.)

`y_pred_new = y_pred + lambda_ * y_pred_residuals`
Updates the predictions by combining the original stump’s output with a scaled correction from the residual tree.

---
### Sheet13 Review: Train/Test Split

`x_train, x_test, y_train, y_test = train_test_split(
    x, y, train_size=0.8, random_state=102)`

---
### Sheet14: Boosting Model Setup

`l_rate = 0.1`
`boosted_model = GradientBoostingRegressor(
    n_estimators=1000,
    max_depth=1,
    learning_rate=l_rate)
boosted_model.fit(x_train, y_train)
y_pred = boosted_model.predict(x_test)`


---

l_rate = 0.1: sets the learning rate λ.

GradientBoostingRegressor(...): initializes a boosting model:

n_estimators=1000: number of weak learners (trees).

max_depth=1: each tree is a stump.

learning_rate=l_rate: scales each correction step.


.fit(x_train, y_train): trains the model on the training data.

.predict(x_test): predicts on the test data.

---
### Sheet15: Bagging Model Setup

num_bootstraps = 30
max_depth = 3

model = BaggingRegressor(
    estimator=DecisionTreeRegressor(max_depth=max_depth, random_state=3),
    n_estimators=num_bootstraps,
    max_samples=0.8,
    bootstrap=True,
    random_state=3
)

model.fit(x_train, y_train)


---

Explanation

num_bootstraps = 30: number of bootstrapped samples (and thus trees).

max_depth = 3: each tree can grow deeper than a stump, so bagging trees are more expressive.

BaggingRegressor(...):

estimator: base learner = Decision Tree (depth 3).

n_estimators=30: builds 30 trees.

max_samples=0.8: each tree trains on 80% of the data sampled with replacement.

bootstrap=True: enables sampling with replacement.


.fit(x_train, y_train) → trains the ensemble on training data.

---
### Sheet16: Visualizing Bagging vs Boosting

`xrange = np.linspace(x.min(), x.max(), 100).reshape(-1,1)`
`y_pred_boost = boosted_model.predict(xrange)`
`y_pred_bag = model.predict(xrange)`

---

Explanation

xrange: creates 100 evenly spaced points across the predictor range for smooth plotting.

boosted_model.predict(xrange): predictions from Gradient Boosting.

model.predict(xrange): predictions from Bagging.

---
### Sheet17: MSE for Boosting

`boost_mse = mean_squared_error(y_test, boosted_model.predict(x_test))`

---

Explanation

boosted_model.predict(x_test): predicts temperature values for the test set using the Gradient Boosting model.

mean_squared_error(y_test, ...): computes the average squared difference between true and predicted values.

---
### Sheet18: MSE for Bagging

`bag_mse = mean_squared_error(y_test, model.predict(x_test))`

---

Explanation

model.predict(x_test): predicts temperature values for the test set using the Bagging model.

mean_squared_error(y_test, ...): calculates the MSE between the true and predicted values.

---
### Insight

📊 Results: MSE Comparison

Model	Test MSE

Boosting	350.57
Bagging	355.96

---

Insight

Boosting achieved a slightly lower MSE than Bagging on this dataset.

This shows Boosting’s strength: it iteratively corrects errors (residuals), leading to finer adjustments and often better performance.

Bagging, on the other hand, stabilizes predictions by averaging deeper trees, but does not explicitly focus on residual correction.
