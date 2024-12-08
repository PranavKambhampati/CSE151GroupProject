# CSE151 Group Project

Our CSE 151A group's members are:
- Pranav Kambhampati (pkambhampati@ucsd.edu)
- Abhijay Deevi (adeevi@ucsd.edu)
- Kevin Do (k8do@ucsd.edu)
- Ethan Cota (ecota@ucsd.edu)
- Theodoros Hadjitofis (thadjitofis@ucsd.edu)
- Ragini Bomma (rbomma@ucsd.edu)

# NVIDIA Stock Price Modeling Report

## Introduction 

The stock market is used by many individuals,from professional investors to more casual traders. Most buy stocks with the general goal to make more money, off it, or at least protect assets by putting them into something stable. However the stock market is extremely unpredictable. Because of the variety of variables and conditions that all go into determining the price of a stock, there is no guaranteed way to be successful. Trying to predict the price or movement of a stock is a complicated but interesting problem that requires skills in data science, machine learning, and economics. Because of its relevance to the real world, and its application in decision-making and forecasting, we wanted to choose this topic as the foundation for our project. We decided to use a dataset of daily trading data for the last two decades for Nvidia’s stock that we sourced from Kaggle.

	Hypothesis: Given a dataset of historical NVIDIA price data, we can design and train a model to predict future prices.

In a real-world setting, designing and training these models requires vast amounts of data and much more complex models, but we wanted to see how far we could get with the foundational data science and machine learning skills we have. The accuracy of the model in a setting like this is absolutely essential, because otherwise in practice the person using it would lose money. Professional investors and funds using models like these are trying to maximize their returns with minimal risk, and these models need to be beneficial with its recommendations of proper investments or portfolio optimizations. The impact of market models like these even extends beyond the stock market itself, because they are also applicable in other chaotic environments. Interactions on social networks, the spread of diseases across a population, and even the weather are all examples of this. The techniques and insights from this project could also apply in these settings as well.


## Methods


### Data Exploration
Our first step for this project was to look through our data. After analysis of our data we found that there was no missing information or irregularities. Next we looked at the data itself to get an idea of their distributions and other statistical information.

### Data Preprocessing
Next we have to process our dataset into something we can use for our models. We decided to utilize technical analysis indicators as our features. In order to implement these indicators, we utilized TA-Lib, a Python library that contains many of these functions which can be applied to transform stock data easily.

#### Model 1
For our first model, we implemented a Linear Regression Model to predict the price data of the stock. We used MACD, A/D, and RSI metrics (see Discussion for details) as our features, and the closing price for our target. To evaluate the accuracy of our model we calculated the MSE, as well as plotting our predictions against the actual prices.

#### Model 2
The next model we decided to try was a Long Short-Term Memory Model. Before training we prepared our data by transforming it into fixed-length sequences, normalizing our features, and reshaping our data to fit a three dimensional format.

We trained our model using Adam as our optimizer and the MSE as our loss function. We trained the model for 20 epochs using batch sizes of 32. We evaluated the accuracy of this model using MSE as well as RMSE, and plotted our predictions against the actual prices.


## Results

### Data Exploration

Our dataset contains 6 columns of 6393 records of data. There is a Date column, 5 columns relating to the Price, and a Volume column. There is missing or irregular data in our dataset. The price information all have very similar ranges, and volume has a very wide range. All of the columns have a heavy right-skew, and are overall increasing with time.

IMAGES ADD HERE

### Data Preprocessing

During this step, we used our dataset to construct the technical indicators we wanted to use. These are Moving Average Convergence/Divergence (MACD), Accumulation/Distribution (A/D), and Relative Strength Index (RSI). After this we performed normalization on our features to set their range between 0 and 1.
Finally split our data into a training and testing set using a standard 80:20 split. This was performed using the most recent 20% of data as our testing and the remaining data for training.


#### Model 1
After training our model the MSE was 0.3 indicating a good accuracy. The R2 tests were at 0.99, indicating high correlation with our features and the target price.
We ran other metrics, such as the RMSE and MAE for the test and train splits.

IMAGES ADD HERE

#### Model 2

The MSE and RMSE were calculated here and showed improved performance over the linear regression model. The LSTM model also successfully predicted the trends shown through the predicted vs. actual plot. Finally, the evaluation metrics here the MSE and RMSE were lower compared to the linear model, telling us that the LSTM was better at modeling since it also considered the sequential dependencies.

