---
title: "Machine Learning - Why Sample with replacement?"
date: "2023-09-19"
---

Sampling with replacement vs. Sampling without replacement

Dataset: A, B, C, D

Sampling without replacement: C, A, D, B

When sampling without replacement, you extract a row and then do not pick it again. In the example above,  we chose all possible row combinations.

Sampling with replacement: A, B, A, C, B, D, C, A .....

The difference is clear. Sampling with replacement is limitless because you can select a row multiple times. In the example, you will roughly get 1/4 of each even if you go to infinity - being consistent with the distribution.

Typically sampling is done when the original data set is immense, and you want smaller sample data sets. In machine learning, we sample to build different models with some independence from each other. If every model is the same, there is no use for more than one model.

Practitioners typically generate sample datasets of the same size. However, there is no rule that they must be the same size. When generating data sets of the same size, you will have diversity among those datasets. If you sample without replacement, you will have the same data points in a different order. The primary reason we sample - is because we want a variety. Sampling with replacement provides us with the goal we are looking for.

Sampling with replacement provides us with independent data sets. Independent datasets lead to independence amongst the models built. Independent models are better at making predictions.

Imagine you have a big box of colorful marbles, and you counted  100 of them. Each marble has a unique design, just like snowflakes. Now, close your eyes and try to grab one marble at a time from the box. Each time you pick one up, note its design, and then gently put it back without peeking. We call this "sampling with replacement."
Now, here's a fun fact: Every time you reach into the box without peeking, each marble has an equal chance of being picked by you. But just because they all have an equal chance doesn't mean you'll pick every marble if you do this 100 times. Some marbles might feel left out and not get chosen even once, while others might get chosen multiple times.
Math wizards, like the ones who came before us, have discovered something cool about this marble game. If you keep trying to pick all 100 marbles by diving your hand into the box 100 times, there's a magical number that comes up. It turns out that, on average, you'll end up picking about 63 out of the 100 unique marbles. The other 37 might just be feeling a little shy that day!

Why 63, you ask? It has to do with a special number known as 'e'. It's a magical and mysterious number, just like Pi. When mathematicians played around with these numbers, they found out that the chance of a marble not getting picked even once is about 36.8% (or 0.368 for short). And so, the chance of a marble getting picked at least once is the opposite, which is roughly 63.2% (or 0.632 for short).