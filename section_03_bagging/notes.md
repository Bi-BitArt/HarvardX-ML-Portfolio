# Section 3: Bagging


## Key Concepts from Video 1

### Drawbacks of decision tree

- When a tree is too shallow, it cannot devide thee input data into enough regions: **underfits**
- When a tree is to deep, it cuts the input space into too many regions and fits to the noise of the data: **overfits**

Therefore,  one issue with dicision tree is determining the depth of the model to avoid underfitting / overfitting.

Plus, to capture a complex decision boundary, we must do axis-aligned splits (parellel to the coordinate axes (e.g. x,y,x)). As a result, we need to use a large tree or deep tree. However, those deep trees have high variance and are prone to overfitting.

For these reasons, decision tree models actually underperform in comparison to other classification or regression methods.


### Ensemble Learning

The intuition of ensemble learning is that it is a method to build a single model by training and aggregating multiple models. In short, this method is based on several models to produce one optimal predictive model.

