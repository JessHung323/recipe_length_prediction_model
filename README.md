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

## **Framing the Problem**
From last time, we explored the relationship between the time (in minutes) a recipe takes and its corresponding rating, and answer the question:

***What is the relationship between the cooking time and average rating of recipes?***

This time in this project, we aim to derive features from the given dataset to:

***Predict the time, in minutes, a recipe takes to prepare*** 

Since we are predicting a numeric variable with non-finite choices, we will be using *regression*, instead of classification. We chose to predict the `'minutes'` column of the dataframe because as users of recipes, we wish to follow recipes that generally don't take too long to complete and we would like to have a rough estimate on how long the recipe would take. If we were told features such as the number of ingredients needed or number steps the the recipe has, we can perhaps have a relatively accurate estimate of how long that recipe would take before fully committing to executing it.

In this project, we would use R^2 score and the Root Mean Squared Error (RMSE) to analyze the accuracy of our predictions on the training dataset. These scores and metrics are more intuitive for readers and easy to interpret as we try to predict a numeric variable.

From our dataset, we transformed the string dates in the `'submitted'` column to the type `datetime` to better understand the timespan of the data. We see that the oldest recipe was submitted on 2008-01-01, while the most recent recipe was submitted on 2018-12-04. We believe that the other columns needed from this project and the essence of our prediction will not be greatly impacted by when the data was collected.

### **Dataset Brief Overview**

