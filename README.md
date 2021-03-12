# Bike-Sharing-Multiple-Linear-Regression

### Problem Statement:

A bike-sharing system is a service in which bikes are made available for shared use to individuals on a short term basis for a price or free. Many bike share systems allow people to borrow a bike from a "dock" which is usually computer-controlled wherein the user enters the payment information, and the system unlocks it. This bike can then be returned to another dock belonging to the same system.

A US bike-sharing provider BoomBikes has recently suffered considerable dips in their revenues due to the ongoing Corona pandemic. The company is finding it very difficult to sustain in the current market scenario. So, it has decided to come up with a mindful business plan to be able to accelerate its revenue as soon as the ongoing lockdown comes to an end, and the economy restores to a healthy state. 

In such an attempt, BoomBikes aspires to understand the demand for shared bikes among the people after this ongoing quarantine situation ends across the nation due to Covid-19. They have planned this to prepare themselves to cater to the people's needs once the situation gets better all around and stand out from other service providers and make huge profits.

They have contracted a consulting company to understand the factors on which the demand for these shared bikes depends. Specifically, they want to understand the factors affecting the demand for these shared bikes in the American market. The company wants to know:

>1. Which variables are significant in predicting the demand for shared bikes.\
>2. How well those variables describe the bike demands.\
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

### Analysis Methodology

**Data Cleaning**

1. Checked datatypes and nulls in each column- there were no nulls in any column. The datatypes were all correctly assigned too.
2. Replaced some categorical column values with understandable names.
3. Removed some variables that were not useful for the analysis(instant, dteday, atemp, casual, registered)

**Univariate and Bivariate Analysis**

1. Temp variable is highly correlated with the variables season and month.
2. Target variable cnt is correlated with month.
3. Variables month and season are very highly correlated.
4. hum and weathersit are moderately correlated
5. weekday was not impacting the target variable cnt significantly. Demand on all weekdays was almost the same. However, on weekends, demand was lesser than on weekdays.
6. The least bikes were rented when the weather was snow and rainy. The most bikes were rented in summmer and fall on clear days.

**Preparing the data for modelling**

*Creating dummy variables*\
Dummy variables were created for all categorical variables- 'season','mnth','weekday','weathersit'. drop_first was set to yes as a dummy score of 0 would indicate the first value.

*Train and test data*
1. Data was split 70-30 into train and test.
2. MinMax scaling was applied on the data to bring all variables in data to the same scale. Standard Scaling brings all variables in the range of 0 to +1.
3. 
