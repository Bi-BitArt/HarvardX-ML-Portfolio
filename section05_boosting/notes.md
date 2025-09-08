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

Suppose there are three simple, weak models (Each model has an accuracy of 60% / 64% / 70% ), we want to create a batter model by combining them appropriately.


## Key Concepts from Video 2

### Recap: Boosting

The key idea of boosting is to linearly combine simple trees that we call `Th` into a strong learner, which we call `T`.

#### Gradient Boosting

- Gradient boositng is a method for **iteratively**(=repetitive) building a complex model `T` by adding simple models.
- Each new simple model added to the ensemble compenstes for the weakness of the current ensemble.

For example, we use the weak learner, which will be a tree with only one split chosen to minimize the MSE. In order to learn from mistakes, **Residuals** matters. It represents the difference between **the actual y values** of our points and **the predicted y values** of our trees. 

#### Gradient Boosting: Step-by-Step

**Step 1. Fit an initial simple model \(T^{(0)}\)**
- Train a weak learner (e.g., a shallow decision tree with one split) on the training data.  
- This serves as the starting point of the ensemble.  

**Step 2. Compute the residuals**
- Calculate the difference between the actual values \(y\) and the predicted values \(\hat{y}\) from the current model.  
- Residuals = errors made by the current model, indicating where it performs poorly.  

**Step 3. Fit a new model to the residuals**
- Train another weak learner using the residuals as the new target.  
- This model focuses specifically on the parts the previous model got wrong.  

**Step 4. Update the ensemble model**
- Add the new weak learner into the ensemble with a weight controlled by the **learning rate \(\lambda\)**:  
  \[
  T \leftarrow T + \lambda T^{(i)}
  \]  
- The learning rate ensures that each update is gradual and stable.  

**Step 5. Recompute residuals**
- With the updated model, calculate the new residuals.  
- Repeat Steps 3–5 until the stopping criterion (e.g., number of trees, convergence) is met.  

---

#### Gradient Boosting: Additive Model

In Gradient Boosting, the **current model** at any step is the sum of all weak learners built so far.  
Each new weak learner is trained on the residuals and then added to the ensemble with a weight controlled by the learning rate (λ).

Formally:

- Start with the initial weak learner 

`F₀(x) = T₀(x)`

- At iteration 1

`F₁(x) = F₀(x) + λ T₁(x)`

- At iteration 2

`F₂(x) = F₁(x) + λ T₂(x)`

...

- Until iteration m

`Fm(x) = F(m-1)(x) + λ Tm(x)`

---

### Key Idea
- The **current model** = all the weak learners added so far.  
- Each new weak learner is a small correction, trained to reduce the errors (residuals) of the current model.  
- By repeatedly adding weak learners, the ensemble gradually improves, turning many weak models into a strong one.

#### Summary of Gradient Boosting
- Gradient Boosting builds a **strong model** by **iteratively adding weak learners**.  
- Each new learner is trained on the **residuals**, compensating for the errors of the current ensemble.  
- The **learning rate (λ)** controls how much influence each weak learner has, balancing speed and stability.  
- Compared to Bagging/Random Forests (which reduce variance by averaging complex trees),  
  **Boosting reduces bias by combining many simple trees step by step**.


## Key Concepts from Video 3

###
