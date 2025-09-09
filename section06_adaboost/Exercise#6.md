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

- AdaBoost requires class labels to be −1 and +1.
This step transforms the dataset labels appropriately.

- Note: np.where(condition, A, B) works like a vectorized if-else:

    - If the condition is True → assign A.

    - If the condition is False → assign B.

Here, if y == 0, it becomes -1; otherwise, it becomes 1.

---
### Sheet4: Implementing AdaBoost from Scratch (with Stumps)

Goal: Build AdaBoost manually using decision tree stumps (depth=1) and understand each step: weighted training, weighted error, stump weight (λ), sample-weight update, normalization, and final weighted vote.


1. Initialize sample weights

`sample_weight = np.ones(N) / N`

Initialize all sample weights equally at 1/N.

This gives a uniform distribution over all samples.


`sample_weight_list.append(sample_weight.copy())`

Save a snapshot of the current weights into a list.

.copy() is necessary so future updates don’t overwrite this stored version.


2. Define the weak learner (stump)

`estimator = DecisionTreeClassifier(max_depth=1, random_state=0)`

A stump is a decision tree with a single split (2 leaves). This is the standard weak learner for AdaBoost.


3. Fit with current sample weights

`estimator.fit(X, y, sample_weight=sample_weight)`

Normally, fit(X, y) is enough: all samples are treated equally, as if each has the same weight (1/N).

By adding sample_weight, we can change how much influence each sample has on training.

In AdaBoost, this is crucial: misclassified samples from previous rounds are given higher weights, so the next stump pays more attention to them.


In short, sample_weight is what makes AdaBoost different from ordinary training: it focuses learning on the “hard” points.


4. Predict & build misclassification vector

`y_predict = estimator.predict(X)`

Uses the trained stump to predict labels for all training samples.

`incorrect = (y_predict != y)`

Creates a boolean mask:

- True if the sample was misclassified.

- False if the prediction was correct.


5. Weighted error (ε)

`estimator_error = np.average(incorrect, weights=sample_weight)`

- Plain error is simply the fraction of misclassified samples.

- Weighted error takes the current sample weights into account:

    - Misclassifying a low-weight sample contributes little to the error.

    - Misclassifying a high-weight sample contributes a lot to the error.

- In other words: 

    - Easy samples (low weight) matter less if they are misclassified.

    - Hard samples (high weight) matter much more, since the algorithm is forcing the model to focus on them.


This weighted error (ε) is then directly used to compute the stump’s weight (λ).

Small ε: stump is more reliable: gets a larger λ (in order to strengthen the influence of this stump).

Large ε: stump is less reliable: gets a smaller λ.


6. Stump weight (λ)

`eps = 1e-12`
Tiny constant used as a safeguard.
Prevents log(0) when error = 0 or 1.

`err = np.clip(estimator_error, eps, 1 - eps)`
Keeps error into [eps, 1 - eps] to keep it in a safe range.

`estimator_weight = 0.5 * np.log((1 - err) / err)`

The closed-form weight from minimizing exponential loss. Lower error = larger λ = stronger influence.

- Intuition:

    - A stump with low error gets a large λ, so it contributes strongly to the ensemble.

    - A stump with high error gets a small (or even negative) λ, so it has little influence.

This formula ensures that good stumps have stronger voices in the final weighted vote, while poor stumps are down-weighted.


7. Update sample weights (unnormalized)

`sample_weight *= np.exp(-estimator_weight * y * y_predict)`

With labels in {-1, +1}:

Correct prediction: y * y_predict = +1: weight decreases (× exp(-λ)).

Incorrect prediction: y * y_predict = -1: weight increases (× exp(+λ)).

In short:

- Easy / correctly classified points lose weight → model pays less attention next time.

- Hard / misclassified points gain weight → model focuses more on them in the next iteration.

- This shifting of attention from easy to hard samples is the core mechanism of AdaBoost, making each new stump complement the weaknesses of the previous ones.

8. Normalize weights

`sample_weight /= sample_weight.sum()`

Keep them summing to 1 for stability and probabilistic interpretation.


9. Convert lists to arrays

`estimator_list = np.asarray(estimator_list)`

Convert all result lists into NumPy arrays for easier handling.

10. Final prediction (ensemble output)

`preds = np.array([np.sign((y_predict_list[:,point] * estimator_weight_list).sum()) for point in range(N)])`

For each sample:

1. Collect the predictions from all stumps for that sample.
   
2. Give each prediction a weight (λ) according to how good the
   stump is.
   
3. Add up all these weighted predictions.

np.sign(...) is then applied:

Positive sum: final prediction = +1

Negative sum: final prediction = −1

Summary:
The final prediction is a weighted majority vote of all stumps, where stronger stumps (low error, high λ) count more, and weaker stumps count less.

---
### Sheet5: Testing AdaBoost Implementation

Goal: Evaluate the custom AdaBoost function by checking prediction accuracy.

Explanation:

`AdaBoost_scratch(X, y, M=9)` 
runs the boosting algorithm with 9 weak learners (stumps).

`preds` contains the final boosted predictions for all samples.

`accuracy = np.round((preds == y).mean(), 3)` 
computes the proportion of correct predictions, giving the overall accuracy.

This single line actually performs three steps at once:

1. (preds == y) → creates a Boolean array (True for correct predictions, False otherwise).

2. .mean() → converts that Boolean array into a numeric average (accuracy as a fraction).

3. np.round(..., 3) → rounds the result to 3 decimal places.

Can be expanded in these three lines:

- correct = (preds == y)
- accuracy_raw = correct.mean()
- accuracy = np.round(accuracy_raw, 3)

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

---
### Results & Insight

#### Results

- Classes: The model successfully mapped the labels into the required form {−1, +1}.

- Predictions: The ensemble predictions also fall strictly in {−1, +1}, with no zeros, confirming correct sign handling.

- Sample size: Dataset contains 91 samples with 2 features (latitude, longitude).

- Accuracy: Raw accuracy = 0.956 (=95.6%).

---

#### Insight

The AdaBoost implementation from scratch is working as intended:

- Predictions align with the expected label space (−1, +1).

- No invalid or missing predictions occurred.

- Even with simple stumps as weak learners, AdaBoost achieved approximately 95% accuracy, proving how reweighting and combining weak models can create a powerful ensemble classifier.
