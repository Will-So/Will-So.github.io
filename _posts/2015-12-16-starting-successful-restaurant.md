---
layout: post
title: "Starting a Successful Restaurant"
draft: false
tags: [Python, Machine Learning, Dashboards]
---


Founding a successful restaurant is hard. 60% fail in 3 years. This pales in comparison to hotel chains that average a 7% failure rate over 10 years. Growing up, my uncle Jim struggled with his restaurant for 3 years before he finally admitted defeat. 

Can we use Data to improve the chances of starting a successful restaurant? This month, I decided to find out.

The goal for this project is:

 1. Find the location that maximizes their chances for success
 2. Choose the ameneties and other features that maximize their chance of success 
 2. Predict their chances of success given their prospective location and amenities. 


# 1. Getting the Data

Thanks to the Yelp Challenge, the data I needed was easy to get and the data was shockingly consistent. There were next to no anomalous data points.

# 2. Defining Success

One challenge of this project is defining success. I don't have the profits and losses of individual restaurants so I need to infer success based on observable data we have. I define the function as $$F(Rating, \frac{Number\_Reviews}{Month}, Time)$$. The actual form of this function is somewhat complicated and can be seen in the `measuring_success` notebook. The top 25% of Yelp Restaurants according to this function were labeled as successful. 

# 3. Feature Engineering

## Location Location Location

One of the key features in my model is the success of businesses in a nearby location. I use a K-d tree to efficiently find the 10 nearest businesses to a given point and then average those success values.

## Latent Topics

I use latent topics with Latent Dirichlet allocation to make features. This combination of using an unsupervised learning technique to generate features for a supervised model can provide powerful insights.

Unfortunately, latent topics in this case were simply not predictive of success. This is despite coherent categories. 

## Explicit Topics

Explicit topics (i.e., categories) perform much better than LDA topics. They end up predicting nearly as well as location in the aggregate. 

# 4. Modeling, Evaluating, and Refining 

I tried a number of different classifiers at first. Random Forests performed the best but there was still something wrong with it. It had way too many false positives. If my model predicted that the restaurant would be successful, the restaurant would only actually be successful 50% of the time. This is much better than change (25%) but I thought we can do better.

I adjusted the classifer to have balanced classes in each tree and this decreased the false positive substantially. Now my model had a true positive rate of 66%. This represents more than a 150% increase in the chanse of being successful. 

![](https://s3.us-east-2.amazonaws.com/screens12/S3668.png)

The most important features:

![](https://s3.us-east-2.amazonaws.com/screens12/S3669.png)


# 5. Building the Web App

Since most people who want to start restaurants tend not to be very technically savy, I decided to build a web app that allows people to easily use the model I built. Building the app was extremely straightfoward thanks to the awesomeness of Flask. 

This is a screenshot of the app I built: 

![](https://dl.dropboxusercontent.com/u/97258109/Screens/S3665.png)

The user workflow is:

1. User picks city. Map pans to city.
2. User Picks the type of restaurant
3. User clicks on the map to specify a location
4. App returns probability of success. 

# Conclusion

Using data to inform our decisions when founding a new business can be extremely valuable. 

This project also shows how valuable a good frontend can be for communicating insights in data. Without a good interface it would be difficult for people to make use of this data. 

# Threats to Model Validity

- Maturation: The test dataset that we evaluated accuracy on only has currently existing companies. It could be the case that past patterns of success are no longer predictive. 
- Saturation: My model does not consider the potential of saturation. If we have too many sushi places right next to eachother, this can cause problems for profitability. Businesses being close to eachother is a good strategy to a point

# The Next Iteration 

1. Add a heat map
2. The app is currently not hosted anywhere. I hope to deploy it in the near future so others can make use of it.
3. Add NLP features based on the reviews of the restaurant. 
4. Generalize to other businesses.
