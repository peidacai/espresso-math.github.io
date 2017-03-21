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

I spent quite some time figuring out how to use the folium package in Python to visualize the geospatial data, somehow combined all three data sources into one big visualization which offers limited interactive functions. However, due to the chosen blog theme, the toggle button to remove and add layers cannot be displayed here. I may have to decide which is less painful: dig deep into the D3 javascript code to reposition the layer button or edit the layout of the blog to accomodate wider iframes. (Work in progress)

<iframe width="800" height="400" src="{{ site-url }}/images/map.html"></iframe>

### Modeling Approach

![model_approach]({{site-url}}/images/capstone_model_approach.png)

A baseline model was created using traditional rental data: (1) Available floorspace; and (2) Zipcode.

A second model was created with Yelp business review data added as features.

A third model was created with Yellowcab dropoff zipcode count data.

Finally, a last model was created with the dropoff times binned into 4 6-hourly time windows.

Recursive feature selection was performed on the final model to determined the feature importance.

### Performance of predictive models

![model_performance]({{site-url}}/images/capstone_model_performance.png)

As shown above, as Yelp data and Taxi dropoff data were added as additional features, the model performed better in both R2 and RMSE performance metrics. Modeling performance was obtained with 10-fold shuffle-split cross-validation.

Under an average scenario, a rental unit of 2,700 sqft, my model reduced the RMSE of predictions by an average of $4.80 per sqft per year. That translates to a potential saving (from erroneous rental pricing) of $12,960 per year per rental unit.

### Feature Importance

![top_5_features]({{site-url}}/images/capstone_top_5_features.png)

As expected, the taxi dropoff data was a useful proxy for potential customer reach and hence dropoff count between 1200hrs and 1800hrs was the most important feature.

Features approximating location reputation came in fourth most important. "Cost_mean" refers to the average number of "$" for the top 10 businesses surrounding each available rental unit.

All in all, this further strengthen my original hypothesis of improved predictive abilities with the addition of customer reach and reputation data sources.

### Conclusion and Way ahead

In summary, the project demonstrated a proof-of-concept for the use of proxies on customer reach and location reputation. The next step should be to scale up and use this approach with a larger rental prices dataset further improve the robustness of the model.
