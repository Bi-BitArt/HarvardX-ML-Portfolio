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
1. Interpretability: Average model is no longer easiliy interpretable: one can no longer trace the "logic" of an output through a series of decisions.
2. Aggregating shallow trees cannot properly fit: underfitting.
3. The final model still overfits if individual trees are deep or large. 
