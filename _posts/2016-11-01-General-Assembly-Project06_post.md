---
layout: post-no-feature
title: "GA Project06"
description: "IMDB Movie ratings"
comments: true
category: articles
mathjax: True
quote: "The most powerful leadership tool you have is your own personal example. - John Wooden"
---

![IMDB_logo]({{site-url}}/images/logo-IMDB.jpg)

### Project06: Predicting movie ratings for top 250 all-time favorites

### 1. Introduction / Storyline

In this project, we pretended to be hired by Netflix to work with movie data, to try and figure out the factors which determined the IMDB movie ratings for top movies of all time. 

Here, we explored ensemble modelling, use of APIs and natural language processing.

#### Problem statement:

- To determine the most influential factors which determine IMDB ratings of a mmovie

### 2. Data exploration

Data from top 250 movies was obtained using the [IMDB API](https://www.omdbapi.com/), prominent features include: actors, title, awards, director and plot.

Original data from API:
- Columns: 20
- Rows: 250

Supplementary data (scraped from IMDB website):
- USA box office gross revenue
- Sound technology used in movie

#### a. Risks and assumptions of data

- 250 movies may not be large enough dataset to properly train a model to predict IMDB ratings. More movies should be obtained to create a better model.

### 3. Exploratory Data Analysis

![rated]({{site-url}}/images/proj6_movies_by_ratings.png)

- Movie ratings distribution of Top 250 movies

![top_directors]({{site-url}}/images/proj6_top_directors.png)

- Director distribution of Top 250 movies

### 4. Logistics Regression and K-Nearest Neighbor modelling

A total of 19 features were used as inputs into a logistics regression model to predict if a passenger survived. However, accounting for the arbituary intercept and dummy variables for categorical features, there were only effectively 7 features:

- Sex
- Passenger Class
- Age
- Fare paid
- Number of siblings and/or spouse
- Number of parent and/or children
- Port embarked

![top_coeff]({{site-url}}/images/proj5_top5coeff.png)

The top most influential coefficients were Sex (male); Passenger Class(third class); 3 siblings and/or spouse; and 4 siblings and/or spouse.

![logreg_conmat]({{site-url}}/images/proj5_best_logreg_conmat.png)

Confusion matrix for best logistics regression with cross-validation (5-fold)

![knn_conmat]({{site-url}}/images/proj5_best_knn_conmat.png)

Confusion matrix for best k nearest neighbors

![roc_knn_logreg]({{site-url}}/images/roc__knn_logreg.png)

Comparison of ROC curves and AUC for Logistics Regression and K Nearest Neighbors

From the ROC curves comparison, Logistics Regression (LogReg) had higher Area Under Curve performance compared with K Nearest Neighbors (KNN). Below a True Positive Rate (TPR) of about 0.85, LogReg model performs better than the KNN model. However, while in search of higher TPR (above 0.85), KNN starts to perform better, increasing TPR with increasing False Positive Rate (FPR) compared to LogReg whichs maintains the same TPR with increasing FPR.

### 5. Optimizing for Precision Recall as opposed to Accuracy

![precision_recall]({{site-url}}/images/proj5-precision-recall.png)

![pr_conmat]({{site-url}}/images/proj5_precision_recall_conmat.png)

While in a ROC curve, we strive to be in the top left quadrant, in a precision-recall curve, we are striving to be in the top right quadrant instead, i.e. perfect precision and recall of 1.0 each.

However, in the plot above, it is evident that there is a compromise, gain one and lose the other (downward slope). In the context of a disaster relief, we are striving for highest possible recall (rescue all survivors). In the hypothetical situation with unlimited resources (time, manpower and equipment) for rescue operations, we can operate at recall = 1.0 and a low level of just above 0.4 for precision, to make sure all survivals are rescued, i.e. leave no stone unturned.

In the real-world however, time is always the biggest enemy. Facing elements of winds and currents (perpetual expanding search area at the rate of radius squared compared to linear search efforts), combined with the very real threat of hypothermia, rescue efforts at sea have to be balanced with limited rescue units such as ships and other air assets like helicopters and drones.

With low precision and high recall, rescue efforts would have be increased many folds, just to sift through more false positives, which takes resources away from rescuing the true positives.

In this case, two possible recommendations are offered, depending on the amount of rescue resources there are at hand: 

1) 0.8 recall and 0.8 precision;

2) 0.9 recall and 0.47 precision

### 6. First look at DecisionTree and Bagging

![model_performance]({{site-url}}/images/proj6_model_performance.png)

A decision tree model was used to model the data with a gridsearchCV modifying "splitter", "max_depth" and "max_features" parameters of Decision Tree. In general, decision tree model did not perform as well as LogReg or KNN.

![roc_compare_all]({{site-url}}/images/proj5_roc_compareall.png)

A bagging classifier model was introduced on top of the decision tree model to randomly resample the data (putting them into a bag) and predicting using the decision tree model created. This improved the performance of the decision model, putting it on par with LogReg and KNN.

### 6. Conclusion

The main takeaway from this project was the understanding of ROC curve as well as precision-recall curve. The other area was the use of other modeling techniques such as DecisionTree and Bagging classifier to help further tune the model.

As mentioned in the risks and assumptions section above, in order to build a more up-to-date predictive model, more modern data sets are required.

Finally, this project also struck a chord with me personally. A search and rescue operation at sea was a very real possibility that I used to be trained for and prepared to conduct at short notice while serving in the navy. This is one of the reasons why I have a strong desire to join the data science community - to use data in service of the greater community and for a greater good.
