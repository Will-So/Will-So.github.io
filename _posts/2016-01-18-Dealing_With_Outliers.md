---
layout: post
title: "Attack of the Outliers"
draft: false
tags: [Statistics]
---

One of the more difficult part of making predictive models is making sure that our model is robust to outliers. Fitting data strongly to outliers tends to give us bad predictive results. There are two solution that will work well in most cases and the are a few other methods worth considering when the first two either fail or are impracticle.

## Use a robust loss function

Mean squared error (MSE) and thus root mean squared error (RMSE) are very sensitive to outliers [^1]. Sometimes, this is desirable where we want to punish an error of 20 much more than an error of 5. (400 vs 25). This is not desirable when we are trying to avoid outliers from having an exaggerated effect on our model. 

Mean Absolute Error is somewhat less sensitive to outliers because it does not square the errors. It is not "robust" to outliers but it represents a middle ground between traditional loss functions and methods discussed below. 

### The Huber Loss function

One of the better loss functions to choose is the Huber Loss function. It is [defined](http://en.wikipedia.org/wiki/Huber_loss) as:

$$L_\delta (a) = \begin{cases}
 \frac{1}{2}{a^2} & \text{for } |a| \le \delta, \\
 \delta (|a| - \frac{1}{2}\delta), & \text{otherwise.}
\end{cases}$$

This has the effect of combining Squared Errors and Absolute Errors. For $$\mid a\mid \le \delta$$, the error function represents the RMSE. For $$ \mid a\mid \ge \delta$$, the error function represents Mean Absolute Errors.

## Winsorizing the Data

Winsorizing the data provides the same results as the Huber Loss Function when $$\delta$$ in the huber loss function is equal to the point at which the value is winsorized. This has a large disadvantage compared to the Huber Loss Function in that we are making changes to the dataset. That leaves us with a choice between making two copies of the data (one winsorized and non-winsorized) or discarding some information that may come in useful in future analysis. The former may not be practical for very large datasets.

An advantage of winsorizing data is that it makes it easy to visualize that data in a meaningful way. 


Pandas has `pd.clip()` for this capability which makes it easy to use. 

![](http://upload.wikimedia.org/wikipedia/commons/c/c1/RhoFunctions.png)
Source: Wikipedia

## Discarding Outliers

Most data munging has outliers that are almost certainly products of measurement errors or very rare anomalies. 

Some examples of this:

1. Demographic surveys where someone has 17 PhDs
2. Someone borrowing from lending club with a yearly income of 7M.
3. A stock transaction with a trade time before stock markets opened.

Even in the event some of those observations were accurately measures (quite unlikely), they almost surely lack external validity so will only hurt the performance of our model unless we delete them.

## Use a Model that is Robust to Standard Errors

Support Vector Machines in particular tend to not consider outliers during training. Gradient Boosting, and Random Forests also have nice properties that make it difficult (but not impossible) to overfit a training dataset. 

# A Bad method: Log transforms

Log transforms are often mooted as a good way to make outliers stand out less. When we use a log transform we are allowing the outliers to dictate how we describe **all of our observations**. This is just the opposite of using a robust measure.

This is not to say that log transforms should not be used. There are many situations when log transforms should be used **due to the behavior of all the observations**:

1. When the errors have an extreme positvely skewed distribution. We then use the [Box-Cox](http://en.wikipedia.org/wiki/Power_transform#Box.E2.80.93Cox_transformation). This allows us to improve the validity of the Pearson Correlation coefficient, stabalize variance, and in general make traditional descriptive statistics more meaningful.
2. When the relationship being investigates is close to exponential.
3. When you want to represent a model that explains the explanatory variables' influence in terms of percentages (relative values) rather than absolute differences. 

# References

- http://stats.stackexchange.com/questions/48267/mean-absolute-error-or-root-mean-squared-error
- http://projecteuclid.org/download/pdf_1/euclid.aoms/1177703732

## Footnotes

[^1]: This is easy to see from the mathematical definition of MSE: $$1/n \sum( \hat Y_i- Y_i)^2$$. Since the distance between the sample mean and the datapoint is squared, it exaggerates the influence of outliers. 


