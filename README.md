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
