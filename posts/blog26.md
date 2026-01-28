Title

The Volume Decomposer: Auditing the Intraday Buy/Sell Delta Framework



Brief Description

This strategy is an intraday sentiment analysis tool designed to reveal the hidden mechanics of volume bars. While standard volume is color-blind, this system decomposes total volume based on candle displacement, classifying volume as Buying (Bullish displacement) or Selling (Bearish displacement). By utilizing a dynamic array-based cumulative sum that resets every trading day, the framework provides a real-time percentage breakdown of relative buying vs. selling power, identifying points of institutional accumulation or distribution.



Source Links

Source code: TradingView Public Pine Script (Day Buy Sell Volume label)

Pine Script Version: v4



Original Code Segment (Pine Script)



```pinescript

// Volume Calculation \& Array Management

is\_new\_day = time("D") != time("D")\[1]

if is\_new\_day

&nbsp;   array.clear(pos\_bv\_arr)

&nbsp;   array.clear(neg\_sv\_arr)



// Segmentation Logic

buying\_vol = close > open ? volume : 0

selling\_vol = close < open ? volume : 0



array.push(pos\_bv\_arr, buying\_vol)

array.push(neg\_sv\_arr, selling\_vol)



total\_buy = array.sum(pos\_bv\_arr)

total\_sell = array.sum(neg\_sv\_arr)

```



Current Pros and Cons



Pros



Sentiment Transparency: It reveals if a massive volume spike was driven by aggressive buying or heavy distribution, providing insight unavailable in standard charts.



Multi-Timeframe Stability: The use of arrays to track cumulative daily volume ensures the data remains consistent whether viewing a 1-minute or 60-minute chart.



Modern Interface: The dynamic table UI allows an immediate summary of the day's dominant market force.



Cons



Binary Classification Error: Buy/Sell classification is based strictly on candle close vs open. It does not account for wick activity, which can mislabel volume in high-volatility scenarios.



Zero-Sum Lag: As a cumulative indicator, the Delta becomes more stable as the day progresses but can be erratic during the first 30 minutes after open.



No Directional Bias: The script provides volume data but does not trigger trades, requiring pairing with a price-action system.



Weak Points Identified



The primary vulnerability is Displacement Bias. By labeling all volume of a candle as Buy or Sell based only on the close, intra-candle volatility is ignored. A candle with a large upper wick may hide heavy selling pressure even if it closes green. Without a volatility filter or tick-level breakdown, the delta can present a skewed view of true exhaustion.



Suggested Improvements



Wick-Weighted Volume: Improve segmentation by assigning partial volume to Sell if a significant upper shadow exists.



Relative Volume Integration: Compare the Buy/Sell delta against a 20-day volume average to detect institutional anomalies.



Z-Score Alerts: Add statistical alerts when Buy/Sell percentages deviate more than 2 standard deviations from the daily mean.



Improved Code Segment (Reworked Python Implementation)



```python

import pandas as pd

import numpy as np



def calculate\_daily\_volume\_delta(df):

&nbsp;   """

&nbsp;   Splits volume into Buy/Sell based on candle color and resets daily.

&nbsp;   Input df requires: DateTime index, Open, Close, Volume

&nbsp;   """

&nbsp;   # 1. Identify Buy vs Sell Volume components

&nbsp;   df\['buy\_vol'] = np.where(df\['Close'] > df\['Open'], df\['Volume'], 0)

&nbsp;   df\['sell\_vol'] = np.where(df\['Close'] < df\['Open'], df\['Volume'], 0)

&nbsp;   

&nbsp;   # 2. Daily Grouping and Cumulative Sum

&nbsp;   df\['date'] = df.index.date

&nbsp;   df\['day\_total\_buy'] = df.groupby('date')\['buy\_vol'].cumsum()

&nbsp;   df\['day\_total\_sell'] = df.groupby('date')\['sell\_vol'].cumsum()

&nbsp;   

&nbsp;   # 3. Calculate Sentiment Percentages

&nbsp;   df\['total\_day\_vol'] = df\['day\_total\_buy'] + df\['day\_total\_sell']

&nbsp;   df\['buy\_percent'] = (df\['day\_total\_buy'] / df\['total\_day\_vol']) \* 100

&nbsp;   df\['sell\_percent'] = (df\['day\_total\_sell'] / df\['total\_day\_vol']) \* 100

&nbsp;   

&nbsp;   # 4. Signal Logic (Buy/Sell Imbalance)

&nbsp;   df\['bullish\_imbalance'] = df\['buy\_percent'] > 70

&nbsp;   df\['bearish\_imbalance'] = df\['sell\_percent'] > 70

&nbsp;   

&nbsp;   return df

```



Conclusion



The Day Buy Sell Volume indicator is a strong diagnostic tool for intraday traders. It provides an objective view of session sentiment, but its reliance on candle color makes it a first-step filter rather than a standalone system. By integrating wick-weighted volume and statistical imbalance alerts, the system evolves from a simple label into a professional-grade volume sentiment engine.







