# Section 1: Decision Trees I

## Preface

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


### What are **classification boundaries** ot **decision boundaries**?

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


### Classification Error

Can be defined as `errors / total`
also as `Number of minority class data points / Total number of data points`

## Insight


## Reflections


