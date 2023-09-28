---
title: "Machine Learning - AdaBoost"
date: "2023-09-27"
---

Stands for adaptive boosting. Modifying the data set by giving it more or less weights in every step retraining simple models (weak learners) with those weighted datasets.

First dataset used to create a simple model and equaly weighted. Then the model is used to try to predict on that dataset. Then the points that were predicted incorrectly are given greater importance for the next dataset.

The weight for each point of the first dataset is 1/8. Then based on the predictions each datapoints weight either increases or decreases. If the datapoint was predicted wrong then the importance increases. If the datapoint was predicted correct than importance decreases.

AdaBoost takes a weighted voting average in order to make the final prediction. We combine the weak learners to create a strong learner.

AdaBoost builds weak learners that are decision trees with restricted depth. Weak learners mean that they are primarily one or two step decision trees.