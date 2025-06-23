# Section 2: Decision Trees II


## Preparation knowledge

- **Regression**

Decision trees are often utilized for classification tasks. However, they can also serve as a tool for regression - that is, **predicting** a quantitative outcome.

(At first, I imagined regression as "going back" through the decision tree,  
like using feature parameters to trace the identity of something.  
But I realized that regression still works by going forward - 
the tree uses those parameters to split the input space and reach a numerical output at the end.)


## Key Concepts from Video 1


### Example of regression1


Suppose there are many features, in Euclidean space.  
If one of these features has an `x` value > 6.5, it is anticipated to have a `y` value of 0.697,  
since this `y` value is the **average** of all training points in that region.  
If `x` is less than 6.5, it is anticipated to have a `y` value of -0.008,  
because this is the average of the target values in that region.

In short, regression trees split the input space using feature values,  
and **predict the average target value** in each resulting region.


### MSE, Mean Squared Error

**MSE = average of (prediction − actual value)²**

We calculate the **weighted MSE** like this:

(minimize over p and tₚ)
→ [ (N₁ / N) × MSE(R₁) ] + [ (N₂ / N) × MSE(R₂) ]

- It measures how far the predicted values are from the actual values.
- The differences are squared, so **larger errors are penalized more**.

- When calculating the MSE, we need to consider the number of points in each region. We take the weighted average over both regions so that a few outliers don’t have too much influence on the result.


### Example of regression2

How regression trees work:

- We split the input space (x-axis) using thresholds like `x > 6.5`
- These splits divide the training data into regions
- For each region, we calculate the **average of y-values**
- That average is the predicted value for any new input falling in that region

So at each leaf node, the model just outputs the **mean y** of its region


### Stopping Conditions for Regression


classification trees, we use **stopping conditions** to prevent overfitting.  
In general, we look for **accuracy gain** to decide whether a split is worth it.

This **accuracy gain** is essentially the same as what we previously called **impurity decrease**  (like Gini index reduction or entropy decrease).  
So when we talk about “gaining accuracy,” we’re really talking about **making the leaf nodes purer**, that is, reducing the uncertainty or mixing in the resulting regions.


### Regression Trees Prediction

1. Traverse the tree by following the split rules, until we reach a leaf node
2. Predict `ŷ` by calculating the average y-value of the training points in that leaf.

(I realized that what we’re doing here is pretty simple:  
We just split the input space based on x-values, and then for each region, we calculate the average y-value. That average becomes our prediction.  
So basically, we’re just grouping similar x’s together and predicting y by the average from the training data in that group.)