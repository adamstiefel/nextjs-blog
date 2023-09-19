---
title: "Machine Learning - Ensemble Techniques"
date: "2023-09-19"
---

What are Ensemble Techniques in Machine Learning?
Ensemble techniques in machine learning are methods that combine multiple models to enhance overall performance. These often result in more accurate and robust predictions than what can be achieved by any individual model.

The fundamental concept is to harness the strengths of each individual model while mitigating their weaknesses. This combination significantly enhances predictive value. The key lies in ensuring that the models are independent of one another. Strong models (e.g., 90% accuracy) are beneficial, but it's more crucial for the models to differ to boost accuracy. A group of highly diverse (independent) weak learners (models with low accuracy) can still culminate in a robust prediction. While strong learners are preferred, a collection of "weak learners" can be effective if they are sufficiently diverse.

The overarching principle is that a committee of experts can make better decisions than a single expert. Each model should offer a unique perspective to be valuable. Replicas of the same model offer no advantage.

These techniques involve amalgamating different models to derive a single prediction. One approach is to let each model cast a vote, with the majority decision prevailing. This approach is particularly effective for binary classification tasks, such as determining if someone will default on a loan: yes or no.

Ensemble methods typically reduce prediction variance. They usually comprise 100 or 200 models.

The process involves creating a variety of models using training data. Predictions are then made on test data to obtain the ultimate combined forecast. Each model predicts the outcome for the test data; these predictions are then combined to produce a singular prediction.

It's worth noting that computational time is considerably longer with ensemble techniques than when using a single model. However, many argue that this trade-off is justified. If numerous weak learners are present, the added computational time is a necessary investment to achieve strong predictions. Many hesitate to spend extra time merging strong learners. Plus, these strong learners are often more intricate, leading to extended computational durations. Ensemble techniques are typically associated with merging weak learners because the trade-off between computational effort and accuracy is reasonable.

Ensemble techniques are versatile and can be applied to both regression and classification tasks. For classification, the method proceeds row by row, considering the majority vote (yes or no). For regression, it progresses row by row, averaging the values to determine the final prediction.
