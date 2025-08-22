# The First Step for Programming

This document is not just a summary of the first steps in programming with Python (focusing on exercise #1), but also a **reminder note**.
Whenever I get stuck during exercises, I may come back here and quickly review the essential imports and their roles.

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
### Sheet4: Initialize and fit decision trees (depth = 2, 10)

`dt1 = DecisionTreeClassifier(max_depth=2, random_state=0)`

`dt1.fit(X_train, y_train)`

DecisionTreeClassifier(max_depth=...): sets how deep the tree can grow (controls complexity).

random_state=0: ensures reproducibility of results.

.fit(X_train, y_train): trains the model on the training dataset.

dt1: simple model (depth = 2).

dt2:  more complex model (depth = 10).

---
### Sheet5: Plot decision boundaries of classifiers (visualizing model boundaries)
---
### Sheet6: Train decision trees with multiple predictors (depth = 2, 10, 15)
---
### Sheet7: Compute train and test accuracy (evaluating models)
---
### Sheet8: Display results as table (comparing accuracy scores)
