# Section 2: Decision Trees II


## Preparation knowledge

**Regression**

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


In classification trees, we use **stopping conditions** to prevent overfitting.  
In general, we look for **accuracy gain** to decide whether a split is worth it.

This **accuracy gain** is essentially the same as what we previously called **impurity decrease**  (like Gini index reduction or entropy decrease).  
So when we talk about “gaining accuracy,” we’re really talking about **making the leaf nodes purer**, that is, reducing the uncertainty or mixing in the resulting regions.


### Regression Trees Prediction

1. Traverse the tree by following the split rules, until we reach a leaf node
2. Predict `ŷ` by calculating the average y-value of the training points in that leaf.

(I realized that what we’re doing here is pretty simple:  
We just split the input space based on x-values, and then for each region, we calculate the average y-value. That average becomes our prediction.  
So basically, we’re just grouping similar x’s together and predicting y by the average from the training data in that group.)


### Numerical attributes

**Encoding** the values of the categorical features using numbers makes it possible to treat **categories** in the **decision tree** .

(They insisted that we can arrange the split by labeling differently.  
At first I wondered if I was missing something important here,  
but I think the message is pretty straightforward:  
if you encode categories with different numbers, the tree will split them differently.  
It makes sense mechanically, but I don’t think it’s deep or surprising.  
Maybe it’s just a reminder that label encoding isn’t neutral: it can introduce some bias.)


### One-Hot Encoding vs Dummy Encoding

1. **One-Hot Encoding**: A method where each category becomes a separate column, where values are either 0 or 1. This approach is often considered redundant in linear models.

2. **Dummy Encoding**: A method where one category is dropped, where the remaining columns represent comparisons to the reference.

Dummy Encoding is often regarded as a superior method in that it avoids multicollinearity (feature redundancy that can confuse the model) in linear models and eliminates unnecessary columns.


## Key Concepts from Video 2


### Issue with Using Stopping Conditions

The major issue with pre-specifying a stopping condition is that we may stop too early or stop too late. However, lecturer also mentioned that we can fix this issue by choosing several stopping criteria.


### Alternatives to Using Stopping Conditions: Pruning 

 - In this method, we aim to reduce the number of leaves. For example, when it comes to evaluating applicants, we can ignore or prune a condition (represented as a leaf) which we don't think is indicative of a successful applicant.

 - Motivation for Prunning: as we make our tree deeper, model complexity increases, lowering bias and increasing variance. Pruning helps us reduce model complexity whereas keeping prediction error low, rather than preventing a complex tree from growing.


### Cost Complexity Pruning

One common example of pruning.

Can be expressed as: `C(T) = Error(T) + α |T|` (Also known as cost complexity score)

- **C(T)**: Cost of the tree `T`
- **Error(T)**: Training error (classification or regression error) of the tree `T`
- **|T|**: The number of terminal nodes (leaf nodes) in the tree
- **α (alpha)**: Regularization parameter that controls the trade-off between tree complexity and training error
- **Penalty term**: `α × |T|`

A big tree (with many leaves) usually fits training data very well=low error. 
But it can overfit: perform poorly on new data. 
So we add a penalty for being too complex: Penalty term

Overall, we try to adjust these parameters in order to minimize `C(T)`.
By using cross-validation, we can find the best value of alpha.  But the selection should be based on the error on the validation set: not on the Cost Complexity Value (C(T)), because C(T) includes a penalty term that is only useful for pruning, which is irrelevant to judging actual performance.


### How do we actually choose which leaves to prune and set alpha?

Suppose we have **a full tree `T0`.** Plus, we call pruned TO tree as `T1`. 

In order to choose the best leave to prune, we will choose the one that maximizes the difference of **cost complexity score** (`C(T) = Error(T) + α |T|`). 

In other words, our goal is to maximize C(T0)-C(T1) by minimizing the ratio of the difference of the errors **over the differences in complexity.** Explained below: 

### Efficient Pruning Using Error-Complexity Ratio

To decide which branches to prune, we use the following ratio:

[E(T₀) - E(T₁)] / [α|T₀| - α|T₁|]

- **Numerator**: Increase in error caused by pruning  
- **Denominator**: Decrease in tree complexity (number of leaves removed)  

By **minimizing this ratio**, we aim to reduce model complexity efficiently with minimal loss in accuracy.  
This method allows us to prune leaves that give the **most "simplicity gain" per unit error increase**, leading to the higher adaptability.

We continue this pruning process to gain a sequencing pruned trees:  
**T2, T3, ..., TL**, TL is containing only the root of `T0`.

For each value of **α**, we select the best tree `T*` (among the sequence) using **cross-validation**.  
This is the best pruned tree *given α*.  

Finally, we also use **cross-validation to choose the optimal α**, based on validation performance.  
So, the final output is the tree `T*` that achieves the best performance under the best α.

(In short, we choose T*, which is the best one given α by cross-validation. And then, we choose the best alpha by again using cross-validation.)

# Exercise #1 Decision Tree from Scratch

The goal of this exercise is to better understand the inner workings of a Decision Tree and how the boundaries are computed.

---
### Sheet4 Implementing Gini Calculation for Candidate Splits

- Why weighted? Because larger groups should influence impurity more strongly.

- Why max(1e-5, …)? To avoid division by zero if a node ends up empty.

- Outcome: For each candidate split value, we get a score. The best split is the one with the lowest total_gini.

---
### Sheet5 Extracting Unique Threshold Values from Predictors

- Get unique values of x1
`x1_unique = np.sort(tree_df["x1"].unique())`

Note: .unique() alone keeps the order of appearance.
For grading, values must be sorted, so we always use np.sort().

---
### Sheet7 Evaluating Total Gini for Predictors

- Applying `get_total_gini` to compute impurity scores for x1 and x2, preparing to select the best split.

---
### Sheet11 Getting the Best Threshlold for Root Split

1. Find lowest impurity index
`np.argmin(gini_scores)`: index of the candidate split with the minimum Gini impurity.

2. Compute threshold
`threshold = (unique_values[idx] + unique_values[idx+1]) / 2`: Take the midpoint between unique_values[idx] and unique_values[idx+1].

3. Store the result
Save the result as `x2_threshold`. This value will be the actual splitting rule for the root node.

---
### Sheet15 Splitting Data into the Left Leaf

`first_split_df = df[df["x2"] <= x2_threshold]`

- Boolean mask: `df["x2"] <= x2_threshold` creates a Boolean array where each row is marked True if its x2 value is less than or equal to the chosen threshold, and False otherwise. By placing this mask, we can extract only those rows where the condition is True.

---
### Sheet36 Splitting the Left Node of the 2nd Split

`second_split_left_df = first_split_df[first_split_df["x1"] <= x1_threshold_2split]`

Creates a new DataFrame, `second_split_left_df`, which contains only those rows from `first_split_df` where x1 is less than or equal to the threshold chosen for the second split.

---
## Insights

Why do we only use the "left child" for the next split?

After the first split, the dataset is divided into two regions:

Left child: points that satisfy the split condition (<= threshold)

Right child: points that do not satisfy the condition (> threshold)


When we want to calculate the second threshold, we must look only inside the region we are further splitting.
If we use the entire dataset again, we would mix irrelevant points and could even end up obtaining an empty region after the split.
