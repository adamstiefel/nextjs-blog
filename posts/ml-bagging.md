---
title: "Machine Learning - Bagging"
date: "2023-09-19"
---

What is Bagging?

Bagging is an ensemble technique that requires multiple models. We start with the same data but then take different samples of it. The samples are created randomly with replacements. Some data repeats over multiple samples - which is fine.

For example, if the data consists of a million data points. One sample could contain a hundred thousand data points. There would be many samples. 

Then with each sample, we create a model from that sample. So when a model is overfitting, it will only be overfitting the sample, not the entire dataset. When you combine all the models for a single prediction, the prediction cannot overfit the full dataset. 

Bagging reduces variance and guards you from overfitting the data. You do not have to worry about overfitting the individual samples. Bagging is what we call parallel model building.