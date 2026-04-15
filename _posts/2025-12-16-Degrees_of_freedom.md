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

This constraint means that once you know $n-1$ of the deviations, the last one is determined. So only $n-1$ pieces of information are free to vary (i.e number of independent pieces of information is $n-1$). Hence the degrees of freedom is $n-1$.

Let's further explore degrees of freedom in other contexts. 

## ANOVA 

Let's take a look at the following two-way ANOVA table, with factor $A$ at $a$ levels, factor $B$ at $b$ levels, and $n$ replicates per cell.

| Source | df        | SS     | MS                             | F                    |
| ------ | --------- | ------ | ------------------------------ | -------------------- |
| $A$    | $a-1$     | $SS_A$ | $MS_A = \dfrac{SS_A}{a-1}$     | $\dfrac{MS_A}{MS_E}$ |
| $B$    | $b-1$     | $SS_B$ | $MS_B = \dfrac{SS_B}{b-1}$     | $\dfrac{MS_B}{MS_E}$ |
| Error  | $ab(n-1)$ | $SS_E$ | $MS_E = \dfrac{SS_E}{ab(n-1)}$ |                      |
| Total  | $abn-1$   | $SS_T$ |                                |                      |
2 questions arises from the ANOVA table. First one is, how did we come up with these degrees of freedom? Second one is, why do we care about this? 

I will answer first question using all three definitions of degrees of freedom. 
1. Number of independent pieces of information that goes into the computing of statistics. 

   Statistics associated each source of df is given by SS column in the above table. For Source $A$, $SS_A = \sum_{i=1}^{v} r_i \cdot \left( Y_{i\bullet} - \bar{Y}_{\bullet\bullet} \right)^2$ . Here, $Y_{i\bullet}$ denotes the sum of $i_{th}$ group, and $\bar{Y}_{\bullet\bullet}$ denotes sum of all groups. Similar to sample variance, if there are $a$ total groups, only $a-1$ pieces are independent, because last one can be determined using $\bar{Y}_{\bullet\bullet}$ . Hence the degrees of freedom associated to this statistics is $a-1$. 

2. The rank of the quadratic form of the statistics

   $SS_{A}$ can be written in its matrix form. $SS_{A}$ , The rank of its matrix form is $a-1$. 

3. The difference between the dimension of the alternative and null hypothesis parameter spaces.

   You might think we are not doing any sort of hypothesis testing here. However, there is already an inherent hypothesis testing associated with the $SS_{A}$. 
   $$H_{o} : \{\tau_{1} = \tau_2 = \tau_3 = ... \tau_v \}$$       And, $$H_A : \{\text{at least two of the } \tau_i\text{'s differ}\}. 
	$$$SS_A$ tests this hypothesis. We can see the F-test associated with this test in the ANOVA  table as well. Full model associated with this test has all $a$ parameters since they are not equal, but reduced model has all groups equal so we can write the model with only one parameter. That means the difference between the dimension of the alternative and null hypothesis parameter is $a-1$. Hence, the degrees of freedom is $a-1$. 

##Why do we care about the degrees of freedom in ANOVA? 
First, lets check