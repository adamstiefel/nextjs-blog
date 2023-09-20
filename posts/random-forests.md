---
title: "Machine Learning - Random Forests"
date: "2023-09-20"
---

Random forests are a forest of decision trees or a whole bunch of trees.

Start with data and then build a decision tree from there. The problem is that decision trees generally tend to overfit data. It turns out that when data is changed slightly, it changes the tree significantly. Decision trees are a very sensitive or unstable learning technique. Use random forest when you want better accuracy and do not want to take the time to interpret the model.

Tree to Forest

As we previously established, decision trees are sensitive to even minuscule changes in data - we call this unstable. What if we get a bunch of trees to work together to produce a better prediction engine? When making predictions, we utilize the average for regression trees and the mode for classification trees. Decision trees tend to overfit the data, and averaging them corrects this problem. The random forest selects rows and features at random to build various decision trees and then takes the average of all trees to make predictions.

Start with data, then grow a bunch of trees with different splits from that data. With all of these trees, you may get a more stable prediction. Creating the forest with care results in a more stable prediction. Each tree makes its prediction then you take the average to create one prediction.

General ideas

Using multiple models, trees in the case, to get better predictions is what we call ensemble learning. Bagging, which stands for bootstrap aggregating, breaks down the original model into small training components. Bagging components are typically of the same size of data and follow the method of sampling with replacement.

Each tree in the forest has to be slightly different from one another to provide the benefit of not using one tree. This is where bagging comes into play to help us achieve the benefit of forests. Use bagging to create the samples and then use those samples to train different trees. Each different training data set gets a sample of rows with replacements from the original data set. Sampling with replacement means that some rows can be repeated in multiple training data sets.

Bagging

Start with the original data set. Each new data set takes a random sample of the original data set. One row can be repeated in the same new data set and other new data sets. Then we build a decision tree from each of those new data sets.

How do we build the decision trees for each new data set?

We start with a new data set. Then we start to build the tree. The first step is to build the root node. We consider all splits that are possible. We consider the number of columns also known as independent variables. Since we want the decision trees to be very diverse the tree does not choose the best split between all independent variables. Rather it is given a random subset of independent variables and chooses the best split amongst said independent variables. This ensures that each of the decision trees is slightly different from one another.

Random forests summary

Start with random sampling with replacement - AKA bagging. Then with each subset, we build a decision tree. The decision tree is built randomly picking independent variables for each node's branching. We do not prune (pruning is a way of reducing overfitting). We get a prediction for each tree. Then we combine the predictions of each tree to get one prediction. For regression, we use mean and mode for classification.

Pitfalls

We need to ensure that the number of randomly picked independent variables is less than the total number of independent variables in the original data set. If the number of randomly picked independent variables is the same then there will be a high tree correlation. This will lead to random forests not making better predictions. A small number of randomly picked independent variables will lead to weak predicting trees. Since you decrease your chances of selecting influential independent variables. Weak trees mean that error rates are high. Both of those extremes are very bad. You want the number of randomly picked independent variables somewhere in between. There is a lot of forgiving in the good range - so just try to avoid the extremely high or low.

  