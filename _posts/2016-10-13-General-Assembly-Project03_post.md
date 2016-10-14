---
layout: post-no-feature
title: "GA Project03"
description: "Predicting IOWA liquor sales with multivariate linear regression"
comments: true
category: articles
mathjax: true
---

![whiskey]({{site-url}}/images/whiskey.png)

### Project03: Data Science for Business Intelligence: Predicting IOWA liquor sales

### 1. Introduction

In this project, the task was to conduct market research through the lens of a data scientist, and recommend locations for a new liquor store in IOWA state. 2015 and 2016 Q1 statewide liquor store transactional data was provided for the researchand there were 3 sub-tasks:

   - Identify suitable locations for a liquor store owner's expansion plans
   - Create a model to predict future sales
   - Recommend target for bottles sold and average bottle price

### 2. Data exploration

The data provided had the following characteristics:

Columns: 18
Rows: 2,709,551

Each row corresponded to a single transaction in IOWA. Some of the more prominent features of each entry included: county, city, zip code, item description, price per bottle, cost per bottle, bottle sold and sales (or revenue).

### 3. Data munging

All columns were munged and cleaned. Cleaning process included:

- Conversion to appropriate types (integer, float and string)
- Filling empty cells, mostly using information from other similar cells within the same column. For example, over 10,000 cells had empty "County" cells. This was easily resolved by using the corresponding non-empty "City" cell as a key to find the "County" entry for another row with same "City" entry. This was possible since "City" is a subset category of "County" so cells with same "City" entry, would have same "County" entry.
- Correcting cells with wrong value. Examples include zip code of '712-2', when it should have been a 5 digit format. This was corrected manually.



### 4. Data mining

After cleaning the data, county demographic data (dated 2010) were used to supplement the existing dataset in search of a richer analysis. However, without detailed demographic data (such as age groups, gender, income range, etc), using simple land area per store and population per store yielded little value.

1) Location recommendation

We examined all 99 counties across the state of IOWA and selected 2 counties as possible locations depending on the client's requirement.

For a typical store expansion plan (annual revenue of ~ \$200k), Winneshiek was recommended for the following reasons:
   - Top county for average sale per store
   - Top county for average bottle sold per store
   - 97th percentile for average bottle price
   - 97th percentile for average bottle profit
   - 68th percentile for average number of transactions per store
   - 46th percentile for mean bottles per transaction
   - Recommended average price per bottle ~ \$15.50
   - Recommended annual bottles sold ~ 21,000 bottles
   
For a large scale expansion (annual revenue of ~ \$9mil per store), which requires access to a large market base, we recommended Polk County for the following reasons:

   - Top county for total sales, i.e. largest market
   - Top county for total bottle sold per store
   - Top county for total number of transactions
   - 54th percentile for average bottle price and average bottle profit
   - Recommended average price per bottle ~ \$19
   - Recommended annual bottle sold ~ 500,000 bottles

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
