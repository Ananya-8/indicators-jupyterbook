

Title

The Temporal Anchor: Auditing the Intraday Time-Range Breakout Framework



Brief Description

This strategy is a mathematical Time-Anchor model designed to capitalize on the high-volatility window typically found at market opens. It identifies a single candle at a user-defined time (e.g., 09:15) and treats its boundaries as the critical structural pivots for the remainder of the session. By capturing the High and Low of this specific bar, the system establishes a Fair Value Zone. A breakout above this zone signals institutional accumulation, while a break below identifies a distribution phase.



Source Links

Source code: TradingView Public Pine Script (High and low)

Pine Script Version: v5



Original Code Segment (Pine Script)



```pinescript

// GMT-Synchronized Time Check

t1 = time(timeframe.period, time\_str, gmt)



// Anchor Level Capture

if t1

&nbsp;   s\_high := high

&nbsp;   s\_low  := low



// Signal Logic (The "Fikira" Filter)

if close > s\_high and close < p\_long

&nbsp;   buy := true

&nbsp;   p\_long := close

if close < s\_low and close > p\_short

&nbsp;   sell := true

&nbsp;   p\_short := close

```



Current Pros and Cons



Pros



Institutional Alignment: Market opens are when the highest concentration of institutional Smart Money enters the market. Anchoring to this time captures the most significant price discovery of the day.



Mean-Reversion Filter: The Fikira logic—where a Buy is only taken if it occurs at a lower price than the previous entry—prevents the system from chasing overextended breakouts.



Global Flexibility: The robust GMT offset handler allows the strategy to be deployed on any asset class (NSE, NYSE, LSE) regardless of the user's local chart settings.



Cons



Static Barrier Risk: Because the levels are based on a single bar, a News Spike on that specific candle can create an artificially wide range that price never breaks, leading to a dead trading day.



Lack of Volatility Scaling: The system treats a 5-pip range the same as a 50-pip range, failing to adjust its risk profile based on the size of the anchor candle.



No Macro Trend Awareness: It is purely an intraday breakout tool; it does not account for whether a Buy signal is occurring in a long-term bear market.



Weak Points Identified



The primary vulnerability is Single-Point Dependency. By relying on the High and Low of exactly one candle, the strategy is susceptible to anomalous data—such as a single large limit order fill—that doesn't represent the true daily range. Furthermore, the strategy lacks an ATR-based stop management system, leaving trades vulnerable to sharp v-reversals that often occur after an initial opening breakout.



Suggested Improvements



Opening Range (ORB) Expansion: Instead of a single candle, use the first 15 or 30 minutes of trading to establish a more stable Opening Range boundary.



ATR-Adjusted Trailing Stops: Integrate an ATR multiplier (e.g., 2.0x ATR) to manage exits, protecting capital when a breakout fails to maintain momentum.



Volume Confirmation Gate: Require that the breakout candle possesses volume at least 1.5x higher than the anchor candle to confirm institutional participation.



Improved Code Segment (Reworked Python Implementation)



```python

import pandas as pd

import numpy as np



def calculate\_optimized\_time\_anchor(df, anchor\_hour=9, anchor\_min=15):

&nbsp;   # 1. Identify the Anchor Bar

&nbsp;   df\['is\_anchor'] = (df.index.hour == anchor\_hour) \& (df.index.minute == anchor\_min)

&nbsp;   

&nbsp;   # 2. Capture and Forward Fill Anchor Levels

&nbsp;   df\['s\_high'] = np.where(df\['is\_anchor'], df\['High'], np.nan)

&nbsp;   df\['s\_low'] = np.where(df\['is\_anchor'], df\['Low'], np.nan)

&nbsp;   df\['s\_high'] = df\['s\_high'].ffill()

&nbsp;   df\['s\_low'] = df\['s\_low'].ffill()

&nbsp;   

&nbsp;   # 3. Entry Logic with Fikira Price-Improvement Filter

&nbsp;   df\['long\_signal'] = False

&nbsp;   df\['short\_signal'] = False

&nbsp;   last\_p\_long = float('inf')

&nbsp;   last\_p\_short = float('-inf')



&nbsp;   for i in range(1, len(df)):

&nbsp;       if df\['Close'].iloc\[i] > df\['s\_high'].iloc\[i] and df\['Close'].iloc\[i-1] <= df\['s\_high'].iloc\[i-1]:

&nbsp;           if df\['Close'].iloc\[i] < last\_p\_long:

&nbsp;               df.iloc\[i, df.columns.get\_loc('long\_signal')] = True

&nbsp;               last\_p\_long = df\['Close'].iloc\[i]

&nbsp;               

&nbsp;       if df\['Close'].iloc\[i] < df\['s\_low'].iloc\[i] and df\['Close'].iloc\[i-1] >= df\['s\_low'].iloc\[i-1]:

&nbsp;           if df\['Close'].iloc\[i] > last\_p\_short:

&nbsp;               df.iloc\[i, df.columns.get\_loc('short\_signal')] = True

&nbsp;               last\_p\_short = df\['Close'].iloc\[i]

&nbsp;   

&nbsp;   return df

```



Conclusion



The High and low indicator is an elegant and effective temporal strategy for intraday liquidity timing. While the original version provides a sound framework for identifying key session pivots, its true potential lies in its Fikira logic—prioritizing price improvement over naive breakout following. By expanding the anchor to a multi-bar range and adding volatility-based stops, the system evolves from a simple time-check into a professional-grade intraday participation engine.



