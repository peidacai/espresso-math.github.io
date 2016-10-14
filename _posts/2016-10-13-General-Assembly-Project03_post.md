---
layout: post-no-feature
title: "GA Project03"
description: "Business Intelligence with Data Science"
comments: true
category: articles
mathjax: true
---

![whiskey]({{site-url}}/images/whiskey.png)

### Project03: Predicting IOWA liquor sales

### 1. Introduction

In this project, the task was to conduct market research as a data scientist, and recommend locations for a new liquor store in IOWA state. 2015 and 2016 Q1 statewide liquor store transactional data was provided for the research and there were 3 sub-tasks:

   - Identify suitable locations for a liquor store owner's expansion plans
   - Create a model to predict future sales
   - Recommend target for bottles sold and average bottle price

### 2. Data exploration

The data provided had the following characteristics:

- Columns: 18
- Rows: 2,709,551

Each row corresponded to a single transaction in IOWA. Some of the more prominent features of each entry included: county, city, zip code, item description, price per bottle, cost per bottle, bottle sold and sales (or revenue).

### 3. Data munging

All columns were munged and cleaned. Cleaning process included:

- Conversion to appropriate types (integer, float and string)

- Filling empty cells, mostly using information from other similar cells within the same column. For example, over 10,000 cells had empty "County" cells. This was easily resolved by using the corresponding non-empty "City" cell as a key to find the "County" entry for another row with same "City" entry. This was possible since "City" is a subset category of "County" so cells with same "City" entry, would have same "County" entry.

- Correcting cells with wrong value. Examples include zip code of '712-2', when it should have been a 5 digit format. This was corrected manually.

- Finally, an wrong zip code was discovered when using Tableau for EDA. Turned out "52601" was wrongly entered as "56201", resulting in a store that was not in IOWA state.

![wrong_zip]({{site-url}}/images/wrong_zip_des_moines.png)

### 4. Data mining

#### Supplementing transactional data with externally sourced demographics data

After cleaning the data, county demographic data (dated 2010) were used to supplement the existing dataset in search of a richer analysis. However, without detailed demographic data (such as age groups, gender, income range, etc), using simple land area per store and population per store yielded little value.

For example, a county with large land area and a low number of stores could be thought of as a good candidate, since it could mean that the area is underserved, liquor-wise. Similar arguments could be made for high population per store, since it would mean each store can serve a higher number of customers.

Nevertheless, such simplistic analysis would be proven misguided with data below. As a wise man once said, 

__"Let the data be the arbituer of truth."__

![pop_vs_median_sales]({{site-url}}/images/blob_population_vs_median.png)

These 2 plots showed counties with the highest population per store and largest county land area per store (denoted by size of circles). However, the best performing counties (darkest blue) were not the largest circles.

![area_vs_median_sales]({{site-url}}/images/blob_area_vs_median.png)

#### Skewed sales data

![skewed_sales]({{site-url}}/images/total_sales_hist_skewed.png)

This poorly charted histogram for sales per store in 2015 (shown above), was included to demonstrate the extreme skewness of the sales per store data. There were extreme outliers with annual revenue of about \$ 9 million, contrast this with the majority of the stores raking in only \$ 200,000 in 2015.

With this in mind, median (instead of mean) was used to calculate average to better represent the data.

### 5. Locations recommendation

We examined all 99 counties across the state of IOWA and selected 2 counties with 2 very different expansion strategy.

#### a. Large-scale expansion

For a large scale expansion (annual target revenue of up to \$\ 9 million per store), which requires access to a large market base, we recommended Polk County for the following reasons:

   - Top county for total sales, i.e. largest market
   - Top county for total bottle sold per store
   - Top county for total number of transactions
   - 54th percentile for average bottle price and average bottle profit
   - Recommended average price per bottle ~ \$19
   - Recommended annual bottle sold ~ 500,000 bottles
   
![top_sales]({{site-url}}/images/top10_counties_total_sales_bar.png)

The chart above showed that Polk county, in 2015, was the overwhelming leader in terms of sales volume. Linn county, the second highest, came in at less than halve of the total sales in Polk. For a retailer looking to gain market share and establish a presence quickly, Polk would be the ideal county.

![top_sales_median_per_store]({{site-url}}/images/top10_counties_total_sales_bar.png)

#### b. Average expansion

For an average store expansion plan (annual target revenue of ~ \$200k), Winneshiek was recommended for the following reasons:

   - Top county for average sale per store
   - Top county for average bottle sold per store
   - 97th percentile for average bottle price
   - 97th percentile for average bottle profit
   - 68th percentile for average number of transactions per store
   - 46th percentile for mean bottles per transaction
   - Recommended average price per bottle ~ $15.50
   - Recommended annual bottles sold ~ 21,000 bottles
   

2) Predictive modelling using linear regression

Predictive models were created for both the approaches to locations. Models were created using historical per store data from the respective counties using the following features:

   - Median Bottle Cost per store
   - Median Bottle Price per store
   - Median Bottle Profit per store
   - Sum Bottles sold per store
   - Sum Volume sold per store
   - No. of transactions per store

2015 Q1 data was used as the training set and 2016 Q1 data was used as the test set. Ridge regularization and cross-validation were conducted as part of the fitting process. Alpha chosen by Ridge algorithm was 0.2 for regularization and was minimally effective in reducing RMSE. Model performance was considered mediocre with high r^2 (~ 0.95) but relatively high Root Mean Squared Error. Coefficients of the models were also significant.

Suggestions to improve the model include:

   - Scaling the features
   - Use of richer dataset for advanced feature engineering, such as the aforementioned demographics data
   - Using logistical regression to take into account locations of individual stores within the county
   - Include more historical data for time-series analyses of market trends over the years

In conclusion, this a much more complex project which involves heavy data munging and dataframe manipulations. Predictive modelling was limited in scope, primary due to the restriction to linear regression. 
