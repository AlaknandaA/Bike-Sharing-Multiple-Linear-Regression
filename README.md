# Bike-Sharing-Multiple-Linear-Regression

## Problem Statement:

A bike-sharing system is a service in which bikes are made available for shared use to individuals on a short term basis for a price or free. Many bike share systems allow people to borrow a bike from a "dock" which is usually computer-controlled wherein the user enters the payment information, and the system unlocks it. This bike can then be returned to another dock belonging to the same system.

A US bike-sharing provider BoomBikes has recently suffered considerable dips in their revenues due to the ongoing Corona pandemic. The company is finding it very difficult to sustain in the current market scenario. So, it has decided to come up with a mindful business plan to be able to accelerate its revenue as soon as the ongoing lockdown comes to an end, and the economy restores to a healthy state. 

In such an attempt, BoomBikes aspires to understand the demand for shared bikes among the people after this ongoing quarantine situation ends across the nation due to Covid-19. They have planned this to prepare themselves to cater to the people's needs once the situation gets better all around and stand out from other service providers and make huge profits.

They have contracted a consulting company to understand the factors on which the demand for these shared bikes depends. Specifically, they want to understand the factors affecting the demand for these shared bikes in the American market. The company wants to know:

>1. Which variables are significant in predicting the demand for shared bikes.
>2. How well those variables describe the bike demands.
>3. Based on various meteorological surveys and people's styles, the service provider firm has gathered a large dataset on daily bike demands across the American market based on some factors.

I have to model the demand for shared bikes with the available independent variables. It will be used by the management to understand how exactly the demands vary with different features. They can accordingly manipulate the business strategy to meet the demand levels and meet the customer's expectations. Further, the model will be a good way for management to understand the demand dynamics of a new market. 


**Short Description of Data**

Column Name | desc
--- | ---
instant | record index
dteday | date
season | season (1:spring, 2:summer, 3:fall, 4:winter)
yr | year (0: 2018, 1:2019)
mnth | month ( 1 to 12)
holiday | weather day is a holiday or not
weekday | day of the week
workingday | if day is neither weekend nor holiday is 1, otherwise is 0.
weathersit | 1: Clear, Few clouds, Partly cloudy, Partly cloudy    2: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist    3: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds    4: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog
temp | temperature in Celsius
atemp | feeling temperature in Celsius
hum | humidity
windspeed | wind speed
casual | count of casual users
registered | count of registered users
cnt | count of total rental bikes including both casual and registered

## Analysis Methodology

### Data Cleaning

1. Checked datatypes and nulls in each column- there were no nulls in any column. The datatypes were all correctly assigned too.
2. Replaced some categorical column values with understandable names.
3. Removed some variables that were not useful for the analysis(instant, dteday, atemp, casual, registered)

### Univariate and Bivariate Analysis

1. Temp variable is highly correlated with the variables season and month.
2. Target variable cnt is correlated with month.
3. Variables month and season are very highly correlated.
4. hum and weathersit are moderately correlated
5. weekday was not impacting the target variable cnt significantly. Demand on all weekdays was almost the same. However, on weekends, demand was lesser than on weekdays.
6. The least bikes were rented when the weather was snow and rainy. The most bikes were rented in summmer and fall on clear days.

### Preparing the data for modelling

*Creating dummy variables*\
Dummy variables were created for all categorical variables- 'season','mnth','weekday','weathersit'. drop_first was set to yes as a dummy score of 0 would indicate the first value.

*Train and test data*
1. Data was split 70-30 into train and test.
2. MinMax scaling was applied on the data to bring all variables in data to the same scale. Standard Scaling brings all variables in the range of 0 to +1.
3. Target variables 'cnt' was popped from the train data to further divide train set into predictor variables and target variable.


## Building the model


### First model

1. Used RFE for selecting the top 15 features predicting the target variable, added a constant in the train variables.
2. Built the first model using the statsmodels for detailed statistics of the model.
3. Already, the model has a good value of both R-squared and Adjusted R-squared at 84.3% and 83.8% resp.
4. None of the variables have a low enough p-value to get them removed from the model, i.e., they all have p-values below 0.05.
5. Currently, the first build of our model with 15 variables selected by RFE has two variabels- 'hum', 'temp' with extremely high VIFs. Since 'hum' has a higher VIF than 'temp', I have removed it for the second model.

