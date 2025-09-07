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
  - In extreme cases, the process may **diverge** completely.  

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

### A drawback in bagging method