We performed similar data cleaning process as we did in our previous [project](https://jesshung323.github.io/recipe-user-interaction-analysis/).

***Relevant Columns***: `'minutes'`, `'rating_average'`, `'n_steps'`, `'n_ingredients'`, `'tags'`, `'protein'`




## **Baseline Model**

***Relevant Columns***: `'minutes'`, `'n_steps'`, `'n_ingredients'`

In our baseline model 1.0, we chose the two most intuitive features at first glance to use for prediction. It is natural for one to consider that the more steps or ingredients a recipe involves, the longer the recipe would take. 

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

We used the `StandardScaler` from sklearn to standardize both the `'n_steps'` and `'n_ingredients'` columns in a pipeline with LinearRegressor() as the estimator. The result yielded is as follows:
- pipeline score (R^2): 0.0029099031128849706
- pipeline RMSE: 811.5809989228383

From the results above we can see that our baseline model performed poorly 😨 

We analyzed the response variable `'minutes'` and retrieved the following statistics:
- mean: 94.936478
- std: 812.766127
- max: 86415.000000

We can see that the standard deviation of the variable is significantly higher than that of `'n_steps'` and `'n_ingredients'`. We decided to remove the outlier rows using the standard method of determining outlier values (as we did in [Project 3](https://jesshung323.github.io/recipe-user-interaction-analysis/#data-cleaning)). When we run the baseline model again on the new dataframe, we see the following result:
- pipeline score (R^2): 0.23434163141717024
- pipeline RMSE: 21.687973493850077

After furthuring the data cleaning step, we can see that the R^2 score has improved for our baseline model 2.0, and the RMSE descreased drastically. This also suggests that our model may not be well-generalized to unseen data because it is possible for testing data to contain recipes with minutes that are way beyond the threshold that was used to determine outliers in our data cleaning process.

In addition, when we plot a scatterplot visualizing the relationship between `'n_steps'` and `'n_ingredients'` with `'minutes'` respectively, we see that their relationships are not linear 😟 This is a factor we will take into consideration when building a more informative model later. 

## **Final Model**

***Relevant Columns***: `'minutes'`, `'n_steps'`, `'n_ingredients'`, `'tags'`, `'protein'`, `''rating_average''`

We incorporated three new features in our final model. 

We chose `'tags'` because we observed that there are tags within the column for each recipe that indicate how long the recipe might take. For example, there exist tags such as "60-minutes-or-less" and "1-day-or-more". These tags may be a useful indicator and predictor for our model.


We believe `'protein'` may be a useful column because in general, a recipe with protein (more meat) will take longer to cook thoroughly.

Finally, we added the `'rating_average'` feature because a recipe that takes too much time may receive a lower rating as users have to devote much more effort.

To extract useful information from the `'tags'` column, we transformed the tags in the form of strings to list of strings. However, as we attempted to generate the most common tags from the entire dataframe, the process was not successful due to the size of the data. We decided to randomly choose 2000 tags from the column and retrieved the top 50 most common tags. After browsing through the tags, we selected the ones more relevant to time:

- 'occasion'
- '15-minutes-or-less'
- '30-minutes-or-less'
- '4-hours-or-less'
- '60-minutes-or-less'

We created 5 new features using the tags from above, indicating whether one recipe contains the tag (1: True, 0: False)
<table border="1" class="dataframe">
	<thead>
		<tr style="text-align: right;">
			<th>minutes</th>
			<th>n_steps</th>
			<th>n_ingredients</th>
			<th>protein</th>
			<th>rating_average</th>
			<th>60-minutes-or-less</th>
			<th>30-minutes-or-less</th>
			<th>15-minutes-or-less</th>
			<th>4-hours-or-less</th>
			<th>occasion</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>40</td>
			<td>10</td>
			<td>9</td>
			<td>3.0</td>
			<td>4.0</td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>45</td>
			<td>12</td>
			<td>11</td>
			<td>13.0</td>
			<td>5.0</td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>40</td>
			<td>6</td>
			<td>9</td>
			<td>22.0</td>
			<td>5.0</td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>40</td>
			<td>6</td>
			<td>9</td>
			<td>22.0</td>
			<td>5.0</td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
		</tr>
		<tr>
			<td>40</td>
			<td>6</td>
			<td>9</td>
			<td>22.0</td>
			<td>5.0</td>
			<td>1</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
			<td>0</td>
		</tr>
	</tbody>
</table>

We utilized Binarizer and StandardScaler in our ColumnTransformer. As the last step in our pipeline, we chose to use DecisionTreeRegressor() as the estimator object, instead of LinearRegressor().

We encountered issues with the `'rating_average'` column as some recipes have not been rated. To address this issue, we randomly sampled a number of values from the `'rating_average'` column to fill the np.NaN. Since there are mutiple columns that do not contain the information we need, such as `'id'`, `'name'`, etc. We choose to drop the qualitative columns from our dataframe. Now the only remaing columns are:

`'minutes'`, `'n_steps'`, `'n_ingredients'`, `'rating_average'`, `'protein'`,`'occasion'`, `'4-hours-or-less'`, `'60-minutes-or-less'`, `'30-minutes-or-less'`, `'15-minutes-or-less'`


If we select the `max_depth` and `min_sample_split` of the DecisionTree to be 5, the result yielded is as follows:
- pipeline score (R^2): 0.8929511159009624
- pipeline RMSE: 8.109488244776092

These results are the most optimal from what we have tested. We used GridSearchCV to determine the final max_depth and min_sample_split hyperparameters for our DecisionTreeRegressor(). 

Our final model is an improvement compared to the baseline model not only by outputting better R^2 and RMSE values. Our model takes into account that the relationship between variables and `'minutes'` is not linear, hence we used the DecisionTreeRegressor. In addition, we performed train_test_split on our data to check how well our model generalizes to unseen data. After dropping the outliers from the dataset, our model consistently produces an R^2 of roughly 0.89. However, we do acknowledge that if we were to predict an extreme outlier, similar to some of the data points we've dropped at the beginning of the analysis, our model would not perform as well. From the original dataframe, we chose the data points that are under the 93rd percentile in the `'minutes'` column. When we used our trained pipeline on this dataset, we see a score of 0.75, which is still significantly better than our baseline model. 


## **Fairness Analysis**

***Group X***: protein under average of the protein from the final model

***Group y***: protein equal to or over the average of the protein from the final model

***Metric***: mean protein from the final model

***Null Hypothesis***: the accuracy of the two groups (equal to or over avg_protein and under avg_protein) are the same, and the difference was by random chance.

***Alternative Hypothesis***: the accuracy of the two groups (equal and over avg_protein and under avg_protein) are different, and the difference was NOT by random chance.

***p_value***: 0.09

***Conclusion***: Since the resulting p-value is higher than the chosen significance level (0.05), we fail to reject the null, meaning the difference in accuracy across the two groups might NOT be significant.












