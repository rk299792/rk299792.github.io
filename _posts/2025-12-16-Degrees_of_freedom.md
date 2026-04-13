---
layout: post
title: "Degrees of Freedom"
date: 2025-12-16
description: "Why do we divide by n−1 in sample variance? A deep dive into degrees of freedom in statistics."
tags: [statistics, inference, probability]
---

We come across degrees of freedom in statistics everywhere. Yet very few courses dive deep into this topic. The first place we encounter degrees of freedom is when discussing sample variance:

$$\hat{\sigma}^2 = \frac{\sum(X_i - \bar{X})^2}{n-1}$$

An obvious question arises: why are we dividing by $n-1$ instead of $n$? Usual answers are:

1. Dividing by $n$ makes the estimator biased; dividing by $n-1$ corrects for that bias.
2. Because the degrees of freedom of this statistic is $n-1$.

But what *is* degrees of freedom? There are several definitions:

1. **Number of independent pieces of information** that go into computing the statistic.
2. **The rank of the quadratic form** of the statistic.
3. **The difference between the dimension of the alternative and null hypothesis** parameter spaces.

Definitions 2 and 3 ultimately reduce to definition 1. However, they are more useful in contexts where "independent pieces of information" is not immediately obvious.

## Why n−1?

Given observations $X_1, X_2, \ldots, X_n$, we estimate the mean $\bar{X} = \frac{1}{n}\sum X_i$. The deviations from the mean satisfy one linear constraint:

$$\sum_{i=1}^{n} (X_i - \bar{X}) = 0$$

This constraint means that once you know $n-1$ of the deviations, the last one is determined. So only $n-1$ pieces of information are free to vary — hence the degrees of freedom is $n-1$.

## Example in Python

```python
import numpy as np

x = np.array([1, 2, 3, 4, 5])

# Biased estimate (divides by n)
var_biased = np.var(x, ddof=0)

# Unbiased estimate (divides by n-1)
var_unbiased = np.var(x, ddof=1)

print(f"Biased variance:   {var_biased:.4f}")
print(f"Unbiased variance: {var_unbiased:.4f}")
```
