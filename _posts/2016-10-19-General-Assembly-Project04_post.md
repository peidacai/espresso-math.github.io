---
layout: post-no-feature
title: "General Assembly (NYC) Weekly Project 04"
description: "Web scrape and predict Data Scientists' salaries"
comments: true
category: articles
mathjax: true
quote: “Courage is almost a contradiction in terms. It means a strong desire to live taking the form of a readiness to die.” ― G.K. Chesterton
---

![data_science]({{site-url}}/images/data-science.png)

*Image source: [http://www.lix.polytechnique.fr](http://www.lix.polytechnique.fr)

### Project04: Web Scrape & Predict

### 1. Introduction

In this project, the task was to web scrape career sites for salaries of data scientist job offerings and using data obtained to build a predictive model to predict data scientists' salaries:

   - Web scrape from chosen websites
   - Clean data
   - Build classifier model to predict data scientists' salaries

This blog post will be broken down into a non-technical and technical portion.

## Non-Technical

The approach to this project was to predict if the salary of a single Data Scientist job listing in USA would be higher or lower than the median of $85,000 a year. The following factors were taken into consideration:

   - Job Title
   - Location
   - Company name
   - Company ratings
   - Number of reviews
   - Job Summary

The accuracy of the predictive model was about 78.4%, the top 4 most important factors in the model were:

   1. Job Summary (the word "looking")
   2. Job Title (the word "quantitative")
   3. Job Title (the word "data")
   4. New York State

The 4th factor was included as it was the top "non-word" factor which determined the model.

The model was further tuned to minimise occurences (close to zero chance) of predicting a high salary when actual salary is low. However, this was at the expense of higher error rates (about 1 in 2) at predicting low salary.

## Technical

### 2. Web Scraping

Salaries data was scraped from [www.indeed.com](www.indeed.com). Some pre-project research was conducted on the best cities for data scientists in the USA and a list of 246 cities was scraped from a separate [site](https://www.goodcall.com/data-center/best-places-data-scientists) as the basis to search for salaries. Search results was restricted to 300 per city and the following information was extracted:

![screendump_from_indeed]({{site-url}}/images/indeed_screen_dump.png)

### 3. Data exploration

The data scapped had the following characteristics:

- Number of cities scraped: 246
- Maximum number of listings per city: 300
- Total job listings scrapped : 60,294
- Total job listings (less duplicates): 11,311
- Total job listings with monthly/yearly salaries: 571
- Remaining listings after accounting for outliers: 545
- Minimum annual salary (w/ outlier): \$400
- Minimum annual salary (w/o outlier; self-imposed), \$40,000
- Maximum annual salary: \$250,000

### 3. Data munging (Use of REGEX)

This time round, data cleaning process was conducted in 2 areas.

#### a. Cleaning while scrapping

Since data was obtained using web scraping technique (python's: "requests" and "beautifulsoup" since indeed.com does not use AJAX, in which case SELENIUM would be used instead), much of the cleaning process was conducted while scrapping, before writing data into a pandas dataframe. As much as was possible, the ".text" function was used to extract only relevant portions of the html code. 

#### b. Cleaning within dataframe

Besides the usual cleaning process (converting to appropriate types, stripping spaces, checking for correct state entries, etc), the stand out this time was the use of python's Regular Expressions. It is a very powerful language which allowed for the extraction of strings which conformed to a specific format for example, to extract "54.4px", "re.findall( r'(\d+\.\d)', x, re.I)" was used. And since web scraping really involves obtaining a bunch of text from websites, REGEX proved to be an invaluable tools to help with data cleaning and extraction of pertinent information.

```py
# Extract ratings from string

def reg_rate(x):
    matchObj = re.findall( r'(\d+\.\d)', x, re.I)
    if matchObj:
        return float(matchObj[0])
    else:
        return 0.
```

### 4. Data mining

#### Top 10 States with highest median salaries for Data Scientists

![state_salary]({{site-url}}/images/boxplot_salary.png)

   - New York state had the highest median salary \$120,000
   - Pennsylvania had the highest maximum salary, \$250,000.

### 5. Modelling

A logistic regression was used for predicting, together with a gridsearchCV module to find the best parameters for the model. Company ratings and number of reviews were normalized while the rest of the features were categorical. Sklearn's count vectorizer was also used to extract the top 20 most popular words used in Job titles and job summaries.

Best estimator parameters:

   - C = 0.33
   - Penalty - L1 (LASSO regularization)
    
Most important features:

Words like "Data", "Scientist", "Quantitative" rank high in the weightage of coefficients. However, the count vectorizer, even after account for English "stop words", still needed to be tuned for specific stop words. Case in point: the most important feature was the word "looking" in job summary.

Interesting feature to note:

#### "Number of reviews"

Coefficient was zero, i.e. this had no impact on the predictions.

#### "Company Ratings"

Coefficient was -0.77, meaning this was negatively correlated with salary. Interesting as on first inspection, one would assume a positive relationship between company rating and salary. However, it can also be argued that highly rated companies are more popular and in demand, hence are natural price-setters when it comes to "buying the resource", therefore, are able to still get talents with lower salary.

![ROC_for_salaries]({{site-url}}/images/roc_logreg.png)

#### Assumption

   - "Data scientist" search in indeed.com returns data scientist jobs. While only strictly jobs with title "data scientist" could be kept, the role of data scientst is expanding and the definition of data scientist also getting fuzzed, therefore, it was assumed that the search algorithm of indeed.com took care of this and returned only data-scientist related jobs.
   
#### Tuning to minimise false positive rate

In order to reduce false positive rates, the threshold probability can be increased. However, this is done at the expense of lower True positive rates - poorer recall. While one can argue that this helps with expectation management, clients would be "surprise" when actual pay was higher than promised, the low salary predicted may discourage high potential talents to apply for such jobs, resulting in a mismatch in required skillsets and talents.

Therefore, tuning the threshold parameter should be done while being cognisant of such unintended consequences. An equation can be formed with expected loss of talents (in dollar value) with the increase in false negative rates to determine the best "compromise" between minimising false positive rates and managing the resulting increase in false negative rates.

### 6. Conclusion and parting thoughts

In this project, webscraping and logistic regression were covered in depth and used. The biggest challenge was actually the webscrapping portion as it required some knowledge of html syntax and the use of SELENIUM, python requests and BeautifulSoup. Then it was a matter of figuring out how each feature was labeled on the website and extract them to a dataframe.

The modeling and model tuning portion were comparatively more manageable. Some future work could be to include results from other websites, scraping and merging them would be the main challenges. Another could be to model the data using different algorithm such as KNN, random forest, Naive Bayes, etc, then comparing the performances across, to select the best model.

There were some important lessons learnt:

   a. When scraping from search results, to optimise the search process, consider performing a mini-scrape to put the number of search results per city into a list (of tuples or dataframe). The number of results would then be known and the actual scrapping would consequently be more efficient. Some cities had less than 100 results while others like San Francisco had over 1,900 results.
    
   b. After scraping the results, drop duplicates needs to be performed.
    
   c. Always save scraped results to a csv file immediately after scraping. This would be the equivalent of an initial raw dataset.
