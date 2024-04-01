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
3. Pnl of a Trade = P/L(Exit) - P/L(Entry) for long trade, vice versa for short trade.
4. Since the calculation of returns in unclear due to absence of initial amount or expense of trade, the shape is calculated as the (mean(pnl) - risk free)/std_dev(pnl)
5. The drawdown is calculated with respected to the cummax() of the pnl.
                                                  


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

# Z Score with Bollinger Bands Mean reversal MODEL
- Bollinger Bands: Bollinger Bands are calculated based on the z-score rather than the raw price or spread data. This involves calculating the middle band (the rolling mean of the z-score), the standard deviation of the z-score, and then the upper and lower bands by adding/subtracting the standard deviation (scaled by a multiplier) from the middle band.
### Entry Logic

The strategy looks for specific conditions to enter a trade, focusing on the relationship between the previous and current z-scores (iv_spread) and the Bollinger Bands:

- Long Entry Condition: A long position is entered when the previous z-score is below the lower band, and the current z-score has risen above the lower band. This signals that the spread is moving back towards the mean, potentially indicating an uptrend.
- Short Entry Condition: A short position is initiated when the previous z-score is above the upper band, and the current z-score has dropped below the upper band. This indicates the spread is reverting back towards the mean from an overextended position, potentially signaling a downtrend.

### Exit Logic

Positions are exited based on the relationship between the current z-score and the Bollinger Bands or if a maximum number of bars (time periods) have been reached:

- Long Exit Condition: A long position is exited when the current z-score rises above the upper band, suggesting the spread might be overextending upwards.
- Short Exit Condition: A short position is exited when the current z-score falls below the lower band, indicating the spread might be overextending downwards.
- Additionally, positions are automatically exited after a predefined number of bars, regardless of the z-score's position relative to the Bollinger Bands, to limit exposure.

### Bollinger Bands vs Traditional Z Score
Adaptability to Volatility

- Dynamic Widths: Bollinger Bands automatically widen during periods of high volatility and contract during periods of low volatility. This dynamic adjustment provides a more nuanced view of the market's volatility, enabling traders to adjust their strategies in real-time. In contrast, conventional scoring strategies that use static thresholds might not adapt as quickly to changes in market conditions.

Visual Trend Indicators

- Clear Trend Indications: Bollinger Bands can provide visual cues about the trend direction and momentum. For instance, a price consistently touching or exceeding the upper band might indicate a strong uptrend, whereas prices lingering near the lower band might signal a downtrend. This visual simplicity can aid traders in making quicker decisions.

Overbought/Oversold Conditions

- Identification of Extremes: Bollinger Bands can help identify overbought or oversold conditions. Prices near the upper band may indicate that an asset is overbought, while prices near the lower band may suggest it's oversold. This can be especially useful in spotting potential reversal points. Conventional scoring strategies may not directly relate these conditions to the current price context.

Complementary to Other Strategies

- Integration with Other Indicators: Bollinger Bands can be easily combined with other technical indicators and trading strategies to confirm signals or generate more robust trading setups. This versatility makes them a valuable tool in the trader's toolkit.


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
