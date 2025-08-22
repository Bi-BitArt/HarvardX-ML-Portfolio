# The First Step for Programming

This document is not just a summary of the first steps in programming with Python, but also a **reminder note**.
Whenever I get stuck during exercises, I may come back here and quickly review the essential imports and their roles.

---
### Sheet1: Import libraries

In this sheet, we import the most essential Python libraries for data analysis and machine learning.

`import numpy as np`

Pandas (handles tabular data)

Sklearn (builds and evaluates ML models)

Seaborn (visualizes statistical data)

Matplotlib (plots and customizes graphs)

Utilities: plot_boundary (decision boundaries), PrettyTable (summary tables)

Configs: pd.set_option(...), plt.rcParams[...] for display/plot settings

These imports prepare the environment so that the exercise can run smoothly.

---
### Sheet2: Read Data

In this sheet, load the training and testing datasets from CSV files into Pandas DataFrames and do a quick check.

`elect_train = pd.read_csv("election_train.csv")`

`elect_test  = pd.read_csv("election_test.csv")`

elect_train.head(): quick look at the training data

elect_train: training dataset

elect_test: testing dataset

.head(): verify columns and structure


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
