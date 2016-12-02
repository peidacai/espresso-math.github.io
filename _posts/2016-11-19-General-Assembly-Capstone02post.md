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

### Capstone Project: Wrangling NYC Yellow Cab data

This week, in place of the weekly project, we began our journey into our capstone projects.

My chosen project is "Predicting NYC retail rental prices with taxi data".

### Aim / Impetus

I knew I wanted to do a project with geospatial data, however, after discussions with the bootcamp instructors, most geospatial analyses by themselves involves descriptive projects as opposed to predictive modelling. Therefore, in order to fulfil the bootcamp requirement, I had to find a way to marry the geospatial data with a predictive model.

From my experience in the retail food and beverage industry, I found the inspiration for my capstone project. In my previous life, I had the opportunity to launch 3 food retail stores in Singapore. And like all other retail business, location of stores ranked the highest in priority. However, many times, I found information lacking on the potential locations. Specifically, information on potential customer reach of the locations such as estimate of customer footcount, customer demographics, etc.

Therefore, I started looking around at data science projects which dealt with geospatial data in NYC and stumbled upon this [brilliant blog](http://toddwschneider.com/posts/analyzing-1-1-billion-nyc-taxi-and-uber-trips-with-a-vengeance/) by Todd W. Schneider. In his blog, Todd worked with over 1.1 billion rows of taxi and uber trip data with some breathe-taking visualizations! I was so inspired to attempt something like this before I realized that the size of the data he worked with was over 3.3 gigabytes! There was no way my aging MacBook Air with a measly 4GB RAM would be able to take such a beating. I had to think of a way to work around this. More on this in subsequent posts!

### Web scrapping retail rental data

Now that I had a clear direction, I had to start gathering the data. As mentioned, I faced a significant road block when trying to get taxi data, so I decided to put that aside for now and focus on gathering the rental data. I did a search for online resources of commercial rental properties in NYC and found one that was very promising from [loopnet](http://www.loopnet.com/), and the bonus was that they do not use AJAX (Asynchronous JavaScript and XML). While useful for users, websites which use AJAX make life for web scrappers much harder and more time-consuming.

However, my joy was short lived. While scrapping loopnet for commercial data, I managed to get over 1,000 listings in NYC. I was overjoyed! But once I put in practice one of my lessons learnt from the earlier projects, to drop duplicates, I was left with less than 50 listings! When I tried to expand my search in loopnet, they banned my IP address! :(

### Experiencing Selenium and PhantomJS

I had to turn my attention elsewhere, and found another promising website [cityfeet.com](http://www.cityfeet.com/cont/ny/new-york-retail-space#). As you probably can tell, this site uses AJAX and I had to dig in with Selenium and PhantomJS (with some additional lines of code to "wait" for rest of the page to load before continuing to scroll down). I found the code after searching on [stackoverflow](http://stackoverflow.com/questions/28928068/scroll-down-to-bottom-of-infinite-page-with-phantomjs-in-python).

```py
# Infinite scrolling

lastHeight = driver.execute_script("return document.body.scrollHeight")
while True:
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    time.sleep(pause)
    newHeight = driver.execute_script("return document.body.scrollHeight")
    if newHeight == lastHeight:
        break
    lastHeight = newHeight
```

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