IMAGES ADD HERE

## Discussion

### Data Exploration

We started our exploration step first by verifying the integrity of our data. We searched through it to find any irregularities within the data and checked for missing values. We found the dataset was already clean and did not need any additional cleaning during preprocessing, so we could get directly into our analysis (Fig. 1). 
We have 5 price columns, Open, High, Low, Close, and Adj. Close, have extremely similar ranges (Fig. 3), so we used Close during our base level analysis. Looking for any clear trends or patterns that appeared we found that the day to day the price is volatile and unpredictable, but there is a clear upward trend as time increases. This is demonstrated by the heavy right skew in our dataset. However based on the price and volume data alone, seeing the relationship between our dataset and potential future prices is not clear. Because of this we focused on basic information and statistics for this step.

### Data Preprocessing

#### Technical Indicators	
A brief explanation of technical analysis and indicators

There are many ‘features’ which have been hypothesized within the finance industry to attempt to make accurate predictions. These are mathematical patterns which are derived from historical data, and are used by technical traders or investors to predict future price trends and make trading decisions.

For our project, this is the most vital part because the features we create here form the baseline for the rest of our work with the models. After ensuring that all the data was valid and present, our next step was to select features which we could use to accurately predict the price. Instead of trying to come up with our own features from scratch, it is better to instead take advantage of these already existing formulae. There is a vast number of these indicators, and countless ways they can be implemented in combination. After testing and analysis we decided to focus on these three features:

MACD (Moving Average Convergence/Divergence)
	Indicates momentum based on difference of moving averages
A/D (Accumulation Distribution): 
	Indicates momentum based on supply and demand
RSI (Relative Strength Index)
	Indicates momentum based on relative price movement


After creating these we performed normalization using MinMaxScaler to ensure consistency during model training. Our final preprocessing step is small but very important. In order to ensure that our model is not trained with data that is related to our test set, we need to split our data temporally. Using a standard 80:20 training/testing split, we split our dataset such that our testing data is the most recent 20% of trading data, and the training is the rest.

#### Model 1
Our first model was very simple and straightforward to implement. Beforehand we had an understanding that this likely would not fully represent the relationship between our features and the price because the trends of stock prices are generally not linear. Despite this it provided decent results with a high correlation coefficient and a low loss. The linear relation between our technical indicators were pretty good, but that was a little suspicious because it is generally known that stock prices are non-linear and influenced by a lot of different things. In addition, a simple linear regression model won’t capture the full complexity of the stock market. This also doesn’t account for overfitting, can be susceptible to outliers, and have a bunch more problems. 

#### Model 2
We decided that we should select a model with higher complexity to better account for the changes of these market trends. By implementing a Long Short-Term Memory (LSTM) model, our data was represented into sequential chunks and predictions were more continuous allowing our model non-linear relationships. This was a decent approach and a good next step, because this model outperformed our previous Linear Regression model in both performance and accuracy, and our trend line matched with actual values much better.

## Conclusion
Because the task we selected is extremely complex, trying to model a real-world solution requires a lot more information than we were working with. But given the data we were working with we concluded that much of our results were successful, although there are many areas that we could improve. If we used a larger dataset, either with other related stock tickers or more granular data (ex: by second or minute), that alone could add improvement to the overall accuracy of our model.

In our preprocessing step, we tried a few different combinations of indicators and ended up sticking with a few that were commonly used. One thing we could have done differently is try a greater variety of indicators. Selecting these was a difficult process though without background knowledge about actually putting some of these techniques into practice, so for us this step was a lot of trial and error.

During our modeling step, while the results we got from our first model were successful, it may have been a good idea to pick a more complex model to start even if we achieved worse results. We knew that the linear model alone would not be enough to capture the entire relationship between our features and the data, but it was helpful in that it confirmed that there was a strong correlation with the indicators that we picked. This model also does not account for overfitting, and is much more susceptible to outliers and unseen data. Luckily much of these problems we had in our first model we were able to account for by implementing the LSTM. Our predictions and trend line looked much better under this model. We used an index fund and a tech company that have been steadily increasing for a long time, so it would make sense that this would work out. We are unlikely to see the same results if we applied the model onto a much more volatile stock, or one that did not have as large of a market capitalization. This model alone only accounts for details within the price and volume itself, if there was information regarding the company, or other technical details that don’t appear within the price data, our model would start making inaccurate predictions
	
 All in all, the results are still looking very good. We are making good progress towards something that could actually solve the stock market problem. It is also important to remember that stock price prediction is very hard because of the influence of countless factors. However, by adding more and more features and making sure to consider everything that comes to a stock market price, we should eventually be able to somewhat accurately predict prices.

