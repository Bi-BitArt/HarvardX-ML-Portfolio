# Section 1: Decision Trees I


## Preparation knowledge

- **Exponential notation**:  
  For instance, the number 8,000,000 (eight million) can be expressed as `8e6`, `8 * 10^6`, or `8 × 10^6`.

## Key Concepts from Video 1 and 2


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


### 1. Classification Error

Can be defined as 

- `errors / total`.  
- `Number of minority class data points / Total number of data points`.

Also can be expressed as:

- `1 - Number of majority data points / Total number of data points`.


### In Symbolic Definition

We define the classification error for region R_r, split using predictor **p** and threshold **t_p**, as:

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


### 2. Gini Index

We measure Gini Index to assess the quality of the split.  
Gini Index is calculated for each region as:

`Gini(R_r | p, t_p) = 1 - Σ Ψ(k | R_r)^2`

Where:

- `Ψ(k | R_r)` is the proportion of points in region `R_r` that are labeled with class `k`
- The sum `Σ` for all possible classes

(In short, the equation means:
First, calculate the proportion of items that belong to each class in the region.
Then, square those proportions and add them up.
Finally, subtract that sum from 1, the Gini Index is calculated.)


### 3. Information Theory


- **Entropy**

There are two forms of entropy:

#### 1. General Entropy (from Information Theory)

This is the basic definition of entropy for any random variable:

`H(x) = - Σ Ψ(x) * log₂ Ψ(x)`

#### 2. Entropy in Decision Trees

This is used to measure how pure/mixed the class distribution is **within a region `R_r`** after splitting:

`Entropy(R_r | p, t_p) = - Σ Ψ(k | R_r) * log₂ Ψ(k | R_r)`

For example, assuming there are eight triangles and five circles in the region, the entropy is calculated like:

- Total points = 13  
- Proportion of triangles = 8/13  
- Proportion of circles = 5/13

`Entropy = - [ (8/13) * log₂(8/13) + (5/13) * log₂(5/13) ]`


(Overall, there are three metrics for evaluating the quality of a split; **Classification Error**, **Gini Index**, **Information theory and the entropy**.)

(I wondered why we try to minimize the weighted average of the Gini Index or entropy. If the groups after the split are very mixed, it’s harder to classify them. So we want to choose the feature and cut that give us the lowest Gini index or entropy, like making each group mostly contain one type, so that prediction becomes easier.)

### Greedy Algorithm

We use a **greedy algorithm** to build the 'optimal' decision tree.

1. Start with an empty decision tree (undivided feature space).

2. Choose the 'optimal' predictor on which to split and choose the 'optimal' threshold value for splitting.

3. Recurse on each new node until the **stopping condition** is met.

4. For the case of classification, predict each region to have a class label  
   based on the largest class of the training points in that region (majority class).

(Basically, we use the Gini Index and the entropy because the Classification Error is too simple to use.)

(I read that greedy mechanism means "always picking the best-looking option at each step".  
Honestly, I don’t fully get what that means clearly.  
But I guess we don’t try to plan everything ahead; we just split where it looks best now.  
Maybe I’ll understand it better once I try to build a tree myself.)

## Key Concepts from Video 3


### What are Stopping Conditions?

In order to prevent **overfitting** (this leads to jeopardize the versatility of the model), we use Stopping Conditions.  

Common examples of stopping conditions below; 

 - **maximum depth** (max_depth)  
If `max_depth = 1` , it allows for only one split.

 - Don't sprit a region if all instances in the region **belong to the same class**.  
In other words, there is no gain to split pure leaf nodes.

 - Don't sprit a region if the **number of instances in any of the sub-regions will fall below pre-defined threshold** (min_samples_leaf).  
If `min_samples_leaf = 4` , it doesn't allow a split which will result in regions with less than four instances.

 - Don't split a region if the total **number of laves** in the tree will exceed a pre-defined threshold (max_leaf_nodes).  
In other words, we do not split that will exceed the maximum leaf node.

### **Level-order** and **Best-first**

Normally, **Sklearn** (a.k.a scikikt-learn, a popular machine learning library for python) grows trees in what is called **'level-order'** fashion until a stopping condition is met (satisfied). 

- What is **level-order**  
According to this rule, all the leaf nodes will grow simlutaneously; in other words, the tree expands one level at a time.
It helps to make a well-balanced, easy-to-understand tree.

- What is **Best-first**
Sklearn determines the best split based on **impurity decrease**. The resulting tree will be the same when fully grown, though there will be a difference of the order in which the tree is built.

For example, suppose there are two regions; 
the one on the right has a higher *impurity decrease*, and the one on the left has a lower *impurity decrease*.  
In **level-order**, both might be split at the same time.  
But in **best-first**, the left one (with higher gain) will be split first.

To put it simply, we prioritize a split by which we can have **purer** leaf nodes.





## Insights


## Reflections


