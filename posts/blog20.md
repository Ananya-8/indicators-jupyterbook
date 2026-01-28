

Title

The Price Action Fortress: Auditing the Intraday Breakout \& Auto-SL Engine



Brief Description

This indicator is a Structural Breakout Framework built for fast intraday markets. It tracks the relationship between consecutive candles to detect shifts in buyer and seller control. The system uses Engulfing-style breakout logic, where a candle closes beyond the high or low of the previous opposite-colored candle to trigger entries. Its standout feature is an Enhanced Mode that creates dynamic risk zones and an SL-Mode that can automatically reverse positions when a breakout fails.



Source Links

Source code: TradingView Public Pine Script (Intraday BUY/SELL \& AUTO SL by chaitu50c)

Pine Script Version: v6



Original Code Segment (Pine Script)

The core logic identifies structural breaks of the previous candle.



```pinescript

// Classic Breakout Logic

isGreen = close > open

isRed   = close < open



buySignal  = isGreen and isRed\[1] and close > high\[1]

sellSignal = isRed and isGreen\[1] and close < low\[1]



// Enhanced SL-Mode Logic

if (enhancedMode and high >= stopLevel and lastSignal == "Sell")

&nbsp;   label.new(bar\_index, high, "SL HIT - BUY", color=color.green)

```



Current Pros and Cons



Pros



• Strict Session Control: Limits trading to defined market hours, reducing low-liquidity noise.

• Visual Risk Feedback: Zone coloring shows whether price is in profit or risk territory, helping emotional control.

• Double-Candle Option: Requiring a break of two candles reduces false breakouts in sideways markets.



Cons



• Wick Sensitivity: Large upper or lower wicks make breakouts harder to achieve, causing delayed entries.

• Aggressive Auto-Reverse: SL-mode can flip positions repeatedly in choppy conditions.

• Heavy Chart Load: Enhanced visual elements can slow performance on lower-tier systems.



Weak Points Identified

The main weakness is Volatility Expansion Risk. If the breakout candle is extremely large, the entry happens far from the stop level, leading to poor risk-to-reward trades. The system lacks a filter to avoid overextended candles.



Suggested Improvements



• ATR-Based Entry Filter: Ignore signals when the breakout candle exceeds 1.5–2.0 times ATR.

• Breakeven Rule: Move stop to entry once price reaches a 1:1 risk-reward.

• Volume Confirmation: Only validate breakouts with above-average volume to avoid false moves.



Improved Code Segment (Reworked Python Implementation)

This version adds a volatility cap to prevent chasing oversized candles.



```python

import pandas as pd

import numpy as np



def calculate\_refined\_breakout(df):

&nbsp;   df\['tr'] = np.maximum(df\['High'] - df\['Low'], 

&nbsp;                         np.maximum(abs(df\['High'] - df\['Close'].shift(1)), 

&nbsp;                                    abs(df\['Low'] - df\['Close'].shift(1))))

&nbsp;   df\['atr'] = df\['tr'].rolling(window=14).mean()



&nbsp;   df\['is\_green'] = df\['Close'] > df\['Open']

&nbsp;   df\['is\_red'] = df\['Close'] < df\['Open']

&nbsp;   df\['candle\_size'] = abs(df\['Close'] - df\['Open'])



&nbsp;   df\['buy\_signal'] = (

&nbsp;       (df\['is\_green']) \&

&nbsp;       (df\['is\_red'].shift(1)) \&

&nbsp;       (df\['Close'] > df\['High'].shift(1)) \&

&nbsp;       (df\['candle\_size'] < (df\['atr'] \* 1.5))

&nbsp;   )

&nbsp;   

&nbsp;   return df

```



Conclusion

The Intraday Breakout \& Auto-SL system is a powerful price-action engine that removes guesswork from entries. It performs best in clean, directional markets with steady momentum. However, during volatile news or choppy sessions, the auto-reverse logic can lead to rapid stop-outs. Adding ATR filters and volume confirmation turns it into a more robust, risk-aware trading framework.