## Statement of Collaboration

- Pranav Kambhampati: Coder: Wrote code for both models
- Abhijay Deevi: Writer: Documentation and final written report
- Kevin Do: Coder: Wrote code for both models
- Ethan Cota: Writer: Documentation and data preprocessing
- Theodoros Hadjitofis: Writer/Github: Documentation and managing GitHub
- Ragini Bomma: Coder/Github : Wrote code for one model and Github readme


#### Link to our Google Collab: [Jupyter Colab Notebook](https://colab.research.google.com/drive/1Edk4vvJ_NKKRyiIwQywZn-mUQE3yYegL?usp=sharing)
#### Link CSE 151A Group Project Report: https://docs.google.com/document/d/1VSGpvdCGO1kRqmZKwCLR7EzSLKd6nTViQp5yKNvCdY8/edit?usp=sharing


## We have included our previous milestones submissions below for reference:

# Milestone 1 - Project Abstract
Our group is planning on doing a stock time series prediction. We plan on using historical data about specific stocks to predict how the stock-exchange and crypto-exchange market can fluctuate in the future. In addition, we will figure out the optimal time to buy and sell. 

These are the datasets we are currently planning on using:

S&P 500: https://www.kaggle.com/datasets/henryhan117/sp-500-historical-data

NVIDIA: https://www.kaggle.com/datasets/programmerrdai/nvidia-stock-historical-data

## Abstract 

Our project aims to analyze historical data on stock prices through their time series to help people figure out the best time to buy and sell stocks. We will utilize datasets on the S&P 500 stock market index or stocks like NVIDIA to train predictive models through Python that strive to capture patterns in price movements. The dataset for the S&P 500 index contains data about environment score, social score, and governance score and highest level of controversy based on ESG. The dataset for the nvidia daily stock price data has data from 2004 to 2024 with the highest/lowest price during trading day, price at closing, and volume of shares during day. Our models will be able to successfully predict the best times to buy and sell by parsing through data and finding the trends. We hope to gain insights into how machine learning models can be used to predict trends in highly volataile markets and understand the potential of machine learning in a financial application.

# Milestone 2: Data Exploration and Initial Preprocessing

## Data Evaluation
We are dealing with numerical time-series data. We have a record for each day, containing the Open, High, Low, Close, and Adj. Close price of the stock, in addition to the volume (OHLC). We have 6393 records (observations) in our dataset, representing that many days of trading. There is no missing data.

**Datatypes:**

* Integer: (OHLC)  
* Float: volume  
* String: date

| Column | Description | Range | Distribution |
| :---- | :---- | :---- | :---- |
| Date | The date for each stock price | ‘01-22-1999’ \- ‘06-18-2024’ | N/A |
| Volume | Qtty shares exchanged | \~ (19.68M \- 9.23B) | *Volume has a wider range, with a right skew, but less pronounced than those of the other columns.* |
| *Pricing ↓* | Represents price during curr. period | *↓ (max rounded to 2 decimal places)* | *Open, High, Low, Close, and Adj Close are expected to be right skewed, with more lower values and less high values. They have a peak all the way on the far left. This aligns with how the stock prices increased quickly later on in its “lifespan”.* |
| Open | At market open | 0.034896 \- 132.99 |  |
| High | Highest achieved | 0.035547 \- 136.33 |  |
| Low | Lowest achieved | 0.033333 \- 130.69 |  |
| Close | At market close | 0.034115 \- 135.58 |  |
| Adj. Close | **Close** adjusted for splits/dividends | 0.031291 \- 135.58 |  |

*See notebook for data plots*

## Data Preprocessing
There are many “features” that have been hypothesized within the finance industry to attempt to make accurate predictions on stock data. Instead of trying to come up with our own features from scratch, it will be a good idea to take advantage of these already existing formulae. There are many types of these *Technical Analysis Indicators*, a large portion of our project may be simply exploring different indicators and combinations of them.

* **Technical indicator:**  A mathematical pattern derived from historical data used by technical traders or investors to predict future price trends and make trading decisions  
  *Used as an **indication** of direction/momentum of stock price*

Example Indicators

