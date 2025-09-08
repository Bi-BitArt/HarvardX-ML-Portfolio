# AdaBoost Classifier from Scratch

### Goal

The goal of this exercise is to implement the AdaBoost algorithm from scratch using simple decision tree stumps as weak learners, and then compare the results with scikit-learn’s AdaBoostClassifier.
We will learn how AdaBoost iteratively updates sample weights, assigns higher importance to misclassified points, and combines weak learners into a strong classifier.

---
### [Sheet1] Importing Libraries

---
### [Sheet2] Loading the Dataset

---
### [Sheet3] Preparing the Response Variable

`y = np.where(y == 0, -1, 1)`

AdaBoost requires class labels to be −1 and +1.
This step transforms the dataset labels appropriately.

---
### Sheet4: Implementing AdaBoost from Scratch (with Stumps)

Goal: Build AdaBoost manually using decision tree stumps (depth=1) and understand each step: weighted training, weighted error, stump weight (λ), sample-weight update, normalization, and final weighted vote.

1. Initialize sample weights

`sample_weight = np.ones(N) / N`
`sample_weight_list.append(sample_weight.copy())`

Start with a uniform distribution over samples (each gets 1/N).

.copy() is important: we store a snapshot each round. Without it, later updates would overwrite the history you want to visualize.


2. Define the weak learner (stump)

`estimator = DecisionTreeClassifier(max_depth=1)`

A stump is a decision tree with a single split (2 leaves). This is the standard weak learner for AdaBoost.


3. Fit with current sample weights

`estimator.fit(X, y, sample_weight=sample_weight)`

Passing sample_weight tells the stump to pay more attention to currently “important” samples.


4. Predict & build misclassification vector

`y_predict = estimator.predict(X)
`incorrect = (y_predict != y)`

incorrect is a boolean mask (True for errors). It feeds the weighted error computation.


5. Weighted error (ε)

`estimator_error = np.average(incorrect, weights=sample_weight)`

This is the stump’s error under the current weights. Large if it misses heavily-weighted (hard) points.


6. Stump weight (λ)

`err = np.clip(estimator_error, eps, 1 - eps)`
`estimator_weight = 0.5 * np.log((1 - err) / err)`

The closed-form weight from minimizing exponential loss. Lower error → larger λ → stronger influence.

clip avoids log(0) when the stump is perfect or hopeless.


7. Update sample weights (unnormalized)

`sample_weight *= np.exp(-estimator_weight * y * y_predict)`

With labels in {-1, +1}:

Correct prediction: y*y_predict = +1 → weight multiplies by exp(-λ) → decrease.

Incorrect prediction: y*y_predict = -1 → weight multiplies by exp(+λ) → increase.


This is how the next stump focuses on hard points.


8. Normalize weights

`sample_weight /= sample_weight.sum()`

Keep them summing to 1 for stability and probabilistic interpretation.

9. Convert lists to arrays

`estimator_list = np.asarray(estimator_list)`

Convert all result lists into NumPy arrays for easier handling.

10. Final prediction (ensemble output)

`preds = np.array([np.sign((y_predict_list[:,point] * estimator_weight_list).sum()) for point in range(N)])`

Make the final ensemble prediction by taking a weighted majority vote (using λ) and applying np.sign.

---
### Sheet5: Testing AdaBoost Implementation

Goal: Evaluate the custom AdaBoost function by checking prediction accuracy.

Explanation:

`AdaBoost_scratch(X, y, M=9)` runs the boosting algorithm with 9 weak learners (stumps).

`preds` contains the final boosted predictions for all samples.

`(preds == y).mean()` computes the proportion of correct predictions, giving the overall accuracy.

---
### Sheet6: Visualizing AdaBoost Stumps

Goal: Visualize how each weak learner (stump) contributes to the ensemble.

fig = plt.figure(figsize = (16,16))
for m in range(0, 9):
    fig.add_subplot(3,3,m+1)
    s_weights = (sample_weight_list[m,:] / sample_weight_list[m,:].sum()) * 300
    plot_decision_boundary(estimator_list[m], X, y, N=50, 
                           scatter_weights=s_weights, counter=m)
    plt.tight_layout()

---
### Sheet7: Final AdaBoost Decision Boundary (Scikit-learn)

Goal: Use scikit-learn’s built-in implementation to view the final ensemble decision boundary.

`AdaBoostClassifier(
    estimator=DecisionTreeClassifier(max_depth=1),
    algorithm='SAMME',
    n_estimators=9)`

`boost.fit(X, y)`

Uses scikit-learn’s AdaBoostClassifier with stumps (max_depth=1) and 9 estimators.

Easier and more efficient than building AdaBoost from scratch, but conceptually equivalent to what we did manually.
