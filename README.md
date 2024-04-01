# Quant Research Project - IV Pairs Trading


## Research Thesis

Nifty and Bank Nifty indices have significant overlap in terms of their constituents and weights. Our thesis is that their volatilities are also likely to be affected by similar market conditions and macroeconomic factors. We can thus construct a pairs trading strategy that attempts to capture any dispersion that may occur between the volatilities of the two indices

**Output**: A **medium-frequency strategy** that has a trading horizon ranging between **30 min to 5 days**. 



## Contents

1. Loading and Understanding the dataset
2. Data Preprocessing
3. A z-score based trading system.
4. Kalman Filters: A better model than the z-score trading system
5. Comparison with Metrics


## Assumptions and Findings

1. Data Preprocessing: The wrong tte values are corrected using the mode of that dady, Nan values are filled using ffil() and the data is cleaned for post market and holidays.
2. Since the data for some months (Dec-Jan) is missing, we will be assuming a continuous time series from November to January.
3. Pnl in a Trade = P/L(Exit) - P/L(Entry) for long trade, vice versa for short trade.


# Z Score MODEL

This project is a pairs trading strategy to exploit short-term gains. Pair trading is a trend-independent trading strategy in which the short-term deviation from the mean is targeted.
It is a low risk and low reward strategy.

A trading strategy centered on Z-score and mean reversion seeks to capitalize on the principle that asset prices will, over time, return to their historical average. The Z-score quantifies the distance of an observation from the mean in terms of standard deviations, computed through the following formula:

    Z = (X - μ) / σ

Where:

    X is the observed value
    μ is the mean of the population
    σ is the standard deviation of the population


The trading logic encapsulated in this **Long_Short_Backtester class** is a structured approach towards implementing a mean reversion strategy based on Z-scores in the context of financial markets. Here's a breakdown of how the strategy operates:

- Initialization: Upon instantiation, the class initializes with several parameters including file paths, thresholds for trading signals, window size for rolling statistics, and transaction costs. It also sets up a dataframe to record trades.

- Data Loading: The get_data method loads market data from a specified CSV file, which includes time-series data necessary for calculating spreads and Z-scores.

- Strategy Testing: The test_strategy method computes the 'iv_spread' (the difference between two assets, presumably 'banknifty' and 'nifty'), rolling mean, and rolling standard deviation over a specified window. It then calculates the Z-score, which measures how many standard deviations the current spread is from the rolling mean.

- Backtesting Logic: The run_backtest method iterates through each time point in the data set, applying the trading logic based on Z-score thresholds:

- Entry Logic: A trade is initiated (either long or short) when the Z-score crosses predefined upper or lower thresholds. A long position is taken when the Z-score falls below the lower threshold, indicating the spread is unusually low and expected to revert to the mean. Conversely, a short position is initiated when the Z-score exceeds the upper threshold, suggesting the spread is unusually high.

- Exit Logic: The position is closed based on two conditions:
The Z-score crosses back over more moderate thresholds (pos_threshold for short positions, neg_threshold for long positions), suggesting a partial reversion to the mean.
- A **maximum duration** of holding (defined by 'bars') is reached, after which the trade is exited regardless of the Z-score, to limit exposure.

- Recording Trades: Each trade—its type (long or short), entry and exit points, entry and exit Z-scores, durations, and other relevant information—are recorded in a tradebook dataframe for further analysis.

- Loop and Trade Management: The script meticulously manages the state of being in a trade, waiting for an entry signal, or having placed a limit order. It updates trade entries and exits based on the evolving conditions and logs each trade's details for retrospective analysis.



## Glossary
- **Nifty**: Equity Index representing 50 of the largest and most liquid stocks listed on the National
Stock Exchange of India (NSE)
- **Bank Nifty**: Equity Index on the NSE comprising the most liquid and large-capitalized Indian
banking stocks.
- **Option**: a contract that helps you take a view of the direction and volatility of an underlying
stock
- **Pairs Trading**: involves simultaneously buying and shorting two correlated stocks when their prices diverge. The strategy profits from the pair's price ratio returning to its historical norm, aiming to capitalize on relative price movements.
    - **Volatility Pair Trading:** type of pairs trading strategy that involves taking advantage of the difference in volatility between two correlated securities. The strategy assumes that the volatility will either return to or move away from the historical mean.
- **Implied Volatility (IV)**: metric that reflects the market’s expectation of future volatility of the underlying asset or index price.
