---
layout: post-no-feature
title: "GA Capstone 02"
description: "NYC Yellowcab data"
comments: true
category: articles
mathjax: false
quote: "Data is the new oil? No: Data is the new soil. - David McCandless"
---

![property_comic]({{site-url}}/images/NYC_taxi.jpg)

### Capstone Project Part 2: Wrangling NYC Yellow Cab data

So, picking up from where I left off [previously](https://peidacai.github.io/articles/General-Assembly-Capstone01post), I web scraped [cityfeet website](http://www.cityfeet.com/cont/ny/new-york-retail-space#) (with much pain) for data on retail rental units available in New York City.

This week, I will focus on the second part of the data for this project: New York City Yellow Cab data.

### Easy Peasy?

Thank goodness for [Taxi and Limousine Commission](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml)! They have generously provided anonymized taxi data online for anyone to use! The data is partitioned into months, starting from January 2009 to June 2016, separated into "yellow", "green" and "FHV" (For-Hire Vehicles).

![nyc_tlc_data]({{site-url}}/images/nyc_tlc_data.png)

I wanted to only look at yellow cab data for 2 main reasons: hardware performance limitations and yellow cab data should be the most representative of the customer volume in New York City (out of the 3). Data for each month is about 2 Gbytes and to account for seasonalities, I would need at least 12 months of data. Eventually, I settled for a year of yellow cab data (1% sample) for operational efficiency reasons. I parsed each 2.0 Gbyte file with some lines of code to take 1 of every 100 lines.

```py
factor = 100
outlist = []

filename = 'yellow_tripdata_2015-10.csv'

in_file = '/Users/peidacai/Downloads/'+filename

with open(in_file, 'r') as f:
    count = 0
    for line in f:
        
        if count % factor == 0:
            count += 1
            outlist.append(line)
        else:
            count += 1
            pass
f.close()

out_file = 'mini-'+filename
write_file = '/Users/peidacai/Desktop/NYC Taxi mini data/' + out_file

with open(write_file, 'w') as f:
    
    f.writelines(outlist)
f.close()
```

Even after taking in only 1% of a year's of only yellow cab data, I ended up with a dataframe of over 1.3 million rows.

### EDA

### Trip distance

First step in my EDA was to remove trips that had zero and negative distances. For the purpose of finding out the locations with the highest dropoff passengers, such trips were not useful. Although it could be interesting to investigate these trips for other purposes such as reliability of the logging machines and driver habits, but we will leave that for another time.

![trip_dist]({{site-url}}/images/Taxi_trip_distance.png)

Leaving out the top 0.1% of the longest trip distance, we see that most of the trips were less than 15 miles long (i.e. within the manhattan).

Looking at trip distance over 100 miles showed that the longest trip in the 1% data set was 52,000 miles, about 9 times the maximum straight line distance across the USA ([2,802 miles](https://www.reference.com/geography/distance-across-united-states-f6665a323ae29d9a)! Trips over 100 miles were removed.

### Average taxi speed

A new feature was created using trip distance and trip duration to offer insights into the traffic conditions in NYC. Turned out that the fastest trip had a taxi which went at 3,333 miles per hour, about 4.3 times the speed of sound, or about twice the max speed of an F15E Strike Eagle fighter jet.

![taxi_avg_spd_pickup_hr]({{site-url}}/images/taxi_spd.png)

After the sanity check, the plot above for mean speed by pickup hour was created. Interesting to note that the speed of taxis plummeted from 5 am onwards and never really recovered until about midnight. And from subway station to subway stations, you are really better off taking the subway, unless it is in the middle of the night.

### How much were New Yorkers tipping?

![taxi_tips]({{site-url}}/images/taxi_tips.png)

Tips data were only given for trips paid for by credit cards. Removing tips of over 100% (highest tip amount was 83,333%!), the most common amount New Yorkers tipped was about 20%.

### Taxi rides by month

Shifting my attention back to dropoff counts, I examined distributions of taxi rides across months, day of week and specifically, dropoff counts by the hour.

![rides_by_month]({{site-url}}/images/taxi_rides_by_month.png)

### Taxi rides by day of week

![rides_by_day]({{site-url}}/images/taxi_rides_by_day.png)

### Number of dropoffs by hour

![dropoff_by_hour]({{site-url}}/images/taxi_dropoff_by_hour.png)

The monthly and daily data seemed reasonable and there were no stark differences between them, hence possibly low predictive abilities. However, there seemed to be distinct time blocks within the hourly data which might have to be taken into consideration when building the predictive model.

### Tying back to the project aim

While it was very tempting to dive deeper into the taxi data, I had to remember to tie the analyses back to the aim of the project which was to use the taxi data to predict retail rental prices. Thus far, inituitively and analytically, dropoff counts by time and location seemed to be most relevant. I will zoomed into these features for the purpose of this project, though, if there was more time, it may be interesting to see if higher tipping percentages had any correlation with rental prices, as it may be an indication of customer spending power.

### Dropoff locations scatterplot

![dropoff_scatter]({{site-url}}/images/dropoff_scatter.png)

With dropoff count by specific latitude and longitude, I did a quick and dirty scatterplot of the dropoff locations, simply by assuming a linear scale of latitude and longitude and plotting the value pairs onto a 2D canvas, hence the skew in the plot. But this provided a really quick way of visualizing the density of the dropoffs and possible correlation to rental prices.

![dropoff_scatter_with_rent]({{site-url}}/images/taxi_dropoff_rental_locations.png)

Adding the rental prices overlay to the plot, the coloured markers were the available rental locations, green was expensive, yellow was medium and red was cheap. The sizes represented the available floor space of each unit. 

Three broad takeways:

1. Harlem had some usually large available units, perhaps a large mall. More information was required.

2. There were some meaningful correlation of prices with density. This provided more impetus for the project, further enhancing the hypothesis that taxi dropoff count might have some correlation to prices.

3. No data for super prime locations near mid-town.

After wrangling the taxi data, the next step would be to get social media, Yelp, data, to complete the dataset for this project. In the next blog post, I will share my experience scraping data from Yelp.

