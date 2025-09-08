# Section 6: AdaBoost (Also known as adaptive boosting)

### Introduction to AdaBoost

- Idea #1: Build a stronger model step by step by adding many weak learners together. In the case of trees, a weak learner is just a very simple tree (a stump) with only one split and two leaves.
- Idea #2: Each new stump focuses on fixing the mistakes made by the current model. This is achieved by giving more importance (higher weights) to the observations that were previously misclassified.

---
### A brief example of AdaBoost

[Step 1: Fitting the First Stump]

1. Consider the following data set with two classes: orange circles represent the class number 1. And blue triangles represent the class number 2.
2. Fit a stump, S0, on the data set. The decision boundary chosen to maximize the overall purity will be at x1 = 4.6.
3. The region on the right of the split has two orange and four blue: so the majority is blue. On the left, orange is the majority: so the prediction in this region will be orange.

[Step 2: Initial Weights and Error Calculation]

1. Assume we initialize each data point to have equal weight of 1/N.
2. Calculate the total error of the stump S0 using the weighted error formula.
3. Since all weights are equal at the start, this simply means the error rate is the fraction of misclassified points.

[Step 3: Assigning a Weight to the Stump]

1. Assign the stump a weight (λ⁽⁰⁾), called the scaling factor.
2. This λ indicates how much the stump contributes to the entire ensemble.
3. A stump with lower error gets a higher λ, so its “vote” in the final model carries more weight.
4. Intuitively, good stumps have a strong influence, while weaker stumps have less influence.

Note:
Don’t confuse this λ with the learning rate λ in Gradient Boosting.
In AdaBoost, λ is the stump’s weight, derived from its error rate, and reflects its contribution to the ensemble.
In Gradient Boosting, λ (learning rate) is a user-chosen parameter that scales how much each new weak learner influences the model update.

[Step 4: Constructing the Ensemble Model]

1. From the previous step, we computed a weight (λ) for each stump, representing how reliable it is.
2. Now, we update the ensemble model by combining the stumps using these weights. Stronger stumps (with lower error) have more influence, while weaker stumps contribute less.
3. As we add more stumps, the model improves by making a weighted combination of the weak learners, rather than a simple majority vote.

[Step 5: Reweighting step]

1. After adding a stump to the ensemble, we update the weights of the training samples.
2. Misclassified samples: Their weights are increased, so that the next stump pays more attention to them.
3. Correctly classified samples: Their weights are decreased, since they are already handled well by the current model.
4. This ensures that each new stump focuses on the “harder” points that the ensemble has not yet captured correctly.

The weight update is given by:

$$
w_n^{(1)} \=\ \frac{w_n^{(0)} \, e^{-\lambda^{(0)} y_n T^{(0)}(x_n)}}{Z}
$$

**Explanation of Terms**

$$
w_n^{(0)} : \text{ previous weight of sample } (n)
$$

$$
y_n : \text{ true label of sample } (-1 \text{ or } 1)
$$

$$
T^{(0)}(x_n) : \text{ prediction from the current stump } (-1 \text{ or } 1)
$$

$$
\lambda^{(0)} : \text{ weight (confidence) of the stump}
$$

$$
Z : \text{ normalization factor ensuring weights sum to 1}
$$

[Step 6: Creating a New Stump]

1. Using the re-weighted data from Step 5, we train a new stump .
2. Because the weights of the misclassified points were increased, this new stump pays more attention to those difficult points.
3. As a result,  is more likely to correctly classify the data points that the previous stump  got wrong.

[Step 7: Recalculate the Error with New Weights]

1. With the updated weights, we calculate the total error of the new stump .
2. The error is computed as the weighted fraction of misclassified points (points that  predicts incorrectly relative to their assigned weights).
3. This weighted error now reflects not just the number of mistakes, but their importance (since “harder” points carry more weight).

[Step 8: Assigning a Weight to the New Stump]

1. For the new stump created in Step 6, compute its weight (λ⁽¹⁾) based on the error calculated in Step 7.
2. This weight determines how much the new stump contributes to the entire ensemble.
3. A lower error gives a higher λ, meaning the stump’s “vote” carries more influence in the final model.
4. In the example, the second stump receives a weight λ⁽¹⁾ = 0.20.

[Step 9: Updating the Ensemble Model]

1. Combine the new stump with the existing ensemble using their respective weights.
2. The ensemble model now includes both stumps:
- The first stump contributes with weight 0.25.
- The second stump contributes with weight 0.20.
3. The final prediction is a weighted combination of the two stumps, rather than a simple majority vote.
4. As we add more stumps, the ensemble continues to improve its accuracy by emphasizing stronger learners and down-weighting weaker ones.

---
### AdaBoost: Summary

Given n training points, set w=1/n.
For i = 0 ... until stopping condition is met:

