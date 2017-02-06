---
layout: post-no-feature
title: "General Assembly (NYC) Capstone Project 03"
description: "How much is a Yelp review worth in New York City?"
comments: true
category: articles
mathjax: false
quote: ""
---

![yelp_monopoly]({{site-url}}/images/monopoly_yelp.jpg)

### Capstone Project Part 3: Attempting to quantify reputation

Well, the answer is $412. That is if you are operating an average sized retail store in New York City, paying average rent, and looking to locate in an area with 1 more Yelp review per business on average, you will need to pay $412 per year in rent.

But first, a quick recap from where I left off. I am working on a data science bootcamp capstone project, predicting New York City retail rental prices using machine learning and non-traditional data sources. In my previous [post](https://peidacai.github.io/articles/General-Assembly-Capstone02post), I covered the process of wrangling retail rental data and New York City Yellow taxi data.

In this post, we turned our attention to social media data, specifically Yelp reviews, to attempt to put a price to reputation and use that to predict the prices of retail rental locations.

### Goal recap

In my mind, when we mention anything remotely related to real-estate (retail rental locations included), three things come to mind most prominently:

    - Location, location and location

However, as a retailer, it is not always clear how to measure the price of a location. All things equal (ceteris paribus), a rational retailer would seek to maximise customers volume of a location, i.e. to choose a location with the highest exposure to potential customers. However, as with all things in life, some resources are required in exchange for more customers. In this case, it is rent: more exposure to customers, more expensive rent.

### Retail location pricing model?

To break down this chief concern amongst retailers more systematically, I thought of using a model:

![rental_price_factors]({{site-url}}/images/rental_price_factors.png)

The one thing which should be very clear in a retailer's mind is the amount of floor space required, especially since having unused floor space is a luxury most retailers in New York City can ill-afford. So most rational retailers should choose a space just slightly under the total amount required so that rental cost is optimized.

The illusive factor (highlighted by the dashed box) is then location. I attempted to model the price of a location by breaking it down to two main factors: (1) Customer volume, and; (2) "Reputation".

![rental_price_factors_2]({{site-url}}/images/rental_price_factors_2.png)

In this project, I used New York City Yellow Cab taxi passenger dropoff data to model customer volume and Yelp reviews to model "Reputation" of a location.

The first factor is quite straight-forward, although one could argue customer volume can be more comprehensively modeled using other types of data such as subway turnstile count and foot-traffic. I agree with both these factors and I personally think it would be very interesting to find a way to measure "foot-traffic" without actually having someone with a clicker on the ground. I suspect the telecommunication company such as AT&T and Verizon should be able to have an idea with their ground stations log, though I have never actually seen one. From an efficiency standpoint, and for the purpose of this project, only the Yellow Cab data was used.

The second factor, "Reputation", refers to the association a location has with consumers. For example, a Chinese or Italian restaurant may consider a location along Canal street due to its proximity to Little Italy and Chinatown; or an independent fashion label may consider SOHO as a location. Consumers inherently understand the respective associations to certain locations, hence such places are already attracting their targeted customers and they do not have to make customers go out of their way to grace their stores. The flip-side is that the business would face stiff competition with so many similar businesses in its proximity and may need to work harder to establish a competitive advantage. It should be specified at this juncture that this is a business decision left for the executive, with the overall strategy of the company, to decide what their strengths are and how to position the new location to their strengths through taking advantage of data.

### Approach

To quantify "reputation", for each rental location which was scraped from the internet, I went to Yelp and scraped its nearest 10 businesses.

![top_10_neighbors]({{site-url}}/images/top_10_nearest_businesses_yelp.png)

The blue blob is the address of one specific rental location from my list of about 550 rental locations and the red blobs are the surrounding 10 businesses.

For each neighboring business, the following data was extracted:

    1.  User ratings (Number of stars)
    2.  User "$" ratings (how expensive the business is)
    3.  Number of user reviews
    4.  First 20 user review text corpuses (for sentimental analyses)*

* Only 20 reviews were used for each neighboring businesses mainly because of operational efficiency. The main reason to include this is to practise simple Natural Language Processing modules in Python and see its influence on the prices, if any. Should this be a production project, all reviews would be obtained, also assuming that I will be working on a much more powerful machine (which, hopefully, would be paid for by the good company I would be working for).

### Natural language processing - Sentiment Analyses

Sentiment analyses were conducted on the user review text corpuses using TextBlob, a python library which has a built-in sentiment analyses function. The function returns a value between the range of -1.0 to 1.0 for negative and positive sentiments respectively.

### Yelp EDA

The plot below is the comparison between "above mean price locations" (left y-axis) and "below mean price locations" (right y-axis), for each of the four Yelp performance metrics.

![yelp_compare]({{site-url}}/images/yelp_performance_compare.png)

If a metric is a strong differentiating factor, we should see a steep gradient, regardless of polarity of slope. A metric with no differentiating effect returns a line with zero gradient (horizontal).

User star ratings actually had little effect on rental prices. This means that there are more or less equal chances of getting a business with below average rating in expensive locations compared to cheaper locations. I had actually expected the more expensive locations to have better businesses on average.

The other interesting thing to note is that the metric that had the most differentiating effect was "Number of user reviews". In general, this means that locations with more sheer number of Yelp user reviews (regardless of ratings), would have a positive effect on prices. I took this as a metric to measure the "popularity" and/or "reputation" of a location. More users reviewing a location means more buzz on the internet of the place, which in turn, means more people frequenting the location, hence driving up the rental prices.

### How much does rent change with just one more Yelp review?

Since the number of Yelp reviews had the most effect on prices, I thought I would do a linear regression on number of reviews with price per square foot per year to figure out how much does a Yelp review "cost"?

![yelp_cost]({{site-url}}/images/Scatter_psf_yelp_review.png)

The x-axis is the average for top 10 closest businesses to a rental location on Yelp. Therefore, the gradient of the regression line measures the unit cost for an increment in average number of reviews. In this case, it means that each of the 10 neighbors had to increase on average by 1 review, i.e. a total of 10 reviews.

As it turned out, to locate in an area with average 1 more Yelp review per neighboring business, rent increases by $0.15 per square foot per year. For an average size of 2,700 sqft, $76 psf/year, we are looking at an additional $412 per year!

### Conclusion

In this post, I have examined the effects of Yelp performance metrics on retail rental prices, identified the single most deterministic Yelp metric which influenced rental prices most as well as figured out, on average, the cost of locating in an area with 1 more Yelp review.

In the next post, I shall put everything together, rental data, taxi data and Yelp data, using geospatial visualization and create a machine learning model to predict retail rental prices to see if adding taxi and Yelp data helped improved the predictive ability of the models.
