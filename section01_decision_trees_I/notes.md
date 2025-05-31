 Section 1: Decision Trees I

## Preface
- **Exponential notation**: For instance, the number 8,000,000 (eight million) can be expressed as `8e6` or `8 * 10^6` or `8 × 10^6`.

## Concept Breakdown

- What is a decision tree?

  A decision tree is a machine learning algorithm that can be used for both classification and regression.  
  It is one of the supervised algorithms, characterized by a tree-like structure, where:

  - Each internal node represents a feature or attribute.
  - Each branch represents a decision.
  - Each leaf node represents the outcome or prediction.

- What is classification?

  Classification is the task of predicting a categorical label (class) based on input features.  

  One of the most basic classification models is **logistic regression**, which calculates the probability of belonging to a particular class.

- When does logistic regression work best?

  - The classes are well-separated in the feature space
  - The classification boundary has a nice geometry

  In other words, **linearly separable** in the feature space. That is, a straight line (or hyperplane) can separate the categories effectively.

- What are **classification boundaries** (also known as **decision boundaries**)?

  Classification boundaries are the points or regions in the feature space where the model is equally uncertain between two classes.  

  In other words:

  - P(Y = 1) = P(Y = 0)
  - Since P(Y = 0) = 1 - P(Y = 1), this means P(Y = 1) = 0.5


## Key Concepts


## Pseudocode Insight


## Reflections


