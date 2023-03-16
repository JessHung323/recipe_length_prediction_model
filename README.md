# Welcome to Recipe Length Prediction Model
## Let's explore more about recipes and their time-needed to prepare!
{:.no_toc}

#### By: Jessica Hung, Samantha Lin
{:.no_toc}


---
### Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}
---

## **Introduction**
From last time, we explored the relationship between the time (in minutes) a recipe takes and its corresponding rating, and answer the question:

***What is the relationship between the cooking time and average rating of recipes?***

This time in this project, we aim to derive features from the given dataset to:

***predict the time a recipe takes to prepare*** 

Since we are predicting a numeric variable with non-finite choices, we will be using regression, instead of classification. We chose to predict the `minutes` column of the dataframe because as users of recipes, we wish to follow recipes that generally don't take too long to complete. If we were told features such as the number of ingredients needed or number steps the the recipe has, we can perhaps have a relatively accurate estimate of how long that recipe would take before fully committing to executing it.

In this project, we would use R^2 score and the Root Mean Squared Error (RMSE) to analyze the accuracy of our predictions on the training dataset. These scores and metrics are more intuitive for readers and easy to interpret as we try to predict a numeric variable.

From our dataset, we transformed the string dates in the `submitted` column to the type `datetime` to better understand the timespan of the data. We see that the oldest recipe was submitted on 2008-01-01, while the most recent recipe was submitted on 2018-12-04. We believe that the other columns needed from this project and the essence of our prediction will not be greatly impacted by when the data was collected.

### **Dataset Brief Overview**

We performed similar data cleaning process as we did in our previous project.

***Relevant Columns***: `'minutes'`, `'rating_average'`, `'n_steps'`, `'n_ingredients'`, `'tags'`, `'protein'`




## **Baseline Model**

***Relevant Columns***: `'minutes'`, `'n_steps'`, `'n_ingredients'`

In our baseline model, we chose the two most intuitive features at first glance to use for prediction. It is natural for one to consider that the more steps or ingredients a recipe involves, the longer the recipe would take. 

***Feature Description***

`'n_steps'`:
This feature is quantitative. 

Some relevant statistics include:
- mean: 10.017831
- std: 6.442330
- max: 100.000000

`'n_ingredients'`:
This feature is quantitative as well. 

Some relevant statistics include:
- mean: 9.071652
- std: 3.822948
- max: 37.000000

We used the `StandardScaler` from sklearn to standardize both the `'n_steps'` and `'n_ingredients'` columns in a pipeline with LinearRegression() as the estimator. The result yielded is as follows:
- pipeline score (R^2): 0.0029099031128849706
- pipeline RMSE: 811.5809989228383

From the results above we can see that our baseline model performed poorly 😨 

We analyzed the response variable `minutes` and retrieved the following statistics:
- mean: 94.936478
- std: 812.766127
- max: 86415.000000

We can see that the standard deviation of the variable is significantly higher than that of `n_steps` and `n_ingredients`. We decided to remove the outlier rows using the standard method of determining outlier values (as we did in Project 3). When we run the baseline model again on the new dataframe, we see the following result:
- pipeline score (R^2): 0.23434163141717024
- pipeline RMSE: 21.687973493850077

After furthuring the data cleaning step, we can see that the R^2 score has improved for our baseline model, and the RMSE descreased drastically. This also suggests that our model may not be well-generalized to unseen data because it is possible for testing data to contain recipes with minutes that are way beyond the threshold that was used to determine outliers in our data cleaning process.

In addition, when we plot a scatterplot visualizing the relationship between `n_steps` and `n_ingredients` with `minutes` respectively, we see that their relationships are not linear 😟 This is a factor we will take into consideration when building a more informative model later. 

## **Final Model**

***Relevant Columns***: `'minutes'`, `'n_steps'`, `'n_ingredients'`, `tags`, `protein`, `rating_average`

We incorporated three new features in our final model. 

We chose `tags` because we observed that there are tags within the column for each recipe that indicate how long the recipe might take. For example, there exist tags such as "60-minutes-or-less" and "1-day-or-more". These tags may be a useful indicator and predictor for our model.


We believe `protein` may be a useful column because in general, a recipe with protein (more meat) will take longer to cook thoroughly.

Finally, we added the `rating_average` feature because a recipe that takes too much time may receive a lower rating as users have to devote much more effort.

To extract useful information from the `tags` column, we transformed the tags in the form of strings to list of strings. However, as we attempted to generate the most common tags from the entire dataframe, the process was not successful due to the size of the data. We decided to randomly choose 2000 tags from the column and retrieved the top 50 most common tags. After browsing through the tags, we chose the one most relevant to time:

- '1-day-or-more'
- '15-minutes-or-less'
- '30-minutes-or-less'
- '4-hours-or-less'
- '60-minutes-or-less'

We created 5 new features using the tags from above, indicating whether one recipe contains the tag (1: True, 0: False)
<!-- Fill in dataframe -->

We utilized Binarizer and StandardScaler in our ColumnTransformer. As the last step in our pipeline, we chose to use DecisionTreeRegressor as the estimator object, instead of LinearRegressor().

We encountered issues with the `ratint_average` column as some recipes have not been used by

If we select the max_depth of the DecisionTree to be 25, the result yielded is as follows:
- pipeline score (R^2): 0.6632343840875179
- pipeline RMSE: 14.395653930339554
