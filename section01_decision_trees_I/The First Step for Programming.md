### The First Step for Programming

This document is not just a summary of the first steps in programming with Python, but also a **reminder note**.
Whenever I get stuck during exercises, I may come back here and quickly review the essential imports and their roles.

---
### Sheet1: Load election train dataset (loading data)

In this sheet, we import the most essential Python libraries for data analysis and machine learning.

Numpy (calculates with arrays and matrices)

Pandas (handles tabular data)

Sklearn (builds and evaluates ML models)

Seaborn (visualizes statistical data)

Matplotlib (plots and customizes graphs)

Helper functions (plot_boundary, PrettyTable) for visualization and summarizing results


These imports prepare the environment so that the rest of the exercise can run smoothly.

---
### Sheet2: Split train and test datasets (train–test split)

In this sheet, we separate the input features (X) and the target (y) for both training and testing datasets.

X_train = elect_train[["minority", "bachelor"]]

X_test  = elect_test[["minority", "bachelor"]]

y_train = elect_train["won"]

y_test  = elect_test["won"]


This step ensures that:

1. The model has the correct predictors (minority, bachelor) as input.


2. The response (won) is set as the variable to predict.


3. Both training and testing data are structured consistently.



This preparation is necessary so the Decision Tree can be trained on the input features and evaluated on unseen test data.

---
### Sheet3: Split predictors and response variables (specifying features X and target Y)
---
### Sheet4: Initialize and fit decision trees (depth = 2, 10)
---
### Sheet5: Plot decision boundaries of classifiers (visualizing model boundaries)
---
### Sheet6: Train decision trees with multiple predictors (depth = 2, 10, 15)
---
### Sheet7: Compute train and test accuracy (evaluating models)
---
### Sheet8: Display results as table (comparing accuracy scores)
