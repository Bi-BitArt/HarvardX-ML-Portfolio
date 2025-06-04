# Section 1: Decision Trees I


## Preparation knowledge

- **Exponential notation**:  
  For instance, the number 8,000,000 (eight million) can be expressed as `8e6`, `8 * 10^6`, or `8 × 10^6`.

## Key Concepts


### What is classification?

Classification is the task of predicting a categorical label (class) based on input features.  
One of the most basic classification models is **logistic regression**, which calculates the probability of belonging to a particular class.


### When does logistic regression work best?

- The classes are well-separated in the feature space  
- The classification boundary has a nice geometry

In other words, **linearly separable** in the feature space.  
That is a straight line (or hyperplane) can separate the categories effectively.


### What are **classification boundaries** or **decision boundaries**?

Classification boundaries are the points or regions in the feature space  
where the model is equally uncertain between two classes.

In other words:

- `P(Y = 1) = P(Y = 0)`
- Since `P(Y = 0) = 1 - P(Y = 1)`, this means `P(Y = 1) = 0.5`

Also can be expressed as:

- `Xβ = 0`  
  (`x` are the values of the features; longitude and latitude,  
  `β` is a vector of the parameters or the coefficients of the logistic model)

This equation can define the decision boundary, like:  
`Latitude (Y) = 0.8 * Longitude (X) + 0.1`

(I felt it's really similar to ordinary equations like: `y = 3x + 2`.)


### What is a decision tree?

A decision tree is a machine learning algorithm that can be used for both classification and regression.  
It is one of the supervised algorithms, characterized by a tree-like structure, where:

- Each internal node represents a feature or attribute  
- Each branch represents a decision  
- Each leaf node represents the outcome or prediction


### What is a motivation for employing Decision Trees?

The decision boundaries sometimes cannot easily be described by a single equation.  
So we wish for a model that:

- Allows for complex decision boundaries  
- Is also easy to interpret

This is why decision trees are useful. They can create models that:

- Are easy for people to understand, because the process looks like a flowchart  
- Can deal with complex data by dividing the space into smaller parts  
- Use simple rules like `X > threshold`, which can be written as short mathematical expressions

For example, a question like **height > 6.5?** in a fruit example is similar to **longitude > 6.5?** in a map. This shows that decision trees can split data using simple conditions based on features.

 By repeating this procedure, we can make even more complex decision boundaries.

(I thought decision trees are really handy because it can even define when each splitting should be done.)


### Classification Error

Can be defined as 

- `errors / total`.  
- `Number of minority class data points / Total number of data points`.

Also can be expressed as:

- `1 - Number of majority data points / Total number of data points`.


### In Symbolic Definition

We define the misclassification error for region R_r, split using predictor **p** and threshold **t_p**, as:

`Error(R_r | p, t_p) = 1 - max_k Ψ(k | R_r)`


### Meaning of each symbols:

- **R_r**: A region in feature space divided by a decision tree split  
- **p**: The predictor (feature) used for splitting  
- **t_p**: The threshold value used for that predictor  
- **Ψ(k | R_r)**: Proportion of data points in **R_r** that are labeled with class **k**  
- **max_k Ψ(k | R_r)**: Proportion of the majority class in **R_r**

(Come back here when I get lost in terms of meaning of each symbol.)


### Importance of Weighted Average

Sometimes one region has more data points than the other.  
If we treat both regions equally, **a region with only a few points could influence the decision too much**, which is misleading.

We use a weighted average of the classification error in both regions, so that:  
 - Larger regions (with more points) have more impact.
 - Smaller regions (with fewer points) have less impact. 


## Insights


## Reflections