***What is the difference between R-squared and Adjusted R-squared?***\
R-squared is the a goodness-of-fit measure for linear regression models, in other words, it represents how closely our regression line fits the data. An r-squared value if 100% means that all of our data is explanied by the variables in our model. In more technical terms, it indicates the percentage of the variance in the dependent variable that the independent variables explain collectively.\
Adjusted R-squared is an improvement on R-squared in that it penalizes the model for addition of new variables that might not increase the goodness-of-fit significantly.\
R-squared of a model will always increase whenever we add a new variable, whether this new variable is contributing significantly to the regression model or not. In this way, even if we add all the variables from our dataset into the model, R-squared value will reach 100% and our model will explain the entire variance in the data. But this leads to overfitting. A model having 100% R-squared on the train set will not generalize well and perform poorly on the test set. This results in R-squared value of model being high on the train set, but dropping significantly on the test set. To avoid this, we use the Adjusted R-squared metric to measure the goodness-of-fit of a model,

***Variance Inflation Factor (VIF)***\
VIF is the metric used to measure the multicollinearity among the independent variables. Usually, a VIF value of greater than 5 is considered too high and leads to a variable being removed from the model.

### Second model
1. All variables have accceptable p-values.
2. VIF of 'temp' variable is still higher than 5, though not as high as it was when 'hum' variable was present in the model too.
3. Removed the 'mnth-Jan' variable since it has the lowest coefficient.
4. Cannot remove the temp variable from the model because it is an important predictor variable with the highest coefficient of all variables.

*What does it mean when a variable has a low coefficient:* 
Since we are building a simple linear regression model, which results in a straight line being fitted to the data, the equation of the model is that of a straight line\
y = mx + c\
where,
y = target variable or dependent variable\
x = predictor variable or independent variable\
m = coefficients determining the measure by which the predictor variable affects the target variable\
c = constant\
When the coefficient of a predictor variable in the model is low, it means that that variable is not impacting our target variable as much as some of the other predictor variables with higher coefficients. Thus, we can afford to drop this low coefficient predictor variable. This is also the reason we added a constant before fitting the model on our data, because the linear equation has a coefficient.

### Third model
1. The variable 'mnth-Feb' p-value shot up to 0.391, which is much higher than our threshold of 0.05. Thus I removed this variable from the model.
2. All other variables showing satisfactory p-values, coefficients, and VIfs.
3. Adjusted R-squared value is still good at 82.9%

### Fourth model
1. Adjusted R-squared value slightly increased to 83%.
2. All variables having satisfactory p-values and VIFs.
3. 'mnth_Sept' variable removed as it has the lowest of all coefficients.

### Fifth model
1. 'mnth_Dec' variables removed from the model since it has lowest coefficient.
2. All parameters of model normal.

### Sixth model
1. Adjusted R-squared is at a good 82%.
2. All other statistics of variables satisfactory.
3. This is thus the final model I proceeded with. Number of variables in the final model is 10.

### Residual Analysis of error terms in data
According to the conditions for linear regression:
1. There must be a linear relationship between the dependent and independent variables.
2. Error terms of the independent variables must be independent.
3. The variance of residuals must be constant at different values of the predictor variables.
4. The error terms should be normally distributed.
To check whether these conditions are followed, I performed residual analysis of the error terms of independent variables in our dataset, and saw that they were true.

## Evaluation and final prediction based on model
When scaling the train data, I used this command:\
*df_train[num_vars] = scaler.fit_transform(df_train[num_vars])*\
whereas with the test data, the following command was used:\
*df_test[num_vars] = scaler.transform(df_test[num_vars])*\
The train data is fit and transformed, while the test data is only transformed. This is because the model learns the parameters of scaling on the train data and in the same time scales the train data. We only use transform() on the test data because we use the scaling paramaters learned on the train data to scale the test data.

**After making predictions on the test data using the model trained on the train data, we evaluated our model using the r-squared and adjusted r-squared values of the model on train and test data resp. Adjusted R-squared value of final model on train data was 82.4% while on the test data it was 82.28%. This value is good, and since the r-squared values on train and test data are so similar, it also indicates that the model is neither overtrained neither undertrained on the train data.**
