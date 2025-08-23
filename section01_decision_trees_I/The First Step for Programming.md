# The First Step for Programming

This document is not just a summary of the first steps in programming with Python (focusing on exercise #1), but also a **reminder note**.
Whenever I get stuck during exercises, I may come back here and quickly review the essential libraries and their roles.

---
### Sheet1: Import libraries

In this sheet, we import the most essential Python libraries for data analysis and machine learning.

`import numpy as np`

`from sklearn.tree import DecisionTreeClassifier`

Pandas (handles tabular data)

Sklearn (builds and evaluates ML models)

Seaborn (visualizes statistical data)

Matplotlib (plots and customizes graphs)

Utilities: plot_boundary (decision boundaries), PrettyTable (summary tables)

Configs: pd.set_option(...), plt.rcParams[...] for display/plot settings

These imports prepare the environment so that the exercise can run smoothly.

---
### Sheet2: Load Data

In this sheet, load the training and testing datasets from CSV files into Pandas DataFrames and do a quick check.

`elect_train = pd.read_csv("election_train.csv")`

`elect_test  = pd.read_csv("election_test.csv")`

elect_train.head(): quick look at the data

elect_train: defining training dataset

elect_test: defining testing dataset

.head(): checking columns and structure


---
### Sheet3: Split predictors and response variables (specifying features X and target Y)

We separate input features (X) and the target (y) for both training and testing.

`X_train = elect_train[["minority", "bachelor"]]`

`X_test = elect_test[["minority", "bachelor"]]`

X_train / X_test: use minority, bachelor as predictors

y_train / y_test: use won as the target label

- single bracket vs double brackets: while single bracket returns just one column (**Series(1D)**) as a vector, double brackts returns **DataFrame(2D)**.

---
### Sheet4: Initialize and fit decision trees (2 cols) (depth = 2, 10)

`dt1 = DecisionTreeClassifier(max_depth=2, random_state=0)`

`dt1.fit(X_train, y_train)`

DecisionTreeClassifier(max_depth=...): sets how deep the tree can grow (controls complexity).

random_state=0: removing randomness and ensuring reproductivity.

.fit(X_train, y_train): let the model train using the training dataset.

dt1: simple model (depth = 2).

dt2:  more complex model (depth = 10).

---
### Sheet5: Visualizing decision boundaries

`plot_boundary(elect_train, dt1, dt2)`

Note: Decision boundaries are drawn on the training data, because they are based on how the model was fit. Test data should only be used for evaluation, not for constructing boundaries.

---
### Sheet6: Train decision trees with multiple predictors (8 cols) (depth = 2, 10, 15)

In this sheet, we define a list of feature names and use it to extract multiple columns from the dataset at once.

`pred_cols = ['minority','density','hispanic','obesity',
             'female','income','bachelor','inactivity']`

`X_train = elect_train[pred_cols]`
`X_test  = elect_test[pred_cols]`

pred_cols: defined to include 8 predictor cols

elect_train[pred_cols]: selects all columns listed in pred_cols from the training dataset, returning a DataFrame with 8 columns.

In short, This approach avoids repetetition: just a shorcut.

---
### Sheet7: Compute train and test accuracy (evaluating models)

Accuracy test for training data:

- Can be < 100%

Shallow tree (too simple): can’t separate all training data → underfitting.

Data overlap/noise:some points always wrong.


- Can be = 100%

Deep tree (no limits): memorizes training data.

But this usually means overfitting.

`dt3_train_acc = dt3.score(X_train, y_train)`
`dt3_test_acc  = dt3.score(X_test, y_test)`

.score(X, y): scikit-learn method that returns the accuracy of the model.

Train accuracy: shows how well the model fits the training data.

Test accuracy: shows how well the model generalizes to unseen data.

Good fit:  both accuracies are high and close to each other.

---
### Sheet8: Display results as table (comparing accuracy scores)

`pt = PrettyTable()`
`pt.field_names = ['Max Depth', 'Number of Features', 'Train Accuracy', 'Test Accuracy']`
`pt.add_row([2, len(pred_cols), round(dt1_train_acc, 4), round(dt1_test_acc,4)])`
`pt.add_row([10, len(pred_cols), round(dt2_train_acc,4), round(dt2_test_acc,4)])`
`pt.add_row([15, len(pred_cols), round(dt3_train_acc,4), round(dt3_test_acc,4)])`
`print(pt)`

Taking the values we already computed in Sheet7 (dt1_train_acc, dt1_test_acc, …)

Arranging them into a table with labels: Max Depth, Number of Features, Train Accuracy, Test Accuracy

Printing the table in an organised form to make it easy to compare.


## Result & Insights

| Max Depth | Number of Features | Train Accuracy | Test Accuracy |
|-----------|--------------------|----------------|---------------|
| 2         | 8                  | 0.8924         | 0.8862        |
| 10        | 8                  | 0.9866         | 0.9126        |
| 15        | 8                  | 0.9996         | 0.9085        |


### Key Points
- **Depth 2**: Underfitting: too simple, both train/test accuracy are low.  
- **Depth 10**: Best balance: slightly lower train accuracy, but highest test accuracy.  
- **Depth 15**: Overfitting: almost perfect train accuracy, but test accuracy is interestingly lower than depth 10.  

### Insights
This illustrates the **bias–variance trade-off**:
- Shallow trees = **high bias**, poor generalization.  
- Medium depth = **best generalization**.  
- Deep trees = **high variance**, fit noise, hurt test accuracy.  

### Summary
Even if train accuracy is nearly 100%, test accuracy can be worse.  

This proves that **more complexity doesn't mean better real performance**.
