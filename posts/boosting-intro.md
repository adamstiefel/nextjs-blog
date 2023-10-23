---
title: "Machine Learning - Intro to Boosting"
date: "2022-09-25"
---

Boosting is a machine learning technique that aims to improve the accuracy of a model. It achieves this by combing "weak" models to create one "strong" model. It is like assembling a dream team of average players with distinct skill sets. Together, they perform better than any one person could.

How is boosting different from bagging? (another ensemble method)

Boosting starts with the dataset and creates one weak learner. Let's call it M1. It is a weak learner. Meaning that it does prediction only slightly better than a random guess. Then we try to use M1 to make predictions. Then you create a different dataset based on which rows m1 was good and bad at predicting (how well it did). Then you create another weak learner on the new dataset. We will call this model M2. Then you keep going on in this way.

Bagging is done parrelly. You create a bunch of samples at once, a bunch of independent models, and then you combine them later. While boosting is done sequentially. Becuase of this boosting is generally considered much more expensive then bagging. Bagging can take advantage of paralel computation.

You can split data into training and testing. You can also do weighted voting where not every model has same voting power. The main idea is sequential training of weak learners to get one strong model. Bagging is parrelel training of weak learners.

Boosting Methods

AdaBoosting (Adaptive Boosting)
- Will measure how well first model does on data
- Then increase weight to the rows that the first model did poorly 
- Make it more important that not well predicted rows in first model are better predicted
- The models are created with a focus on poorly predicted data of the the previous model
- Each new model focuses  more and more and the difficult to fit data

Gradient Boosting
- Second model predict first model error 
- Third model predict second model error
- And so on
- Additive learner
- Eventual output adds up all outputs of previous models
- Each model fit on modified version of original data
- Fits each new model to residuals
- This improves overall model gradually in areas where residuals were originally high


XG Boost (Extreme Gradient Boosting)
- Currently very popular
- Basically gradient boosting on steriods
- Very good implementation of gradient boosting
- 