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



By the end, we should clearly see the different philosophies:

Bagging = parallel averaging of complex trees (variance reduction).

Boosting = sequential correction with simple trees (bias reduction).
