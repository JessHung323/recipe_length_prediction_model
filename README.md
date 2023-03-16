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

In our baseline model, we chose the two most intuitive features to use for prediction. It is natural for one to consider that the more steps or ingredients a recipe involves, the longer the recipe would take. 


