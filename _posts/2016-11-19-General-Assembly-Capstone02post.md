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

I wanted to only look at yellow cab data for 2 main reasons: hardware performance limitations and yellow cab data should be the most representative of the customer volume in New York City (out of the 3). Data for each month is about 2 Gbytes and to account for fluctuations between months, I would need at least 12 months of data. Eventually, I settled for a year of yellow cab data (1% sample) for operational efficiency reasons. I parsed each 2.0 Gbyte file with some lines of code to take 1 of every 100 lines.

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

#### Trip distance

First step in my EDA was to remove trips that had zero and negative distances. For the purpose of finding out the locations with the highest dropoff passengers, such trips were not useful. Although it could be interesting to investigate these trips for other purposes such as reliability of the logging machines and driver habits, but we will leave that for another time.

![trip_dist]({{site-url}}/images/Taxi_trip_distance.png)

Leaving out the top 0.1% of the longest trip distance, we see that

From my experience in the retail food and beverage industry, I found the inspiration for my capstone project. In my previous life, I had the opportunity to launch 3 food retail stores in Singapore. And like all other retail business, location of stores ranked the highest in priority. However, many times, I found information lacking on the potential locations. Specifically, information on potential customer reach of the locations such as estimate of customer footcount, customer demographics, etc.

Therefore, I started looking around at data science projects which dealt with geospatial data in NYC and stumbled upon this [brilliant blog](http://toddwschneider.com/posts/analyzing-1-1-billion-nyc-taxi-and-uber-trips-with-a-vengeance/) by Todd W. Schneider. In his blog, Todd worked with over 1.1 billion rows of taxi and uber trip data with some breathe-taking visualizations! I was so inspired to attempt something like this before I realized that the size of the data he worked with was over 3.3 gigabytes! There was no way my aging MacBook Air with a measly 4GB RAM would be able to take such a beating. I had to think of a way to work around this. More on this in subsequent posts!

### Web scrapping retail rental data

Now that I had a clear direction, I had to start gathering the data. As mentioned, I faced a significant road block when trying to get taxi data, so I decided to put that aside for now and focus on gathering the rental data. I did a search for online resources of commercial rental properties in NYC and found one that was very promising from [loopnet](http://www.loopnet.com/), and the bonus was that they do not use AJAX (Asynchronous JavaScript and XML). While useful for users, websites which use AJAX make life for web scrappers much harder and more time-consuming.

However, my joy was short lived. While scrapping loopnet for commercial data, I managed to get over 1,000 listings in NYC. I was overjoyed! But once I put in practice one of my lessons learnt from the earlier projects, to drop duplicates, I was left with less than 50 listings! When I tried to expand my search in loopnet, they banned my IP address! :(

### Experiencing Selenium and PhantomJS

I had to turn my attention elsewhere, and found another promising website [cityfeet.com](http://www.cityfeet.com/cont/ny/new-york-retail-space#). As you probably can tell, this site uses AJAX and I had to dig in with Selenium and PhantomJS (with some additional lines of code to "wait" for rest of the page to load before continuing to scroll down). I found the code after searching on [stackoverflow](http://stackoverflow.com/questions/28928068/scroll-down-to-bottom-of-infinite-page-with-phantomjs-in-python).



And after waiting a couple of hours, I finally managed to scrap over 1,000 listings, but after accounting for listings in NYC only, I was left with just under 400 listings. Not nearly enough for a good model, but a good place to start.

### Initial EDA

I proceed to create some quick plots of the rental data. I had included two plots of the most significant plots below, both histograms.

The first one below shows the floor space distribution of the available retail listings. It seemed as expected, with the majority of the listings near the 1,000 sqft range, since floor space is at a premium in NYC. There was an interesting development of substantial size (not shown in this histogram as it was limited to 20,000 sqft), and it turned out to be a new 500,000 sqft [development](http://rew-online.com/2016/11/02/blumenfeld-breaks-ground-on-east-harlem-rental-designed-by-bjarke-ingels-group/) in Harlem.

![properties_floor_area]({{site-url}}/images/capstone_part2_floor_area_histogram.png)

The second, more worrying plot was the one below, which showed the distribution of prices per square feet per year. The plot looked very counter-intuitive as I was expecting to see a wider range of listing prices and a higher modal price (right now it seemed to be hovering around $30 per square foot per year). From news reports such as [this from CNBC](http://www.cnbc.com/2016/11/08/manhattan-retail-rents-dip-but-remain-near-record-highs.html), NYC remains the most expensive city to rent a retail property, with average lease price for ground floor units of \$973, up to \$3,138 for luxury rentals along Upper Fifth avenue. This was clearly not the story my scraped data was telling me.

![properties_psf]({{site-url}}/images/capstone_part2_psf_hist.png)

### Sad revelation

And this was when I remembered that like Singapore, NYC probably control the listings of prime retail units. These units would usually be exclusively brokered by some large property consulting companies such as [CBRE](http://www.cbre.com/?utm_source=search&utm_medium=paidsearch&utm_campaign=wpp?utm_term=cbre) and  [Jones Lang LaSelle](http://www.us.jll.com/united-states/en-us) and will not be publicly available on third party websites. In order to have access to these listings, you would either have to be an agent or a leasee, neither of which I am keen to pose at the moment, considering the demands of the bootcamp, and well, life in general.

### Working with what I have

Hence, my approach to this project was to plough ahead with the current list of properties which I found, to create a project based on proof-of-concept more than an actual working one since I can already see that the performance of the model will be poor with the low quality data going into the model.

Nevertheless, conceptually, the project is still feasible and workable, just that it would perform to its full potential only with a high quality rental data input.

With this, I shall end this blog post. In the next post, I will describe the joy and pain of dealing with taxi data!
