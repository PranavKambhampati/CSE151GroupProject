# CSE 151A Group Project Document

# Group Project Milestone 2: Data Exploration & Initial Preprocessing

## Brainstorming

### Feature Selection

[Survey of feature selection and extraction techniques for stock market prediction](https://jfin-swufe.springeropen.com/articles/10.1186/s40854-022-00441-7)

- Difficult read but provides a pretty comprehensive overview of *ML techniques* used for *feature selection*  
- ‘Data inputs and prediction outputs’ section provides examples of the types of indicators used  
  - OHLCV (Open High Low Close Volume) 	data  
  - Technical Indicators				math. formulae on data  
  - Economic Indicators				CPI, macroeconomic factors

### Hypothesis

*\#TODO pick a hypothesis. Are we performing a regression or classification task?*  
	*I think we should stick with regression as we can create classification from whether the stock moves up or down*

- *exp(price) \> price\_curr \=\> up direction*  
- *exp(price) \< price\_curr \=\> down direction*  
- *exp(price) \~ price\_curr \=\> straight direction*

**Hypothesis:**  
Using *stock price data* \+ add. Features:

* Can we correctly predict the future price of the stock?  
* Can we correctly predict the future direction of the stock?

## 

### Data Exploration

#### Data evaluation

We are dealing with **numerical time-series data**. We have a record for each day, containing the Open, High, Low, Close, and Adj. Close price of the stock, in addition to the volume (OHLC). We have **6393 records** (observations) in our dataset, representing that many days of trading. There is **no missing data**  

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

# Data Preprocessing
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

### Implementation
In order to implement these indicators, we can use [TA (Technical Analysis) Lib](https://ta-lib.org/), a Python library that contains a wide range of existing functions to generate technical indicators from stock data

As a part of creating a model to predict stock price, we wrote a linear regression model that uses a Moving Average Indicator, Accumulation Distribution and an RSI indicator. We also calculated the MSE after we added these metrics and plotted the prediction along the actual data. This model looks pretty promising and we will probably refine this further in the next milestone. Our current MSE is about 0.3 which we believe to be pretty low because the numerical values of the stock are larger, indicating relatively good accuracy. Our R^2 test is also pretty high, at 0.99, indicating that the model is pretty well-fitting. We ran other metrics, such as the RMSE and MAE for the test and train splits and the model appears to be pretty accurate, so we will proceed with a linear model.

Our model fits the linear regression line pretty well but we are considering exploring other models. One other model we were thinking of doing was a Support Vector Machine since some research indicated that SVMs could be good to predict whether the stock price will go up or down. This could be another informative metric to analyze. We have already started writing out a preliminary version of this model.

For this milestone, we updated our notebook by adding the linear regression model and calulating some indicators that can go towards aiding the model in making its prediction.

The conclusion of our first model is that its a pretty good fit with relatively low MSE. We can improve on it by potentially adding more metrics to make it more accurate.
