---
layout: post
title: Everything about Random Forests
draft: false
tags: [Python, Machine Learning, scikit-learn]
---

Random Forests are an increasingly popular machine learning algorithim. They perform well in a wide variety of learning and prediction problems despite being exceedingly easy to implement. Random forests can be used for both classification and regression problems.

This post describes the intuition of how Random Forests work as well as its. advantages and disadvantages.

# Intuition for How Random Forests Work

At a high level, the algorithim usually works as follows:

1. Randomly sample $$N$$ observations from the training set at random with replacement [^1]
2. Train a decision tree using $$N$$ and $$\sqrt{num\_features}$$.

Repeat (1) and (2) B times, where B is the number of trees (also called bags). Depending on the nature of data, the ideal number of trees ranging from 100 to several thousand.

![](http://image.slidesharecdn.com/janvitekdistributedrandomforest5-2-2013-130504133205-phpapp02/95/jan-vitek-distributedrandomforest522013-8-638.jpg?cb=1367674437)
[Source: Jan Vitek, Perdue](http://www.slideshare.net/0xdata/jan-vitek-distributedrandomforest522013)

# Main Advantages

## Embarrassingly Parallel

SKlearn will allow you to use all the cores in your computer to train multiple trees at the same time. The algorithim is also available in Spark. This is because all random forests are trained independently of eachother.

## Resistant to Overfitting

The only way to overfit the model is to have trees that have too many branches. Increasing the number of trees does not increase the risk of overfitting. Instead an increasing number of trees tends to decrease the amount of overfitting 

## No Need for Cross Validation

We can also avoid cross-validation by using the *out of bag score*. This is available as an argument in Spark and Sklearn and provides nearly identical results to N-fold cross-validation.[^2]

## Other Advantages
- Most implementations make it trivial to find feature importance
- It is very easy to Tune
- Random Forests excel with highly non-linear relationships in the data.

# Main Disadvantages 

## Can perform worse than Gradient Boost

Gradient Boosted Machines tend to perform better under both of the following conditions:

1. We correctly tune the relatively complicated hyperparamters of GBM. 
2. Either 1) your data fits in ram (for sklearn) or 2) your problem is not a multiclass classification problem (for Spark).
3. (My understanding, not certain) Non-linear relationships 

## Slow to Predict

Since predicting a new observation requires running the observation through every tree, Random Forests will often perform too poorly for real-time prediction.

## Doesn't behave well for very sparse feature sets

In this case, SVM and Naive Bayes tend to perform better than Random Forests. One of the reasons for this is because each tree only has access to $$\sqrt{n}$$ features by default. If very few features are of any importance, most trees will miss important features. 

# Using it in sklearn

Below is an example of using a Random Forest with 50 trees:


```python
model_rf = RandomForestClassifier(n_estimators=200, oob_score=True, verbose=1,
                                  random_state=2143, min_samples_split=50, 
                                 n_jobs=-1)
fitted_model = model_rf.fit(X, y)
```

The `oob_score` argument tells the classifier to return our out of sample (bag) error estimates. The `min_samples_split=50` argument tells the classifer to only create another branch of the tree if the current branch has more than 50 observations. `n_jobs=-1` tells SKlearn to use all the cores on my machine. 

# Using it in Spark

An example of using Random Forests in Spark can be found [here](https://spark.apache.org/docs/latest/mllib-ensembles.html). Here is the same example as in SKlearn:

# Practical tips

- A good way to prevent overfitting is to set a relatively high tree-splitting threshhold. This is the `min_samples_split` argument in SKLearn. 
- We can easily check if we are overfitting by looking at the out of bag error. 
- More trees are always better than fewer. The returns start to dominish quickly. 
    <img src="https://dl.dropboxusercontent.com/u/97258109/Screens/S3580.png" width="300"/>
    - Source [here](http://www.isip.piconepress.com/projects/dpm_inference/html/performance.html)
- Having trees that are too deep can lead to overfitting. This can be avoided by increasing the number of trees. 
- The default number of features ($$\sqrt{n}$$) is usually a sufficiently good value. 
- After picking the best Random Forest with your out of bag errors, 


# References
1. Friedman, Jerome, Trevor Hastie, and Robert Tibshirani. The elements of statistical learning. Vol. 1. Springer, Berlin: Springer series in statistics, 2001.
APA	
2. http://www.slideshare.net/0xdata/jan-vitek-distributedrandomforest522013
3. http://scikit-learn.org/stable/modules/ensemble.html
4. [Random Forests by Leo Breiman](http://download.springer.com/static/pdf/639/art%253A10.1023%252FA%253A1010933404324.pdf?originUrl=http%3A%2F%2Flink.springer.com%2Farticle%2F10.1023%2FA%3A1010933404324&token2=exp=1447688130~acl=%2Fstatic%2Fpdf%2F639%2Fart%25253A10.1023%25252FA%25253A1010933404324.pdf%3ForiginUrl%3Dhttp%253A%252F%252Flink.springer.com%252Farticle%252F10.1023%252FA%253A1010933404324*~hmac=3b7a5a59b9ee6c2c4c500ef943cd2e0725e74621ba10977e4b51c8989b1819c7)

[^1]: This means that the sample size will be the same as the training set size but the composition of the sample will be different because it is being sampled with replacement. 

[^2]: This works by looking at the errors of the trees where the relevant observation $$(X_i, y_i)$$ did not occur. 
