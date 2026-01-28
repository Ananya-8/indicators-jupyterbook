Title

The Shadow Scanner: Auditing the Wick Delta Rejection Framework



Brief Description

This strategy is an exhaustion-detection system designed to identify where price has been rejected by institutional liquidity. It operates on the mathematical premise that wicks represent rejected value. The system isolates the Lower Wick (Buying Pressure) and the Upper Wick (Selling Pressure) to calculate a Raw Delta. By applying Standard Deviation Thresholds (Z-score logic), the system filters out normal market noise and only highlights Outlier Wicks — statistical anomalies that represent extreme blow-off tops or panic bottoms where significant order absorption has occurred.



Source Links

Source code: TradingView Public Pine Script (Wick Delta Buy/Sell Pressure)

Pine Script Version: v6



Original Code Segment (Pine Script)



```pinescript

// Wick Calculation Logic

body\_top    = math.max(src\_open, src\_close)

body\_bottom = math.min(src\_open, src\_close)

upper\_wick  = src\_high - body\_top

lower\_wick  = body\_bottom - src\_low



// Delta Calculation and Volatility Threshold

wick\_delta = lower\_wick - upper\_wick

std\_dev = ta.stdev(wick\_delta, stddevlookback)

upper\_threshold = std\_dev \* stddevlevel

lower\_threshold = -std\_dev \* stddevlevel



// Signal Logic

is\_buy\_outlier  = (wick\_delta > upper\_threshold)

is\_sell\_outlier = (wick\_delta < lower\_threshold)

```



Current Pros and Cons



Pros



Order Flow Transparency: It visualizes Hidden Support/Resistance by showing where aggressive market orders were absorbed by passive limit orders.



Statistical Rigor: By using Standard Deviation (Z-scores) instead of fixed pip values, the indicator automatically adapts to changing market volatility.



Noise Reduction: The optional Heikin-Ashi integration allows the engine to focus on structural rejection rather than erratic tick-level movements.



Cons



Lack of Context: A large rejection wick against a powerful macro-trend is often just a pause rather than a reversal; the indicator does not inherently know the macro-bias.



Repainting Potential: While v6 is stable, using Heikin-Ashi sources can lead to a visual mismatch between the signal and the actual standard price chart.



No Built-in Exit: The script is a pure Point-of-Interest identifier and lacks a structured mechanism for taking profits or managing trailing risk.



Weak Points Identified



The primary vulnerability is Volume Ignorance. A large wick on thin retail volume is a fake-out, whereas a large wick on massive institutional volume is a pivot. The original script treats all wicks equally regardless of the capital flow behind them. Additionally, it suffers from Counter-Trend Bias, often signaling a buy during a crash simply because the wick was long, without confirming if the downtrend has actually stabilized.



Suggested Improvements



Volume-Weighted Delta (RVOL): Multiply the Wick Delta by Relative Volume. This ensures that only High-Effort rejections trigger signals.



Trend Alignment Filter: Add a 50-period Hull Moving Average (HMA). Only take Buy Rejections if the price is trading near or above a structural trendline.



Time-Based Decay: Implement a Signal Strength decay that reduces the validity of the rejection signal if the price does not move away from the wick zone within a set number of bars.



Improved Code Segment (Reworked Python Implementation)



```python

import pandas as pd

import numpy as np



def calculate\_optimized\_wick\_delta(df, lookback=20, std\_dev\_mult=3.0):

&nbsp;   # 1. Basic Wick Calculations

&nbsp;   body\_top = df\[\['Open', 'Close']].max(axis=1)

&nbsp;   body\_bottom = df\[\['Open', 'Close']].min(axis=1)

&nbsp;   

&nbsp;   upper\_wick = df\['High'] - body\_top

&nbsp;   lower\_wick = body\_bottom - df\['Low']

&nbsp;   df\['raw\_delta'] = lower\_wick - upper\_wick

&nbsp;   

&nbsp;   # 2. Volume Weighting (RVOL Integration)

&nbsp;   df\['avg\_vol'] = df\['Volume'].rolling(window=50).mean()

&nbsp;   df\['rvol'] = df\['Volume'] / df\['avg\_vol']

&nbsp;   df\['weighted\_delta'] = df\['raw\_delta'] \* df\['rvol']

&nbsp;   

&nbsp;   # 3. Outlier Detection (Volatility Normalization)

&nbsp;   df\['std\_dev'] = df\['weighted\_delta'].rolling(window=lookback).std()

&nbsp;   df\['upper\_band'] = df\['std\_dev'] \* std\_dev\_mult

&nbsp;   df\['lower\_band'] = -df\['std\_dev'] \* std\_dev\_mult

&nbsp;   

&nbsp;   # 4. Signal Generation

&nbsp;   df\['buy\_rejection'] = (df\['weighted\_delta'] > df\['upper\_band'])

&nbsp;   df\['sell\_rejection'] = (df\['weighted\_delta'] < df\['lower\_band'])

&nbsp;   

&nbsp;   return df

```



Conclusion



The Wick Delta Buy/Sell Pressure indicator is a sophisticated diagnostic tool for identifying price exhaustion. While the original version excels at locating statistical outliers, its true potential is unlocked when wicks are weighted by volume. By shifting from price-only shadows to volume-weighted rejections, this system provides a professional-grade window into the battle between aggressive market participants and institutional liquidity providers.



