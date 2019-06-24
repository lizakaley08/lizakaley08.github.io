---
layout: post
title:      "Multicollinearity When Working with Multiple Regression"
date:       2019-06-24 21:03:10 +0000
permalink:  multicollinearity_when_working_with_multiple_regression
---

# Code Used to Help Identify Multicollinearity: Variance Inflation Factor (VIF)"


**Multiple Regression – in simple terms**

Multiple Regression is a statistical analysis that can be used to model and predict a dependent variable, Y, from two or more independent variables, X.  This analysis produces key results that are indicative of strong relationships between the Y and X variables.  

The first set of results worth noting are R-squared and Adjusted R-squared.  When running a multiple regression model, one goal is to ensure there is a “good fit” between the dependent and independent variables.  R-squared and Adjusted R-squared should always be between 0 and 1.  The closer these two are to 1 the “better fit” the model.  For example, an R-squared of 85% means that the percent of the variance in Y can be explained by all the selected X variables.   It is also worth noting that Adjusted R-squared is just as important as R-squared as it’s resulting percent represents the variance in X that can be explained by the selected Y variables that truly affect X. Adjusted R-squared will always be lower than R-squared; however, if the model is a “good fit” it will not be significantly lower and a high percent (e.g., closer to 1).

Another set of key results in multiple regression are the p-values for each of the X variables.  The lower the p-value the better.  For example, there is usually a threshold limit set in statistical testing, and it is a good thing if the p-value is less than the set limit.  A common threshold limit is 0.05.  So, if the X variable p-value is less than the limit (e.g., 0.05), X has a statistically significant relationship with the dependent variable, Y.
When you have a set of statistically significant X variables with low p-values, it’s a good idea to look at the coefficients’ and their individual standard of errors.  If the standard of errors is high, it can mean that multicollinearity exists, and an X variable may appear insignificant while it in fact should be significant.

**What is Multicollinearity**

Multicollinearity exists when independent X variables are correlated to other independent X variables, as well as, to the dependent Y variable.  This is a sign that there may be too many X variables and subsequently competing against each other making the regression model too complex and reducing its ability to tell the true story about what X variables are in fact significant in determining/influencing the Y variable.

**Multicollinearity and King County Multiple Regression**

One of the first decisions made when working with this dataset to answer what market and home features predict home price in King County was that most of the features would be classified as categorical.  Some features were eliminated during the heat map stage and checking for multicollinearity (before making features categorical); however, along with this choice came an extensive pool of independent X variables.  The initial approach was to see after scrubbing, exploring and evaluating the data, what would the regression results look like with all the remaining continuous and categorical variables.  There may be a question as to why so many variables were left in the dataset.  The simple explanation is that with a background in real estate, there were some features I couldn’t simply remove because they showed low correlation to the price in the Heat Map, specifically, zip code.  Location has always been a market feature that influences price in previous research, so I wanted to see what would happen by leaving it in and making it a categorical variable.

During the multiple regression modeling and testing, there were signs of added complexity and the hunch was that multicollinearity existed after the feature selection process – the hint was the individual features coefficient standard errors were high.

Given with all the variables (over 100), the time to go through each independent variable and figure out which independent variables were collinear seemed to time sensitive.  After doing some added research, I came across the topic of Variance Inflation Factor (“VIF”).  VIF detects and measures collinearity among independent variables within multiple regression.  This is done by assessing how much the variance of an estimated coefficient increases during regression if the independent variables are correlated.  If the resulting VIF for the independent variables is greater than 1 and less than 5 it is indicative of some correlation.  It is more concerning if VIF results are between 5 and 10, which indicates that high correlation exists and that the model is not producing dependable coefficients resulting from multicollinearity.  

The next step chosen was to layer VIF into the end of King County Multiple Regression model and identify which selected features had VIF greater than 5.  One of the solutions after running VIF is to remove independent variables that are highly correlated, but it is not necessary to remove each one.  Consequently, some of the variables with high VIF results were eliminated and the regression testing and modeling was re-run … it took two passes after putting VIF into the model.  The elimination of these variables only slightly impacted the overall R-squared and Adjusted R-squared and almost all the significant features had very low standard errors and all of the p-values remained very low, mostly 0, meeting the threshold of below 0.05.

*Following is the code used in my model:*

from statsmodels.stats.outliers_influence import variance_inflation_factor
from statsmodels.tools.tools import add_constant

X = add_constant(X_fin)
vif = pd.DataFrame()
vif['VIF_Factor'] = [variance_inflation_factor(X.values, i) for i in range(X.shape[1])]
vif['Features'] = X.columns

vif.loc[(vif.VIF_Factor > 5)]

