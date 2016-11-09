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

### 5. Unsupervised machine learning (KMeans clustering and DBSCAN)

KMeans clustering and DBSCAN were used to cluster the scatter plots on the principal component planes. In this situation, KMeans performed better.

Silhouette scores:
- KMeans (5 clusters) = 0.401
- DBSCAN = -0.352 (40 clusters created)

![clustered_airport_lat_lon]({{site-url}}/images/proj07_clustered_airport_lat_lon_all.png)

When we examine the scatter plot visually, it became clear DBSCAN performed poorly in creating clusters. The scatter plots were rather close together and there were no distinct groups. KMeans, having seen there were possibly 3 or more clusters (3 distinct groups of traffic volume) earlier, we were able to try K (number of clusters) within that region to achieve the ideal number of clusters with the highest silhouette score.

There was an interesting discussion on having to drop highly correlated features before conducting PCA as having too many similar would increase the weightage of certain principal components, perhaps unfairly. Therefore, a PCA/KMeans clustering was conducted to examine if removing highly correlated features would give us better clustering (although my initial feel was that it wouldn't, if anything, it would result in less segregation).

![clustered_airports]({{site-url}}/images/proj07_clustered_airport_dropped corr.png)

4 clusters had the highest silhouette score 0.406 (slightly better, poorer performance of 0.384 with 5 clusters). The cluster plots above showed that 4 clusters may not be a good fit, particularly the yellow cluster.

![3D_clusters]({{site-url}}/images/proj07_3D_pca_kmeans.png)

A third component was added to the PCA (explained variance ratio of almost 0.82) and a 3D plot was created. As with all 3D plots, without the ability to rotate the plot in real-time, the plot couldn't covey much else over a 2D plot.

### 6. Underlying trends within clusters (done with Tableau)

![Clustered_arrival_delay]({{site-url}}/images/proj07_clustered_arrival_delays.png)

![Depart_metric]({{site-url}}/images/proj07_depart_metric.png)

We see that cluster 1 consisted of airports with the highest arrival delays, while cluster 2 consisted of airports with the highest cancellations and diversions. The fact that they belonged in 2 different clusters indicated that cancellations and diversions might not have been the reasons behind the delays.

![Size_delay]({{site-url}}/images/proj07_size_delay.png)

The above plot showed the relationship between traffic volume handled (size of cicrle) and average block delay. Again, there was no indication of high traffic volume correlating with higher delay.

![Clustered_geo]({{site-url}}/images/proj07_clustered_airports.png)

When we plotted the clusters in their respective geographical locations, we saw some light in explaining the delays. Cluster 1 (Orange) fell mostly along the eastern coast of Northern American continent (all the west south to Florida), perhaps this had something to do with weather as this is also the [Atlantic hurricane belt](http://www.nhc.noaa.gov/climo/).

Cluster 2 (red), on the other hand, may not have correlation with geographical locations. This could be further looked into to examine the reason for their high numbers of cancellations and diversions.

### 7. Conclusion and future work

Some areas of future work could be:

- Combine with weather data, especially hurricane data to investigate correlation with delays, perhaps looking into delays in hurricane months.

- Examine more operational data from Cluster 2 (red) to investigate high level of cancellations and diversions.

- Overlay flight paths on to the geographical clusters.

Due to the nature of the data (yearly average), this project was only able to provide a high level clustering of the delays, without being able to dive deeper into the detailed reasons behind the delays. One other area to look into is optimizing efficiency in airport management and operations and some of the data we could look into could include individual flight details, employee and passenger surveys and employee operational data (hours worked, etc).
