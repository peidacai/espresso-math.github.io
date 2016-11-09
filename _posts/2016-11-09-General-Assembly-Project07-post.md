---
layout: post-no-feature
title: "GA Project07"
description: "Clustering airports delay"
comments: true
category: articles
mathjax: false
quote: "Torture the data, and it will confess to anything. - Ronald Coase, Economics, Nobel Prize Laureate"
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

![features]({{site-url}}/images/proj07_features.png)

The features are broadly divided into 3 types:

- Geographical information (FAA Regions, state, county, etc)
- Size of airport (departures and arrivals count)
- Operational performance metrics (various delay metrics)

![FAA_regions_original]({{site-url}}/images/proj07_original_FAA_REGIONS.png)

All airports were originally clustered into 9 different FAA regions by geographical locations and primarily for administrative purposes.

![depart_arr_hist]({{site-url}}/images/proj07_hist_depart_arr.png)

There seems to be 3 types of airports based on the volume of departures and arrivals:

- Below 100,000 departures/arrivals; 
- 100,000 to 300,000;
- Above 300,000

This could provide insights into number of clusters later, when conducting unsupervise machine learning.

![scatterplot_size_delays]({{site-url}}/images/proj07_scatterplot_delay_traffic.png)

From this scatterplot, it seemed that there was no strong correlation between volume of traffic handled and delays. The airports with the worst delays handled less than 200,000 arrivals / departures. And there were no clear relationships between FAA regions and delays.

### 4. Dimensionality reduction (2 and 3 principal components)

Principal component analysis was conducted and used as a dimensionality reduction tool. The aim was to reduce the dimensions to 2 so that unsupervised machine learning clustering can be conducted to figure out the underlying airport clusters.

The top 2 principal components accounted for explained variance ratio of nearly 75%. This meant that the reduction from 28 features to 2 features lost only about 25% of the explainability of all features.

Adding a third component increased explained variance ratio to almost 82%.

![clustered_airports]({{site-url}}/images/proj07_clustered_airport_dropped corr.png)

![3D_clusters]({{site-url}}/images/proj07_3D_pca_kmeans.png)

![clustered_airport_lat_lon]({{site-url}}/images/proj07_clustered_airport_lat_lon_all.png)
### 5. Unsupervised machine learning (KMeans clustering and DBSCAN)


### 6. Underlying trends within clusters



### 7. Conclusion and future work

