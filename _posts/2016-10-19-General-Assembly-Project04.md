---
layout: post-no-feature
title: "GA Project04"
description: "Web scrape and predict Data Scientists' salaries"
comments: true
category: articles
mathjax: true
---

<sup>![data_science]({{site-url}}/images/data-science.png)</sup>

*Image was source from [http://www.lix.polytechnique.fr](http://www.lix.polytechnique.fr)*


### Project04: Web Scrape & Predict

### 1. Introduction

In this project, the task was to web scrape career sites for salaries of data scientist job offerings and using data obtained to build a predictive model to predict data scientists' salaries:

   - Web scrape from chosen websites
   - Clean data
   - Build classifier model to predict data scientists' salaries

### 2. Web Scraping

Salaries data was scraped from [www.indeed.com](www.indeed.com). Some pre-project research was conducted on the best cities for data scientists in the USA and a list of 246 cities was scraped from a separate [site](https://www.goodcall.com/data-center/best-places-data-scientists) as the basis to search for salaries. Search results was restricted to 300 per city and the following information was extracted:

   - Job Title
   - Location
   - Salary
   - Company name
   - Company ratings
   - Number of reviews
   - Job Summary

![screendump_from_indeed]({{site-url}}/images/indeed_screen_dump.png)

### 3. Data exploration

The data provided had the following characteristics:

- Filesize: over 330 MB (csv)
- Columns: 18
- Rows: 2,709,551

Each row corresponded to a single transaction in IOWA. Some of the more prominent features of each entry included: county, city, zip code, item description, price per bottle, cost per bottle, bottle sold and sales (or revenue).

### 3. Data munging (Use of REGEX)

This time round, data cleaning process was conducted in 2 areas.

#### a. Cleaning while scrapping

Since data was obtained using web scraping technique (python's: "requests" and "beautifulsoup" since indeed.com does not use AJAX, in which case SELENIUM would be used instead), much of the cleaning process was conducted while scrapping, before writing data into a pandas dataframe. As much as was possible, the ".text" function was used to extra only relevant portions of the html code. 

#### b. Cleaning within dataframe

Besides the usual cleaning process (converting to appropriate types, stripping spaces, checking for correct state entries, etc), the stand out this time was the use of python's Regular Expressions. It is a very powerful language which allowed for the extraction of strings which conformed to a specific format for example, to extract "54.4px", "re.findall( r'(\d+.\d)', x, re.I)" was used. And since web scraping really involves obtaining a bunch of text from websites, REGEX proved to be an invaluable tools to help with data cleaning and extraction of pertinent information.

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

This poorly charted histogram for sales per store in 2015 (shown above), was included to demonstrate the extreme skewness of the sales per store data. There were extreme outliers with annual revenue of about \$9 million.
[
With this in mind, median (instead of mean) was used to calculate average to better represent the data.

### 5. Creation of models

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
