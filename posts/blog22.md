Title

The Convergence Engine: Auditing the Triple-Filter Stoch RSI \& MACD Framework



Brief Description

This indicator is a momentum-exhaustion reversal system built to capture high-probability turning points. It uses a triple confirmation structure: oscillator extremes (RSI and Stoch RSI), momentum direction (MACD behavior), and price action validation (candle structure). By stacking these filters, the system attempts to remove early or weak reversal attempts and focus only on statistically stretched market conditions.



Source Links

Source code: TradingView Public Pine Script (Stoch RSI and RSI Buy/Sell Signals with MACD Trend Filter)

Pine Script Version: v6



Original Code Segment (Pine Script)

The logic requires simultaneous alignment between exhaustion, trend weakness, and bearish candle confirmation.



```pinescript

// Momentum Exhaustion + Trend Filter Logic

stochOverbought = stochK > 90 and stochD > 90

rsiOverbought   = rsiValue > 70

macdTrendDown   = macdLine < macdSignal



// Entry with Candle Confirmation

if (stochOverbought and rsiOverbought and macdTrendDown and close < open)

&nbsp;   label.new(bar\_index, high, "SELL", color=color.red)

```



Current Pros and Cons



Pros



• Strong Signal Filtering: Dual oscillator extremes ensure signals only appear when the market is statistically stretched.

• Structural Awareness: Integration of support and resistance zones provides logical price areas for reversals.

• Multi-Timeframe Context: The dashboard prevents trading against higher-timeframe trend direction.



Cons



• Entry Lag: Waiting for MACD confirmation and candle color often delays entry by several candles after the pivot.

• Oscillator Correlation: RSI and Stoch RSI are mathematically related, reducing true independence of confirmation.

• Chart Clutter: Multiple visual elements can distract from core price structure.



Weak Points Identified

The main risk is strong trending environments. Oscillators can stay overbought or oversold for long periods while price continues trending. If MACD briefly weakens and then resumes trend direction, counter-trend signals can repeatedly fail.



Suggested Improvements



• Volume Rejection Filter: Validate signals only when the candle shows high volume with rejection wicks.

• Dynamic RSI Thresholds: Replace fixed RSI levels with volatility-adjusted bands.

• Break-Even Protection: Move stop to entry once price reaches a 1:1 reward level.



Improved Code Segment (Reworked Python Implementation)

This version ensures MACD momentum is actually turning, not just below its signal line.



```python

import pandas as pd

import pandas\_ta as ta



def calculate\_triple\_lock\_reversal(df):

&nbsp;   df\['rsi'] = ta.rsi(df\['Close'], length=14)

&nbsp;   stoch = ta.stochrsi(df\['Close'], length=14, rsi\_length=14, k=3, d=3)

&nbsp;   df\['stoch\_k'] = stoch\['STOCHRSIk\_14\_14\_3\_3']

&nbsp;   

&nbsp;   macd = ta.macd(df\['Close'])

&nbsp;   df\['macd\_line'] = macd\['MACD\_12\_26\_9']

&nbsp;   

&nbsp;   df\['macd\_turning'] = (df\['macd\_line'] < df\['macd\_line'].shift(1)) \& \\

&nbsp;                        (df\['macd\_line'].shift(1) < df\['macd\_line'].shift(2))



&nbsp;   df\['sell\_signal'] = (

&nbsp;       (df\['rsi'] > 70) \&

&nbsp;       (df\['stoch\_k'] > 90) \&

&nbsp;       (df\['macd\_turning']) \&

&nbsp;       (df\['Close'] < df\['Open'])

&nbsp;   )

&nbsp;   

&nbsp;   return df

```



Conclusion

The Triple-Filter Stoch RSI \& MACD system is designed for patient reversal traders. It sacrifices trade frequency for higher-quality setups and performs best in moderate-trend or range-to-reversal environments. In powerful trending markets, additional filters like volume and macro trend alignment are necessary to avoid repeated counter-trend losses.



