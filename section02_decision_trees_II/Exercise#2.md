# Exercise Decision Tree from Scratch

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

`first_split_df = tree_df[tree_df["x2"] <= x2_threshold]`

- Boolean mask: `df["x2"] <= x2_threshold` creates a Boolean array where each row is marked True if its x2 value is less than or equal to the chosen threshold, and False otherwise. By placing this mask, we can extract only those rows where the condition is True.

---
### Sheet36 Splitting the Left Node of the 2nd Split

`second_split_left_df = first_split_df[first_split_df["x1"] <= x1_threshold_2split]`

This keeps only rows in first_split_df where x1 is ≤ the second-split threshold.

---
## Insights

Why do we only use the **left child** for the next split?

After the first split, the dataset is divided into two regions:

Left child: points that satisfy the split condition (<= threshold)

Right child: points that do not satisfy the condition (> threshold)


When we want to calculate the second threshold, we must look only inside the region we are further splitting.
If we use the entire dataset again, we would mix irrelevant points and end up obtaining an empty region after the split.