* [MACD](https://www.investopedia.com/terms/m/macd.asp) (Moving Average Convergence/Divergence)

	*Tracks the difference between the 12 and 26-period EMA (Exponential MA)*  
	*Indicates a change in bullish/bearish (up/down) momentum*

* [A/D](https://www.investopedia.com/terms/a/accumulationdistribution.asp#:~:text=The%20accumulation%2Fdistribution%20indicator%20\(A%2FD\)%20is%20a,how%20strong%20a%20trend%20is.) (Accumulation Distribution)  
  *Tracks how much money is ‘flowing in/out of’ a stock based on OHLCV*  
  *Indicates how supply/demand is influencing stock movement*  
* [RSI](https://www.investopedia.com/terms/r/rsi.asp#:~:text=The%20relative%20strength%20index%20\(RSI\)%20is%20a%20momentum%20indicator%20used,scale%20of%20zero%20to%20100.) (Relative Strength Index)  
  *Tracks momentum based on the average gain/loss of the stock*

[Further Examples](https://www.home.saxo/learn/guides/trading-strategies/a-guide-to-the-10-most-popular-trading-indicators)

# Milestone 3: Pre-processing and First Model
In order to implement these indicators, we can use [TA (Technical Analysis) Lib](https://ta-lib.org/), a Python library that contains a wide range of existing functions to generate technical indicators from stock data

As a part of creating a model to predict stock price, we wrote a linear regression model that uses a Moving Average Indicator, Accumulation Distribution and an RSI indicator. We also calculated the MSE after we added these metrics and plotted the prediction along the actual data. This model looks pretty promising and we will probably refine this further in the next milestone. Our current MSE is about 0.3 which we believe to be pretty low because the numerical values of the stock are larger, indicating relatively good accuracy. Our R^2 test is also pretty high, at 0.99, indicating that the model is pretty well-fitting. We ran other metrics, such as the RMSE and MAE for the test and train splits and the model appears to be pretty accurate, so we will proceed with a linear model.

Our model fits the linear regression line pretty well but we are considering exploring other models. One other model we were thinking of doing was a Support Vector Machine since some research indicated that SVMs could be good to predict whether the stock price will go up or down. This could be another informative metric to analyze. We have already started writing out a preliminary version of this model.

For this milestone, we updated our notebook by adding the linear regression model and calulating some indicators that can go towards aiding the model in making its prediction.

The conclusion of our first model is that its a pretty good fit with relatively low MSE. We can improve on it by potentially adding more metrics to make it more accurate.

# Milestone 4: Second Model 

We received feedback from our last model about our training and testing data split and we fixed this by manually splitting the data based on the first 80% for the training and the second 20% for the testing data. We did this instead of the randomize split we had before. This ensures that we aren't using future values to predict past values.

#### Where does our model fit in the fitting graph? 

Our model fits basically in the same position as the actual stock price data because it seems like our model is accurate. The LSTM model smoothens out the trend line, so it doesn't get every single exact up and down of the actual price graph.

#### What are the next models you are thinking of and why?

The next model we are thinking of is SVM. We have tried this a bit before but it wasn't as successful as our current model. However, with some more knowledge on building this model, it could be useful for us to more accurately predict the stock prices.

#### Our new LSTM model:

We decided to implement an LSTM model for Milestone 4 because of its ability to handle sequential data. Stock data is inherently sequential as each data point depends on past points. We did additional research to understand that LSTM models are a specialized type of Recurrent Neural Networks that are tailored for this type of task. LSTM models are also good for capturing non-linear relationships, which was a potential problem with our linear regression model from Milestone 3. LSTMS, as a part of deep learning, are power to model these non-linear relationships. To build our LSTM model, we had to scale the data using a MinMaxScaler and then manually split the data. Based on some research, we used the adam optimizer and a mean-squared error loss function. We used 20 epochs and a batch size of 32 to train our LSTM model. Plotting the predicted data against the test data yielded really good results.

#### Conclusion:

Right now it seems like the model is working really good and predicting very well. However, going back to our linear regression model from Milestone 3, we are kind of unsure why it is so accurate. But it seems like the new split means that the model isn't using the "future" data so it should be correct but we are surprised on how good it is.
However, because the LSTM model uses deep learning and is better tailored towards making the predictions we want to make, we are impressed but not surprised by how good it is.

We have included some details on how accurate the model is in the notebook but we can't classify the predictions in terms of FP and FN because we have numerical data and not categorical data.
