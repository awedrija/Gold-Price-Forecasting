# Gold-Price-Forecasting
Analyse the OHLC data of gold price and build a time series model to forecast future price movements. Use time series analysis and data science methodologies to solve this problem. The deliveries are model, accuracy metric like R2, Scatter plot of price_change vs model price change, Scatter plot residual.

An Overall Description of the Project:

I have used the closing stock prices for the project. Financial time series has a key feature that distinguishes it from other types of time series, which is uncertainty. As a result of the increased uncertainty, statistical theory and methods play an essential part in financial time series analysis.

The closing prices of Gold from 2008 to 2022 do not appear to have any specific trend, there are both sharp rises as well as dips in the price.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/5fb76250-b7d8-4f69-918e-54dc928df3a9)

While seasonal variation is not evident from the time series plot, there is clear cyclical variation over the years.

Outlier Detection:

I have used a Boxplot to visualise any outliers in the closing prices, and there do not appear to be any outliers. I have further confirmed this using the Z-scores. Usually, any z-score greater than +3 or less than -3 indicates an outlier.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/7d7cc03a-f663-44dc-9fea-ebe9007a96f5)

Normality is not clear in the data, due to high volatality of the prices.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/f91a5ff0-66c6-4971-9c1e-a7a8793819dc)

Stationarity of the Time-Series:

A stationary time series has statistical qualities that remain constant across time, such as mean, variance, and autocorrelation. Whether the financial time series exhibits stationarity, can be determined by statistical tests of stationarity such as the Augmented Dickey Fuller (ADF) Test.
ADF test : 𝐻0 ∶ 𝛾 = 0, i.e., there is a unit root, i.e., the data is not stationary,
against,
𝐻1 ∶ 𝛾 ≠ 0, i.e., there is no unit root, i.e., the data is stationary.
From the high volatility of the closing prices, it is evident that the time series is not stationary, which is further confirmed by the ADF test.

Differencing:

We need to make the series stationary before applying any AR, MA, ARMA or ARIMA models on it. A method of transformation is differencing. .If 𝑦𝑡 be the value of the original time series at time point t, the value of the differenced time series at time point t is given by,
𝑦′𝑡 = 𝑦𝑡 − 𝑦𝑡−1
We look at the ACF and the PACF plots to determine the order of differencing required to make the series stationary.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/56dc951a-7b4e-45aa-a925-9a8045fa92bd)
![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/6b267b33-38eb-4aac-b3bf-cde4845026d0)

From the above plots, we observe that the ACF gradually decreases while there is a sharp drop in the PACF after one lag. The ACF and PACF plots suggest that there is significant autocorrelation in the data, and confirm that the order of differencing should be 1.

After differencing, the ADF test confirms that the series is now stationary.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/5f59347b-ec7c-4aa9-a23a-5c9010d4be3c)

Then, I have split the data into the training and test sets. I have taken all closing prices from 2008 to 2020 in the training set, and the prices in 2021 and 2022 in the test set.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/9748eef8-49ee-4c20-885b-d83a1267c8e9)

ARIMA Model:

From the ACF and PACF plots, there appears to be strong autocorrelation, so I went with the Auto Regressive Integrated Moving Average model. 

An ARIMA model is characterized by 3 parameters: p, d and q:

p - The number of lag observations in the model; known as the lag or autoregressive order.

d - The number of times that the raw observations are differenced; known as the degree of differencing.

q - The size of the moving average window; known as the order of the moving average.

The ARIMA model is expressed as:

𝑦′𝑡 = 𝑐 + 𝜙1𝑦′𝑡−1 + ⋯ + 𝜙𝑝𝑦′𝑡−𝑝 + 𝜃1𝜀𝑡−1 + ⋯ + 𝜃𝑞𝜀𝑡−𝑞 + 𝜀𝑡

Where, where 𝑦′𝑡 is the differenced series. The predictors include both lagged values of 𝑦𝑡 and lagged errors, 𝜀𝑡 is the random error at t, 𝜙𝑖 and 𝜃𝑗 are the coefficients. This is called ARIMA (p,d,q) model.

I have used the auto_arima() function which determines several appropriate ARIMA (p,d,q)models for the time series data, and then chooses the optimum model by minimizing the AIC and BIC criterion.

Using the auto_arima function, the optimal ARIMA model comes out as ARIMA(2,0,2).
From the graph, it appears that the ARIMA model has predicted prices well, but not been able to capture the volatility at all.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/6621ff4e-315a-4367-a093-5ddb5aa4ce6f)

Model Evaluation:

I have evaluated the model using MSE and RMSE.

RMSE: 1.9268462372754804

MSE: 3.712736422102677

The MSE and RMSE are pretty low, and hence the model can be said to be performing well.

Finally, I fit the model on the original Closing Prices, and the predictions almost co-incide with the actual values, hence the model is a good fit for the time series data.

![image](https://github.com/awedrija/Gold-Price-Forecasting/assets/97799511/7fc6ecbf-1b04-40a0-a930-9856ed4bde00)


