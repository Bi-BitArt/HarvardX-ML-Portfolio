# Section 2: Decision Trees II


## Preparation knowledge

- **Regression**

Decision trees are often utilized for classification tasks. However, they can also serve as a tool for regression - that is, **predicting** a quantitative outcome.

(At first, I imagined regression as "going back" through the decision tree,  
like using feature parameters to trace the identity of something.  
But I realized that regression still works by going forward - 
the tree uses those parameters to split the input space and reach a numerical output at the end.)


## Key Concepts from Video 1


### Example of regression


Suppose there are many features, in Euclid space/
If one of these features has a x value > 6.5, it is anticipated to have y value 0.697, since this y value is the average of...
If x is less than 6.5, 