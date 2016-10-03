---
layout: post-no-feature
title: "GA Project02"
description: "Exploratory data analysis using Python on Year 2000 Billboard Top 100 chart"
comments: true
category: articles
---

##### *General Assembly (NYC) Data Science Immersive Bootcamp Sep-Dec 2016*

### Project02: 2000 Billboard Top 100 songs

### 1. Introduction

We were provided with a dataset consisting of 2000 Billboard chart Top 100 songs. The data set consists of 317 rows with 83 columns, each row gives details of each song including:
- artist
- track name
- genre
- weekly positions on Top 100 (from first entered to finally exiting)
- date peaked

### 2. Problem statement(s)

My approach to creating problem statements is to adopt an industry-profitability view. To put myself in the shoes of someone in the music industry who are looking for factors of songs that maximises profitability. 

With this as a basis, and since this is already a Top100 dataset, the problem identification portion can be distilled to two main factors:

1. Position on chart

2. Duration on chart

###__"We want Top100 songs that stay high on the charts for a long time!"__

#### a. Hypotheses:

1. Songs that entered Top100 during autumn months (Sep, Oct and Nov) stayed in Top100 longer.
2. Songs that peaked quickly had lower average weekly positions.

#### b. Assumptions and risks:

- All given data is accurate.
   
   - The source and how accurate the data was reproduced need to be verified. Typically, in such situation, it may be wise to compare the sample to a "gold standard" for accuracy, if available, especially for data from an older time period, when human was part of the data-entry process.

