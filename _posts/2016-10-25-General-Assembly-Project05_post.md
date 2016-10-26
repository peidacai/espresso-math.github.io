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

### 3. Exploratory Data Analysis

![titanic_pairplot]({{site-url}}/images/titanic_pairplot.png)

![violinplot]({{site-url}}/images/violinplot_sex_age.png)

![swarmplot]({{site-url}}/images/swarmplot_class_age.png)

- Conversion to appropriate types (integer, float and string)

- Filling empty cells, mostly using information from other similar cells within the same column. For example, over 10,000 cells had empty "County" cells. This was easily resolved by using the corresponding non-empty "City" cell as a key to find the "County" entry for another row with same "City" entry. This was possible since "City" is a subset category of "County" so cells with same "City" entry, would have same "County" entry.

- Correcting cells with wrong value. Examples include zip code of '712-2', when it should have been a 5 digit format. This was corrected manually.

- Finally, an wrong zip code was discovered when using Tableau for EDA. Turned out "52601" was wrongly entered as "56201", resulting in a store that was not in IOWA state.

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

This poorly charted histogram for sales per store in 2015 (shown above), was deliberately included to demonstrate the extreme skewness of the sales per store data. There were extreme outliers with annual revenue of about \$9 million.

With this in mind, median (instead of mean) was used to calculate average to better represent the data.

### 5. Locations, volume and price targets recommendation

We examined all 99 counties across the state of IOWA and selected 2 counties with 2 very different expansion strategies.

#### a. Large-scale expansion

For a large scale expansion (annual target revenue of up to \$9 million per store), which requires access to a large market base, we recommended Polk County for the following reasons:

   - Top county for total sales, i.e. largest market
   - Top county for total bottle sold per store
   - Top county for total number of transactions
   - 54th percentile for average bottle price and average bottle profit
   - Recommended average price per bottle ~ \$19
   - Recommended annual bottle sold ~ 500,000 bottles
   
![top_sales]({{site-url}}/images/top10_counties_total_sales_bar.png)

The chart above showed that Polk county, in 2015, was the overwhelming leader in terms of sales volume. Linn county, the second highest, came in at less than halve of the total sales in Polk. For a retailer looking to gain market share and establish a presence quickly, Polk would be the ideal county.

#### b. Average expansion

![top_sales_median_per_store]({{site-url}}/images/blob_top_median.png)

For an average store expansion plan (annual target revenue of ~ \$200k), Winneshiek was recommended for the following reasons:

   - Top county for average sale per store
   - Top county for average bottle sold per store
   - 97th percentile for average bottle price
   - 97th percentile for average bottle profit
   - 68th percentile for average number of transactions per store
   - 46th percentile for mean bottles per transaction
   - Recommended average price per bottle ~ $15.50
   - Recommended annual bottles sold ~ 21,000 bottles

The 68th and 46th percentile rankings for average number of transactions per store and mean bottles per transaction were considered to be desirable. These were indicators of workload per store and for a mediocre workload, stores in Winneshiek had higher sales amount per transaction, indicating customer preferences for more expensive products.

2) Predictive modelling using linear regression

Predictive models were created for both the approaches to locations. Models were created using historical per store data from the respective counties using the following features:

   - Median Bottle Cost per store
   - Median Bottle Price per store
   - Median Bottle Profit per store
   - Sum Bottles sold per store
   - Sum Volume sold per store
   - No. of transactions per store

### Regression model performance for Polk

![polk_reg_performance]({{site-url}}/images/reg_15Q1_pred_16Q1_polk.png)

### Regression model performance for Winneshiek

![winneshiek_reg_performance]({{site-url}}/images/reg_15Q1_pred_16Q1.png)

2015 Q1 data was used as the training set and 2016 Q1 data was used as the test set. Ridge regularization and cross-validation were conducted as part of the fitting process. Alpha chosen by Ridge algorithm was 0.2 for regularization and was minimally effective in reducing RMSE. Model performance was considered mediocre with high r^2 (~ 0.95) but relatively high Root Mean Squared Error. Coefficients of the models were also significant.

Regresion for Winneshiek was based on sparse data, due to there being only 5 stores in Winneshiek in 2015 and 2016.

Suggestions to improve the models include:

   - Scaling the features
   - Use of richer dataset for advanced feature engineering, such as the aforementioned demographics data
   - Using logistical regression to take into account locations of individual stores within the county
   - Include more historical data for time-series analyses of market trends over the years

### 6. Conclusion

This was a much more complex project, compared to earlier ones. The complexity was mainly due to the larger dataset, heavy data munging requirements and dataframe manipulations.

Predictive modelling was limited in scope, primary due to the restriction to using linear regression.

Finally, I would like to discuss a little about the main challenge I faced in completing this project. When first given the project, the problem was seemingly straight forward: predicting liquor sales of a county based on a bunch of historical data. However, as I worked through the steps of analysis, the modelling portion became more confusing. I found it challenging to wrap my mind around the fact that we were trying to predict sales of a location, when we were already given transactional data to "fit" into the model. Total sales could be derived by taking "quantity sold" x "average price sold".

I think the main takeway for me was the thought process I went through when trying to overcome this confusion. The one thing which stood out to me was to have multiple sources of data. I would imagine, if asked to solve this problem for an actual company, I would spend the bulk of my time framing the problem and sourcing for data to supplement the analysis. Without transactional data, clever correlational data sources are required to make an "educated guess" of the sales figures (I was told university enrolment rates correlates strongly with liquor sales).
