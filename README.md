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
This feature is quantitative. Some relevant statistics include:

- mean: 10.017831
- std: 6.442330
- max: 100.000000

`'n_ingredients'`:
This feature is quantitative as well. Some relevant statistics include:
- mean: 9.071652
- std: 3.822948
- max: 37.000000

We used the `StandardScaler` from sklearn to standardize both the `'n_steps'` and `'n_ingredients'` columns in a pipeline with LinearRegression() as the estimator. The result yielded is as follows:
- pipeline score (R^2): 0.0029099031128849706
- pipeline RMSE: 811.5809989228383

From the results above we can see that our baseline model performed poorly. We analyzed the response variable `minutes` and retrieved the following statistics:
- mean: 94.936478
- std: 812.766127
- max: 86415.000000

We can see that the standard deviation of the variable is significantly higher than that of `n_steps` and `n_ingredients`. We decided to remove the outlier rows using the standard method of determining outlier values (as we did in Project 3). When we run the baseline model again on the new dataframe, we see the following result:
- pipeline score (R^2): 0.0029099031128849706
- pipeline RMSE: 811.5809989228383
