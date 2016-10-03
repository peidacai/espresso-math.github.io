---
layout: post-no-feature
title: "GA Project02"
description: "Exploratory data analysis using Python on Year 2000 Billboard Top 100 chart"
comments: true
category: articles
---

## General Assembly (NYC) Data Science Immersive Bootcamp Sep-Dec 2016

## Project02: 2000 Billboard Top 100 songs

## 1. Introduction

We were provided with a dataset consisting of 2000 Billboard chart Top 100 songs. The data set consists of 317 rows with 83 columns, each row gives details of each song including:
- artist
- track name
- genre
- weekly positions on Top 100 (from first entered to finally exiting)
- date peaked

#### Required tasks:

- Include a problem statement
- State the risks and assumptions of the data
- Import data using the Pandas library
- Perform exploratory data analysis
- Use Tableau and/or Python plotting modules to visualize data
- Observe correlations in the data
- Evaluate a hypothesis

## 2. Problem statement(s)

My approach to creating problem statements is to adopt an industry-profitability view. To put myself in the shoes of someone in the music industry who are looking for factors of songs that maximises profitability. 

With this as a basis, and since this is already a Top100 dataset, the problem identification portion can be distilled to two main factors:

1. Position on chart

2. Duration on chart

$$We\ want\ Top100\ songs\ that\ stay\ high\ on\ the\ charts\ for\ a\ long\ time!$$

### a. Hypotheses:

1. Songs that entered Top100 during autumn months (Sep, Oct and Nov) stayed in Top100 longer.
2. Songs that peaked quickly had lower average weekly positions.

### b. Assumptions and risks:

- All given data is accurate.
   
   - The source and how accurate the data was reproduced need to be verified. Typically, in such situation, it may be wise to compare the sample to a "gold standard" for accuracy, if available, especially for data from an older time period, when human was part of the data-entry process.

- All given data is comprehensive and accurately reflects the scene of music industry in 2000.

   - Billboard charts took sales + radio airplay numbers to determine the Top100 position in 2000 (paid digital download became a feature only after [2005](https://en.wikipedia.org/wiki/Billboard_charts). The assumption is that these 2 features captured the popularity of songs in entirety, even though digital formats (mp3) were already widely available then. The risk then, is that, the positions did not completely represent the popularity of the songs.

## 3. Data munging

The data was read into a python dataframe and some degree of cleaning was required in the following areas:

a. Date fields

    The data read into the dataframe were of "string" format, so all date fields were changed to pandas datetime format to facilitate further data wrangling.
    
b. Genre

    Some genre fields were wrongly labeled, specifically "R&B" had 2 variants (other being "R & B"). Such discrepancies were caught and corrected.
    
c. Song duration

    When pandas read in the raw csv data, song duration turned out to be of the format "3,38,00 AM". A customised function was written for parsing, to separate minutes, seconds and milliseconds before recombining them into seconds "218.0".

d. Weekly positions

    There were two tasks for this field: (1) Convert "string" format to "float"; (2) however, weeks without positions had "*" in them, so were changed to Numpy "NaN" instead (for ease of subsequent operations), so this was actually performed first.

## 4. Creating new features

The next step was to create new features to aid analyses.

a. Extracting month from "date_entered" and binning them into seasons

    In order to answer the question on songs entering the charts in autumn, the month field was extracted from the date_entered field and binned into the 4 seasons.

b. Time on chart and time to peak

    The duration taken for a song to reach its peak position was created using date_peaked - date_entered. To consider this as a proportion, this value was divided by the total time the song stayed on the charts. This was further distilled into songs that peaked early (taking less than half the time to reach its peak) and songs that peaked late.
    
c. Average position
    
    While the positions on chart provide an indication of the performance of each song, it provided only a snapshot view of performance. A summation of positions could help provide a more wholesome performance view, nevertheless, this too, does not tkae into consideration the duration of stay on chart.
    
    Therefore, a more encompassing feature would be average position, taking the sum of all positions and divided by the number of weeks on chart.
    
    The data provided included all songs that were present on the charts during the calendar year of 2000. This meant that it included songs which entered in 1999 and continued into 2000, similarly songs which continued into 2001, as well as songs which left and re-entered. The use of average position accounts for these special circumstances and therefore, featured heavily in the subsequent analyses.


## 5. Univariate analyses

![Histogram_time_on_chart]({{site-url}}/images/hist_time_on_chart.png)

## 6. Bivariate Analyses

## 7. Multivariate Analyses

## 8. Statistical analyses

## 9. Conclusion





#### Deploy and validate

Not applicable for this project.

#### Project conclusion and potential future work

- Project conclusion



#### Parting thoughts







     
