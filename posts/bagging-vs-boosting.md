---
title: "Machine Learning - Bagging vs Boosting"
date: "2022-09-26"
---

Baggging

Parallel Training
- All weak learners built in parallel
- Indpendent of one another

Equal weight
- Each weak learner has the same weight in final prediction

Independent samples
- Samples are drawn from original dataset with replacement
- To be used to train each weak learner

Help reduce model variance

Ex: Random Forest, Bagging Classifier


Boosting

Sequential Training
- Weak learners built one after another 
- To improve accuracy from prior learners

Weighted Average
- More weight to the weak models that perform better

Dependent samples
- Each new sample has more of the observations that had high errors in previous weak learner

Help reduce bias of model

Ex: AdaBoost, Gradient Boosting Classifier