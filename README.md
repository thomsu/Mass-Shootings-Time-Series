# Mass-Shootings-Time-Series
Time series forecasting with SARIMA, VAR, Fast Fourier Transform, Exponential Smoothing, Prophet and LSTM Network on US gun violence incidents that result in multiple casualties.

## Overview
Using common time series forecasting models, I attempted to reliably predict the number of weekly gun violence victims from public gun violence tracking data.

## Data Source
The data was downloaded from https://www.gunviolencearchive.org/reports in separate 7 csv files (by year). The time series spans from January 1, 2014 to July 11, 2020. These files share 7 features, which are Incident Date, State, City or County, Address, # Injured, # Killed and Operation. There is no Operation data in any of the files. Some files contain an additional feature, Incident ID. Excluding Operation and Incident ID, there is not many missing values.

## Exploratory Data Analysis
I examined various aspects of gun violence. Here are some of the key points:

1) The Las Vegas country music event shooting in 2017 and the Orlando nightclub shooting in 2016 are the top 2 incidents with the most number killed and number injured. These two incidents are outliers
2) The # Injured and # Killed have a correlation of 0.64. Of the 2373 incidents, there are 1980 incidents where the # Injured is greater than # Killed.
3) There are 4 locations where mass shootings happened twice during the span of the time series.
4) Hawaii, New Hampshire and North Dakota are the only states with no mass shooting incident.
5) Chicago is the top city with the most incidents, # Injured and # Killed.
6) California, Florida, Illinois, Louisiana and Texas consistent make it into the top 10 states with the most incidents every year.

![usa map](/images/USA_map.png)

![chicago map](/images/Chicago_map.png)

There is no clear trend in data but warmer months tend to have more gun violence victims, providing hints that there is seasonality.
![boxplots](/images/Boxplots.png)

## Time Series models
I tried to convert the signal from a time domain to a frequency domain with fast fourier transform. However, the frequency plot shows numerous small amplitudes for all the frequencies except for the first. The time series is too complex to be decomposed into and modeled by a few frequencies.

The next model that I tried is SARIMA. The ACF plots depicted an oscillating wave pattern in the serial correlation while the PACF plot did not have any pattern. This indicated that the autoregressive model is a more suitable starting model. After examining the results of two SARIMA models, SARIMA(0, 0, 0)(1, 1, 0, 52) is the better model with a Mean Absolute Percentage Error of 0.35 and a Mean Square Residual Error of 28.56. This model, like many other models, under predicted the number of victims for most of May and June 2020 as the numbers are much higher than normal for those months.

![acf pacf plots](/images/ACF_PACF_plots.png)

I attempted to fit deseasonalized # Injured and # Killed with VAR but the predictions varied widely with actual numbers. Modeling using Holts-Winters Exponential Smoothing also produced disappointing results, with overestimation of test predictions for January to about May 2020 and underestimation from May to June 2020.

![holt winters plot](/images/Holt_Winters_plot.png)

Because the Prophet model provides an upper and lower bound for forecasting, it is difficult to discern if this model is better than the SARIMA model. The Prophet predictions breach those limits on five different weeks in the first 26 weeks of 2020. I felt that the actual values should lie consistently within the bounds of those limits if the model is reliable. The final and most complex model which I attempted to fit is LSTM. Because all the parameters are randomly initialized, I ran the model many times until I got the most optimal result.

![lstm prediction plot](/images/LSTM_prediction_plot.png)
