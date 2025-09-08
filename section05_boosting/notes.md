# Section 5: Boosting

## Preparation knowledge

###  The Role of Learning Rate (λ)

#### 1. Gradient Descent as an Example

- Here, `λ` is the **learning rate**, which determines the size of each step toward the minimum.  
- The red zig-zag lines in the plots represent the trajectory of these updates as the algorithm searches for the minimum.

#### 2. Two Key Cases
- **λ too small**  
  - Steps are very tiny.  
  - The algorithm safely converges but requires **many iterations** (very slow).  
- **λ too large**  
  - Steps overshoot the minimum.  
  - The trajectory “bounces” back and forth, sometimes failing to settle.    

#### 3. Why It Matters for Boosting
- In Boosting (e.g., AdaBoost, Gradient Boosting), we also apply a **learning rate (shrinkage parameter)**.  
- Instead of moving parameters directly, we add weak learners step by step:  
  - **Small λ** → gradual improvements, more stability, often better generalization.  
  - **Large λ** → faster improvements, but risk of instability or overfitting.  

**Summary**  
- The learning rate \( \lambda \) controls **how aggressively the model updates**.  
- Too small = safe but slow.  
- Too large = unstable, possibly divergent.  
- This balance is crucial both in gradient descent and in Boosting methods.

## Key Concepts from Video 1

### Recap: Decision Tree Issues

- Shallow trees: suffer from high bias and do not train well.
- Deep trees: have low bias but suffer from high variance leading to very low generalization error.
- As the tree grows: the model complexity increases, the bias goes down, and the variance goes up.

### Random Forests Issues

- Variance: Although variance reduction is better RF than bagging, the generalization error is still high.
- Speed: Large nunber of trees can make the algorithm very slow and ineffective for real-time predictions.

### Motivation for Boosting

Could we address the drawbacks of singlt decision trees models in some other way?

A solution to this problem is making an expressive model from simple trees, is another class of ensemble method called **boosting**.

- Option no.1: reducing the variance by aggregating a number off complex trees, and therefore, reducing model complexity.
- Option no.2: starting from simple shallow trees and trying to increase the overall complexity of a model by combining trees in a smart way, going from left to right on the plot.

- In bagging and random forest, we overfit and try to reduce the variance by aggregating (Averaging the predictions by majority voting).
- In boosting, we underfit and trying to increase the complexity while reducing the bias of the shallow trees (Aggregating underfitting trees to decrease the bias).

### How does Boosting work?

Suppose there are three simple, weak models (Each model has an accuracy of 60% / 64% / 70% ), we want to create a batter model by combining them appropriately.


