---
layout: post-no-feature
title: "GA Project02"
description: "Exploratory data analysis using Python on Year 2000 Billboard Top 100 chart"
comments: true
category: articles
---

### Project02: 2000 Billboard Top 100 songs

### 1. Introduction

A dataset consisting of Billboard Top 100 songs in year 2000 was provided, consisting of 317 rows with 83 columns, each row provided details of each song including:

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

__"Top100 songs that stay high on the charts for a long time = Good"__

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

a. Date fields - Converted "string" format to pandas datetime format to facilitate further data wrangling.
    
b. Genre - Combined "R&B" and "R & B" as one. Further found genres, as a feature to be quite lopsided, with over 33% "Rock" and 6 out of total of 11 genres had less than 10 songs per genre. Hence, genre was not examined in detail as a feature.
    
c. Song duration - Original string format of "3,38,00 AM" was cleaned using a customised function into seconds, i.e. "218.0".

d. Weekly positions - Converted "string" format to "float"; replaced "\*" with numpy "NaN"
   
e. Artists - Similar to genre, even more lopsided. Most artists only had 1 song on the charts, artist with highest count had 5.

### 4. Creating new features

The next step was to create new features to aid analyses.

a. Extracting month from "date_entered" and binning them into 4 seasons

b. Time on chart - Duration on chart was extracted (date_peaked - date_entered). 

c. Time to peak (binned) - (Weeks to peak /duration). Separated into 2 bins: (1) peaked early (took less than half the time on chart to reach peak); (2) and songs that peaked late.
    
d. Average position - Weekly positions and sum of all positions could not account for duration on chart. A more encompassing feature, average position, was used heavily for performance analyses.
    
   The data included all songs that were present on the charts during the calendar year of 2000. This meant that it included songs which entered in 1999 and continued into 2000, similarly songs which continued into 2001, as well as songs which left and re-entered. The use of average position would account for these special circumstances.


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

   In the group with average position above 40, it was clear that songs that fell within this group almost certainly did not stay on the chart for more than 20 weekly (with the exception of a few outliers). Even for those that made it past 20 weeks, the longest any song stayed was about 30 weeks.
    

- Lowest average position = best songs?

   There was a clear median line at 20 weeks on chart (dark blue area, i.e. more songs overlap here). In the upper left quadrant, there was also a small group of song cluster around average position of 20 but only stayed on the charts for about 40 weeks.
    

- So, where are the unicorns?

   The most interesting phenomenon was that songs with the best average weekly positions were not the ones which stayed on the charts the longest. This indicated that there was a sweet spot of hitting average weekly position of between 20 and 40, where there was a chance of becoming "unicorns" that last more than a year on the charts.

![Boxplot_peak_time]({{site-url}}/images/boxplot_time_vs_peak.png)

#### Comments:

Songs which peaked proportionately earlier had a higher variance of stay duration on the charts, nevertheless, they had lower median duration on charts and also a lower high compared to songs which peaked late.

### 7. Multivariate Analyses

![scatter_pos_vs_time_vs_mth]({{site-url}}/images/scatter_pos_vs_time_vs_mth.png)

![scatter_pos_vs_time_vs_peak]({{site-url}}/images/scatter_pos_vs_time_vs_peak.png)

#### Comments:

There seemed to be no discernable pattern in the plots, particularly in the top left quadrant (where the supposedly unicorn songs reside). There was no clear dominant color in either plot in the said area or otherwise.

A possible solution could be to get more data, data from years before and after. However, I highly doubt that it would yield much more information than it already has now, primarily because of the intuition that timing the entry to Top 100 charts or adjusting the time to peak (if that is even possible) just don't seem like plausible methods of pushing a song to becoming a unicorn.

### 8. Statistical analyses

Normality checks were conducted for both the average_position and time_on_chart distributions and both were shown to be highly unlikely normally distributed. Therefore, the resampling-shuffle test was conducted on both distribution, with 10,000 trials of random shuffling of labels.

- The mean number of weeks on chart difference between autumn songs and other songs was about 1.76 weeks, with corresponding p-value of 0.0712, statistically significant at alpha = 10%.

- The difference in average position between songs which peaked late and peaked early was 9.84 positions, with corresponding p-value of 0.0003, statistically significant at alpha = 1%.

### 9. Side-Quests

#### a. Explore effects of song duration

While song duration didn't feature in the original 2 problem statements, this was the only independent feature which could be used to investigate the data further (others being genre and artist, which were discredited earlier). Therefore, some time was devoted to a "side-quest" to investigate this feature further, in the hope that it could help unlock the factors to finding a unicorn song.

![unicorn_time]({{site-url}}/images/unicorn_vs_time_on_chart.png)

![unicorn_pos]({{site-url}}/images/unicorn_vs_weekly_position.png)

Although it was not clear on either charts that song duration was a determining factor for unicorn songs, there was a useful takeaway. Songs over about 250 seconds (4 min 10 seconds), almost never made it longer than 22 weeks on Top100 and almost never made it better than average position of 38.

__"Keep songs shorter than 250 seconds!"__

#### b. Which were the unicorns in 2000?

![unicorn_table]({{site-url}}/images/unicorn_df.png)

### 10. Conclusion and parting thoughts

1. Songs that entered Top100 during autumn months (Sep, Oct and Nov) stayed in Top100 longer, @ alpha = 0.10).
2. Songs that peaked quickly had better average weekly positions, @ \&#913 = 0.01). \U+03B1 \&#945
3. Avoid making songs longer than 250 seconds.

It was admitted difficult to pinpoint features or factors which propelled a song into "unicorn" status in 2000 and I am under no illusion that it would be any more straightforward in 2016. A dataset with more features could perhaps shed more light in this aspect, additional features such as marketing amount spent on song, production costs, time taken to produce the song, number of concerts with song before entering and after entering could perhaps be more interesting from the cost versus profit perspective.