- Train a weak learner S⁽⁰⁾ using weights wₙ⁽⁰⁾.
- Calculate the total error, ε⁽⁰⁾, of the weak learner.
- Calculate the importance of each model, λ⁽⁰⁾.
- Combine all the stumps into the new ensemble model, T⁽⁰⁾.
- Adjust the weights assigned to each data point to ensure the next stump focuses on the points misclassified by the previous stump.

- How do I create a stump?
In AdaBoost, the stumps are created using simple decision trees with max_depth = 1.

- How do I use normalized weights to make the stump?
There are two options:
A) Create a new dataset of the same size as the original dataset with repetition based on the newly updated sample weight.
B) Use a weighted version of the Gini index.

---
### Loss Function in AdaBoost

In regression with Gradient Boosting, we usually minimize the Mean Squared Error (MSE).  
However, in AdaBoost we minimize a different function called **exponential loss**.

1. **Labels**  
   - In AdaBoost, the class labels are represented as -1 and +1.  
   - This makes the formulation simpler and naturally leads to the exponential form.

2. **Misclassified samples**  
   - When a sample is misclassified, its penalty becomes much larger.  
   - This means errors are emphasized strongly.

3. **Correctly classified samples**  
   - When a sample is correctly classified, its penalty becomes very small.  
   - These points are almost ignored in the loss calculation.

4. **Intuition**  
   - As a result, AdaBoost pays special attention to the mistakes made by the model.  
   - The algorithm keeps adjusting to focus more on the harder points.

**Takeaway:**
AdaBoost minimizes exponential loss, which **heavily penalizes mistakes**.  
This is why AdaBoost naturally shifts its focus toward the samples that are hardest to classify correctly.

---
### Choosing a Learning Rate

In AdaBoost, unlike gradient boosting for regression, the weight (λ) of each stump is not chosen manually.  
Instead, it is computed analytically by minimizing the **Exponential Loss**.

1. **Automatic calculation**  
   - The algorithm derives the optimal λ for each stump directly from the error rate.  
   - This avoids manual tuning of a learning rate, unlike Gradient Boosting.

2. **Error-based weight**  
   - If a stump has lower error, it receives a higher λ.  
   - This means its prediction carries more influence in the ensemble.  
   - Conversely, stumps with higher error get smaller λ and have less impact.

3. **Direct optimization**  
   - By solving the Exponential Loss function, we can find λ in closed form.  
   - This makes AdaBoost efficient and theoretically grounded.

**Takeaway:**
AdaBoost does not need a manually set learning rate like Gradient Boosting.  
The “learning rate” of each stump (λ) naturally comes out of the optimization of Exponential Loss, making the process adaptive and data-driven.

---
### Exponential Loss and Gradient Descent

- AdaBoost can be seen as doing **gradient descent on exponential loss**.  
- Misclassified points get higher weights, while correctly classified ones get lower weights.  
- Reweighting in AdaBoost is therefore equivalent to gradient descent in function space.  

**Takeaway:**
AdaBoost is not just a heuristic — it is an optimization method, closely related to Gradient Boosting.

---
### Gradient Descent with Exponential Loss

- The update step in gradient descent is written as updating the prediction with a term that involves the learning rate, the weight of the sample, and its true label.  
- Similar to gradient boosting, we approximate the gradient with a weak learner that depends on the input features.  
- This means training each weak learner on a re-weighted set of target values.
- In short, gradient descent with exponential loss means iteratively training weak learners that focus on the points misclassified by the previous models.

---
### Comparison: Gradient Boosting vs AdaBoost

- **Gradient Boosting**
  - Minimizes the **Mean Squared Error (MSE)**.
  - The gradient is based on residuals (difference between actual and predicted values).
  - Update step adjusts predictions directly using residuals.

- **AdaBoost**
  - Minimizes the **Exponential Loss**.
  - The gradient is re-expressed as weights on samples.
  - Misclassified points get larger weights, so the next learner focuses more on them.
  - Update step adjusts predictions using both sample weights and true labels.

#### Key Contrast
- Gradient Boosting focuses on **residuals**.
- AdaBoost focuses on **sample weights**.

---
### Final Sumary on Boosting

- **Stopping condition**: Same as Gradient Boosting. Stop when maximum iterations (number of stumps), minimum improvement in loss, or no further progress in iterations.  
- **Overfitting**: Unlike bagging/Random Forest, boosting methods (like AdaBoost) can overfit if run for too many iterations. Regularization methods exist but are not covered here.  
- **Hyper-parameters**: All mainly tied to the stump (weak learner) and the stopping conditions above.

There are a few implementations of boosting:
- **XGBoost**: an efficient gradient boosting decision.
- **LGBM**: Light gradient boosted machines, a library for training GBMs, also competes with XGBoost. 
