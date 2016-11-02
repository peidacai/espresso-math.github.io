---
layout: post-no-feature
title: "GA Project06"
description: "IMDB Movie ratings"
comments: true
category: articles
mathjax: false
quote: “The most powerful leadership tool you have is your own personal example." - John Wooden
---

![IMDB_logo]({{site-url}}/images/logo-IMDB.jpg)

### Project06: Predicting movie ratings for top 250 all-time favorites

### 1. Introduction / Storyline

In this project, we pretended to be data scientists working for Netflix to try and figure out the factors which determined the IMDB movie ratings for top movies of all time. 

Here, we explored ensemble modelling, use of APIs and natural language processing.

#### Problem statement:

- To determine the most influential factors which determine IMDB ratings of all-time favourite movies

### 2. Data exploration

Data from top 250 movies was obtained using the [IMDB API](https://www.omdbapi.com/), prominent features include: actors, title, awards, director and plot.

Original data from API:

- Columns: 20
- Rows: 250

Supplementary data (scraped from IMDB website):

- USA box office gross revenue
- Sound technology used in movie

#### a. Risks and assumptions of data

- 250 movies may not be large enough dataset to properly train a model to predict IMDB ratings. More movies should be obtained to create a better model.

### 3. Exploratory Data Analysis

![rated]({{site-url}}/images/proj6_movies_by_ratings.png)

- Movie ratings distribution of Top 250 movies

![Correlation_awards]({{site-url}}/images/proj6_corrplot_award.png)

- Correlation of various continous features with IMDB Rating

Note that there were no particularly strongly correlated features to rating.

![scatterplot]({{site-url}}/images/proj6_scatter.png)

Besides a somewhat weak correlation of IMDB votes with IMDB ratings, there were no clear linear determinants of IMDB ratings .

### 4. Natural Language Processing

Using the CountVectorizer function from python's sklearn feature-extraction libary, key words were extracted from plot summaries and movie titles to become features as part of the machine learning model. Such NLP features were restricted to top 50 words to avoid overfitting with too many features.

![top50_plot_words]({{site-url}}/images/proj6_top_words_in_plot.png)

- Top 50 words in plot summaries of Top 250 movies of all time

### 5. Ensemble Modelling

![Model_comparision]({{site-url}}/images/proj6_model_performance.png)

Numerous tree-based models were attempted. After removing some outlying movies which grossed over $500 million on opening day, the best performing model with all features was Gridsearched GradientTreeRegressor with mean squared error of 0.036 or RMSE of about 0.19 for IMDB ratings of maximum 10.0.

A separate GradientTree regressor model was fitted using only top 25 most important features excluding "IMDB Votes" and "Metascore" (movie ratings from rottentomatoes). Both features were removed to test the robustness of the model, especially since these features may not be available when predict the IMDB rating of an entirely new movie. After removing both features, a further step was taken to use only top 25 features of the original (~350 features) as part of dimensionality reduction.

Performance of model when using only 25 features was better with RMSE of about 0.25 (approximately average of 0.06 IMDB rating difference from using all features), which is a rather good performance considering the reduction in amount of features.

### 6. Most influential features

![Full_features]({{site-url}}/images/proj6_top10_feature_99.png)

- Top 10 most influential features with all features considered

![25_features_only]({{site-url}}/images/proj6_top10_features_no_vote.png)

- Top 10 most influential features when "IMDB Votes" and "Metascore" were removed

Of interest to note was that Oscar award, either nomination or winning it, mattered less than other awards. And being nominated for awards mattered more than actually winning them. Although these features paled when compared with Gross revenue and Runtime.

Other interesting thing to note was the absence of plot words and title. And the fact that relative unimportance of actors and directors in movies compared with the top features.

### 7. Conclusion

This was an interesting project, in particular, the performance comparison of tree-based machine learning models as well as the use of tuning parameters to improve model performance for each type of model.

Some suggested future work include:

- Expand dataset to inclue top 1000 or top 2000 movies
- Restrict features to only production-related features as oppose to including awards-related features (To try and isolate the "secret recipe" of a popular movie)
- Scrape the corpus of each movie reviews to determine the keywords used by reviewers to associate with top movies
