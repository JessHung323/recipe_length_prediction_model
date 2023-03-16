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