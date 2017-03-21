---
layout: post-no-feature
title: "General Assembly (NYC) Capstone 04"
description: "Predicting NYC retail rental prices. Did we do better?"
comments: true
category: articles
mathjax: false
quote: ""
---

![property_comic]({{site-url}}/images/NYC_taxi.jpg)

### Capstone Project Part 4: Geospatial Visualization and Predictive modeling

In my previous three blog posts, I covered the three main sources of data for my project, which is to create a predictive model to predict New York City retail rental prices using taxi passenger dropoff data and  social media data to model customer volume and area reputation so as to improve the predictive ability of my model.

### Geospatial Visualization

I spent quite some time figuring out how to use the folium package in Python to visualize the geospatial data, somehow combined all three data sources into one big visualization which is offer limited interactive functions.

<iframe width="800" height="400" src="{{ site-url }}/images/map.html"></iframe>


Getting the map to center on a specific latitude and longitude and at a certain zoom level is rather straight-forward.

### Modeling Approach

![model_approach]({{site-url}}/images/capstone_model_approach.png)

A baseline model was created using traditional rental data: (1) Available floorspace; and (2) Zipcode.

A second model was created with Yelp business review data added as features.

A third model was created with Yellowcab dropoff zipcode count data.

Finally, a last model was created with the dropoff times binned into 4 6-hourly time windows.

Recursive feature selection was performed on the final model to determined the feature importance.

### Performance of predictive models

![model_performance]({{site-url}}/images/capstone_model_performance.png)

### Feature Importance

![top_5_features]({{site-url}}/images/capstone_top_5_features.png)

### Conclusion and Way ahead