- All given data is comprehensive and accurately reflects the scene of music industry in 2000.

   - Billboard charts took sales + radio airplay numbers to determine the Top100 position in 2000 (paid digital download became a feature only after [2005](https://en.wikipedia.org/wiki/Billboard_charts). The assumption is that these 2 features captured the popularity of songs in entirety, even though digital formats (mp3) were already widely available then. The risk then, is that, the positions did not completely represent the popularity of the songs.

### 3. Data munging

The data was read into a python dataframe and some degree of cleaning was required in the following areas:

a. Date fields

   The data read into the dataframe were of "string" format, so all date fields were changed to pandas datetime format to facilitate further data wrangling.
    
b. Genre

   Some genre fields were wrongly labeled, specifically "R&B" had 2 variants (other being "R & B"). Such discrepancies were caught and corrected. Genres were quite lopsided in that over 33% of the data was classified as "Rock" and 6 out of total of 11 genres had less than 10 data points in them. Hence, genre was not examined in detail as a feature.
    
c. Song duration

   When pandas read in the raw csv data, song duration turned out to be of the format "3,38,00 AM". A customised function was written for parsing, to separate minutes, seconds and milliseconds before recombining them into seconds "218.0".

d. Weekly positions

   There were two tasks for this field: (1) Convert "string" format to "float"; (2) however, weeks without positions had "*" in them, so were changed to Numpy "NaN" instead (for ease of subsequent operations), so this was actually performed first.
   
e. Artists

   This feature would have been a good feature for analysis, similar to genre, data here is also very lopsided, the most number of songs an artist had on the chart was 5. Most artists only had 1 song on the charts, so using "artist" as a feature may yield unreliable results.

### 4. Creating new features

The next step was to create new features to aid analyses.

a. Extracting month from "date_entered" and binning them into seasons

   In order to answer the question on songs entering the charts in autumn, the month field was extracted from the date_entered field and binned into the 4 seasons.

b. Time on chart and time to peak

   The duration taken for a song to reach its peak position was created using date_peaked - date_entered. To consider this as a proportion, this value was divided by the total time the song stayed on the charts. This was further distilled into songs that peaked early (taking less than half the time to reach its peak) and songs that peaked late.
    
c. Average position
    
   While the positions on chart provide an indication of the performance of each song, it provided only a snapshot view of performance. A summation of positions could help provide a more wholesome performance view, nevertheless, this too, does not tkae into consideration the duration of stay on chart.
    
   Therefore, a more encompassing feature would be average position, taking the sum of all positions and divided by the number of weeks on chart.
    
   The data provided included all songs that were present on the charts during the calendar year of 2000. This meant that it included songs which entered in 1999 and continued into 2000, similarly songs which continued into 2001, as well as songs which left and re-entered. The use of average position accounts for these special circumstances and therefore, featured heavily in the subsequent analyses.


### 5. Univariate analyses

![Histogram_time_on_chart]({{site-url}}/images/hist_time_on_chart.png)

#### Comments:

An overwhelming number of songs stayed on the chart for about 20 weeks. However, the distribution is rather positively skewed. More songs stayed less than 20 weeks compared to longer-lived "unicorns".

![Histogram_avg_position]({{site-url}}/images/hist_avg_position.png)

#### Comments:

Most songs had average positions near 100. Most songs clustered between 60th and 100th positions. There was a small "unicorn" cluster from 10th to 30th position.

The high number of songs at the 100 position should represent "above average" songs which just made it to the Top 100 but couldn't quite stay there before they were dropped altogether.

### 6. Bivariate Analyses

![Scatter_pos_time]({{site-url}}/images/scatter_pos_vs_time.png)

#### Comments:

This chart showed clearly that there were 2 distinct groups of songs: below 40 and above 40 in average weekly position.

- Must stay below 40th average position

   In the group with average position above 40, we see clearly that songs that fell within this group almost certainly did not stay on the chart for more than 20 weekly (with the exception of a few outliers). Even for those that made it past 20 weeks, the longest any song stayed was about 30 weeks.
    

- Lowest average position = best songs?

   When we observe the density of the blue dots, we note the clear median line at 20 weeks on chart (dark blue area, i.e. more songs overlap here). In the upper left quadrant, we note a small group of song cluster around average position of 20 but only stayed on the charts for about 40 weeks.
    

- So, where are the unicorns?

   The most interesting phenomenon is that songs with the best average weekly positions were not the ones which stayed on the charts the longest. We need to examine this in greater detail in the multivariate analyses later, but this indicates that there was a sweet spot of hitting average weekly position of between 20 and 40, where there was a chance of becoming "unicorns" that last more than a year on the charts.

![Boxplot_peak_time]({{site-url}}/images/boxplot_time_vs_peak.png)

#### Comments:

The proportionate time taken to peak was binned into 2 categories "Peaked_early" and "Peaked_late". Songs which took less than half of the time they were on the charts to reach their peak were binned as "Peaked_early" while the rest were binned as "Peaked_late".

Songs which peaked proportionately earlier had a higher variance of stay duration on the charts, nevertheless, they have lower median duration on charts and also a lower high compared to songs which peaked late.

### 7. Multivariate Analyses

![scatter_pos_vs_time_vs_mth]({{site-url}}/images/scatter_pos_vs_time_vs_mth.png)

![scatter_pos_vs_time_vs_peak]({{site-url}}/images/scatter_pos_vs_time_vs_peak.png)

#### Comments:

There seemed to be no discernable pattern in the plots, particularly when we zoomed into the top left quadrant (where the supposedly unicorn songs reside). There is no clear dominant color in either plot in the said area, even after zooming out and examining the plots in their entirety.

A possible solution could be to get more data, data from years before and after. However, I highly suspect that it would yield much more information than it already has now, primarily because of the intuition that timing the entry to Top 100 charts or adjusting the time to peak (if that is even possible) just don't seem like plausible methods of pushing a song to becoming a unicorn.

### 8. Statistical analyses

### 9. Side-Quests

#### a. Explore effects of song duration

While song duration didn't feature in the original 2 problem statements, this was the only independent feature which we could use to investigate the data further (others being genre and artist, which we had earlier anticipated to generate unreliable results). Therefore, some time was devoted to a "side-quest" to investigate this feature further, in the hope that it could help unlock the factors to finding a unicorn song.

![unicorn_time]({{site-url}}/images/unicorn_vs_time_on_chart.png)

![unicorn_pos]({{site-url}}/images/unicorn_vs_weekly_position.png)

#### b. Search for the unicorns


### 10. Conclusion

We started with 2 hypotheses about the dataset:

1. Songs that entered Top100 during autumn months (Sep, Oct and Nov) stayed in Top100 longer.
2. Songs that peaked quickly had lower average weekly positions.

We then proceeded to load the data and clean the dataset, conduct sense-checking, added new features and filter out the features which we were interested in.

Once the data was adequately cleaned, univariate, bivariate and multivariate analyses were conducted via the use of a series of visualization charts.

Finally, we conducted statistical tests on both our hypotheses and found the following:

1. Songs that entered Top100 during autumn months (Sep, Oct and Nov) stayed in Top100 longer (over 90% confidence).
2. Songs that peaked quickly had lower average weekly positions (over 99% confidence).

While conducting the analyses, we side-tracked a little to figure out the unicorn songs in 2000 which satisfied our criteria of __staying high__ on the Top100 charts for a __long time__.

### Parting thoughts







     
