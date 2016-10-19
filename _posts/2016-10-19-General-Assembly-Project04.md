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

The data scapped had the following characteristics:

- Number of cities scrapped: 246
- Maximum number of listings per city: 300
- Total job listings scrapped : 60,294
- Total job listings (less duplicates): 11,311
- Total job listings with monthly/yearly salaries: 571
- Remaining listings after accounting for outliers: 545
- Minimum annual salary (w/ outlier, w/o outlier): $400, $40,000
- Maximum annual salary: $250,000

### 3. Data munging (Use of REGEX)

This time round, data cleaning process was conducted in 2 areas.

#### a. Cleaning while scrapping

Since data was obtained using web scraping technique (python's: "requests" and "beautifulsoup" since indeed.com does not use AJAX, in which case SELENIUM would be used instead), much of the cleaning process was conducted while scrapping, before writing data into a pandas dataframe. As much as was possible, the ".text" function was used to extra only relevant portions of the html code. 

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

New York state has the highest median salary ($120,000) but Pennsylvania had the highest maximum salary, $250,000.

### 5. Modelling



#### a. Assumption

   - "Data scientist" search in indeed.com returns data scientist jobs. While only strictly jobs with title "data scientist" could be kept, the role of data scientst is expanding and the definition of data scientist also getting fuzzed, therefore, it was assumed that the search algorithm of indeed.com took care of this and returned only data-scientist related jobs.

### 6. Conclusion


