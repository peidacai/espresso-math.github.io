---
layout: post-no-feature
title: "Project01: 2001 SAT state scores across USA"
description: "A introductory data science project examining distribution of 2001 SAT scores in USA."
comments: true
category: articles
---

## General Assembly (NYC) Data Science Immersive Bootcamp Sep-Dec 2016

### Project01: 2001 SAT state scores across USA
We were provided with a dataset consisting of 2001 SAT math and verbal scores together with the respective participation rates from all 50 states across the United States of America plus the District of Columbia.

##### Required tasks:

- Exploratory data analyses using Python and NumPy
- Define and apply descriptive statistics
- Utilize Matplotlib, Seaborn and Tableau for data visualization
- Maintain a git repository to keep track of changes and iterations as project evolves
- Use Unix commands to navigate file systems and modify files

### Approach to project

#### Identify the problem(s)

The main question we are looking to answer is:
	
- "How are SAT scores in 2001 distributed across the United States?"

Another sub-question, seeing that we have participation rates as a feature of the data, is:
	
 - "How do the the SAT scores vary with participation rates?"

#### Data acquisition

No data wrangling or acquisition was required in this project since the data set provided was clean, without missing, NaN, or corrupted values.

#### Parsing the data

The data was parsed using the csv.reader() function into a list of list in Python.

#### Mine the data

Not applicable for this project.

#### Refine the data

Not applicable for this project.

#### Build a data model

Not applicable for this project.

#### Present results

A number of plots were created to visualise the data and three were selected to represent the information contained wthin the dataset.

- Boxplots of SAT math and SAT verbal scores

![Boxplot_math-verb](GA-DSI_Proj01_Boxplot_math-verb.png?raw=true)

Boxplots provide a visual summary of the descriptive statistics. The range for math scores is larger than that of the verbal scores. However, the core (25th to 75th percentile) of math scores is more compact compared to verbal, indicating math scores across most states are more similar compared to verbal. While the maximum math score is higher than that of the verbal, verbal has a higher median score.

- Scatterplot of SAT math vs SAT verbal scores with participation rates for size of plot

![Scatterplot_rate_math-verb](GA-DSI_Proj01_Scatterplot_rate_math-verb.png?raw=true)

This plot answers the second question quite adequately, SAT scores (both math and verbal) are higher with lower participation rates and vice versa.

- Majority of states had SAT scores near the national average

Darkest portion of the plots hovered around near 500 Math and 500 verbal. This correlated with the national average SAT scores in 2001, based on a quick google search [Studypoint website](http://www.studypoint.com/ed/average-sat-scores/).

- Higher participation rates correlated with lower SAT scores and vice versa

The relative positions of the smaller circle, which clustered to the upper right quadrant, compared with the larger circles (higher participation rates) which clustered to the lower left quadrant.

- Correlation != Causation

However, correlation doesn't indicate causation, so without more focused experiments, we cannot conclude that lower participation rates will result in better scores.

- Heatmap of SAT math scores across the United States

![SAT_math_score_heatmap](SAT_math_score_heatmap.png?raw=true)

The heatmap was a surprise for me. I would have expected the east coast belt (locations of the Ivy league colleges) and California to dominate with higher SAT scores. Nevertheless, it seems the central states, in particular the Midwest, were where highest SAT math scores congregated in 2001. It would be interesting to see the trend over the past 2 or 3 decades.

#### Deploy and validate

Not applicable for this project.

#### Project conclusion and potential future work

- Project conclusion

SAT scores seem to have a negative correlation with participation rates across the states and math and verbal scores are positively correlated.

- Machine Learning model to predict States SAT scores

One area of possible future work is to create a machine learning model which takes participation rates and use it to predict SAT scores for each state.

- Richer dataset

However, I suspect that overfitting is very likely since there are only 50 states and perhaps a richer dataset with more features as input to the model may help with better prediction. So perhaps combining with another state-related dataset (other educational data such as Teacher to student ratio, number of schools per unit area, average years of schooling and other educational metrics).

The other method to enhance the dataset is to include time series data, getting more historical SAT scores for rich input to the model and also to analyse trends across time.

The model would have some applications for state education administrators tasked with improving the educational standards of the state

#### Parting thoughts

This was the first project in the 12 week Data Science bootcamp. Personally, this week provided a refresher of Python and EDA visualizations. I have also learnt a lot of new things, in particular:

- Github
- Command line
- Blogging: Doing it for the first time in my life.  :sweat_smile:

Looking forward to starting the modelling and machine learning portions!  :bowtie:





     
