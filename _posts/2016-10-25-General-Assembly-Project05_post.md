---
layout: post-no-feature
title: "GA Project05"
description: "Data science for disaster relief"
comments: true
category: articles
mathjax: false
---

![titanic]({{site-url}}/images/titanic-image.png)

### Project05: Data science for disaster relief

### 1. Introduction

In this project, the task was to collect the titanic passenger data from an AWS PostgreSQL instance via Python + Jupyter Notebook, clean up the data before finally creating a bunch of classifying predictive models and compare the performances between the models.

### 2. Data exploration

The data obtained from the AWS instance had the following characteristics:

- Columns: 19
- Rows: 891

Each row corresponded to a single titanic passenger. Some of the more prominent features included: age, fare, passenger class and port embarked.

#### a. Risks and assumptions of data

- Since the complete passenger list consisted of over 2,400 passengers, we had to assume that this data we had was representative of the population and not biased in any way.

- Secondly, without information on the source and the method of obtaining the data, we had to assume the veracity of the data, i.e. passenger A indeed travelled with X siblings/spouse, paid $Y fare, etc.

- Thirdly, titanic happened over a century ago. With advancement in technology in ship safety and higher level of education, insights and predictions from this dataset may not necessarily be relevant to disaster relief operations today.

### 3. Exploratory Data Analysis

![titanic_pairplot]({{site-url}}/images/titanic_pairplot.png)

![violinplot]({{site-url}}/images/violinplot_sex_age.png)

![swarmplot]({{site-url}}/images/swarmplot_class_age.png)

Plots for the most prominent features (Sex and passenger class) were shown above. The plots showed that being a male and being a third class passenger had the lowest chances of survival.

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

![roc_logreg_knn_tree]({{site-url}}/images/roc_logreg_knn_tree.png)

A decision tree model was used to model the data with a gridsearchCV modifying "splitter", "max_depth" and "max_features" parameters of Decision Tree. In general, decision tree model did not perform as well as LogReg or KNN.

![roc_compare_all]({{site-url}}/images/proj5_roc_compareall.png)

A bagging classifier model was introduced on top of the decision tree model to randomly resample the data (putting them into a bag) and predicting using the decision tree model created. This improved the performance of the decision model, putting it on par with LogReg and KNN.

### 6. Conclusion

The main takeaway from this project was the understanding of ROC curve as well as precision-recall curve. The other area was the use of other modeling techniques such as DecisionTree and Bagging classifier to help further tune the model.

As mentioned in the risks and assumptions section above, in order to build a more up-to-date predictive model, more modern data sets are required.

Finally, this project also struck a chord with me personally. A search and rescue operation at sea was a very real possibility that I used to be trained for and prepared to conduct at short notice while serving in the navy. This is one of the reasons why I have a strong desire to join the data science community - to use data in service of the greater community and for a greater good.
