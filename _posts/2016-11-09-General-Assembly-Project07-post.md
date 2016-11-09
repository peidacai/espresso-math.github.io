---
layout: post-no-feature
title: "GA Project07"
description: "Clustering Airports delay"
comments: true
category: articles
mathjax: false
quote: "Torture the data, and it will confess to anything."  Ronald Coase, Economics, Nobel Prize Laureate
---

![delay_comic]({{site-url}}/images/proj07_comic.jpg)

### Project07: Clustering United States airports delay

### 1. Introduction / Project Aim

This week, we investigated airport delays in the United States, with airport yearly averaged data from 2004 to 2014. The task was to identify clusters of airports which have similar operational characteristics and delay performances.

#### a. Data science areas of interest

- Dimensionality reduction
- Principal component analysis
- Clustering

### 2. Risk and assumptions

- Delays are given in averages, no information was provided on the variances and the metric for average is not known.
- It was assumed that the data was obtained via a rigorous process and veracity of data can be assumed.

### 3. Exploratory Data Analysis

For each airport, the data contained averaged yearly data on operational performance and delay times.

![featuresl({{site-url}}/images/proj07_features.png)

The features are broadly divided into 3 types:

- Geographical information (FAA Regions, state, county, etc)
- Size of airport (departures and arrivals count)
- Operational performance metrics (various delay metrics)

![FAA_regions_original({{site-url}}/images/proj07_original_FAA_REGIONS.png)

All airports were originally clustered into 9 different FAA regions by geographical locations and primarily for administrative purposes.

![featuresl({{site-url}}/images/proj07_hist_depart_arr.png)






### 4. Dimensionality reduction


### 5. Unsupervised machine learning (KMeans clustering and DBSCAN)


### 6. Underlying trends within clusters



### 7. Conclusion and future work

