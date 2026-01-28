

Title

The Volatility Filter: Auditing the itrade Signals QQE Framework



Brief Description

This strategy is a momentum-trend hybrid system designed to eliminate the jitter associated with traditional oscillators. It functions by calculating the volatility of the RSI itself rather than price action. By utilizing a Double-Smoothed RSI and a Wilder’s RSI-ATR buffer, the system creates dynamic trailing bands (Fast XLL Trailing Bands). A signal is generated only when the smoothed RSI crosses these volatility-adjusted thresholds, ensuring that entries are triggered by structural momentum shifts rather than minor price fluctuations.



Source Links

Source code: TradingView Public Pine Script (itrade signals)

Pine Script Version: v4



Original Code Segment (Pine Script)



```pinescript

// RSI Smoothing and Volatility Buffer

Rsi = rsi(src, RSI\_Period)

RsiMa = ema(Rsi, ll)

AtrRsi = abs(RsiMa\[1] - RsiMa)

MaAtrRsi = ema(AtrRsi, Wilders\_Period)

dar = ema(MaAtrRsi, Wilders\_Period) \* XLL



// Trailing Band Iterative Logic

longband := RSIndex\[1] > longband\[1] and RSIndex > longband\[1] ? max(longband\[1], newlongband) : newlongband

shortband := RSIndex\[1] < shortband\[1] and RSIndex < shortband\[1] ? min(shortband\[1], newshortband) : newshortband

```



Current Pros and Cons



Pros



Noise Attenuation: By calculating volatility on the RSI rather than price, the strategy remains remarkably stable during periods of erratic, low-volume price action.



Directional Discipline: The trailing band logic ensures that the system stays in a trend until a definitive momentum reversal occurs, preventing premature exits.



Mathematical Sophistication: The use of the XLL Factor (4.238) provides a Golden Ratio inspired buffer that balances sensitivity with reliability.



Cons



Lag in Reversals: Due to the double EMA smoothing of both the RSI and the ATR-RSI, signals can occasionally trigger after the initial move has already matured.



Lack of Volume Context: The strategy is purely mathematical and does not account for whether a band cross is supported by institutional volume.



Standard Threshold Risks: While the bands are dynamic, the strategy still relies on fixed threshold crosses (e.g., 50 level) which can lead to whipsaws in sideways markets.



Weak Points Identified



The primary vulnerability is Lag-Induced Drawdown. Because the trailing bands move based on double-smoothed data, a sharp, violent reversal in price may not trigger a band cross until a significant portion of the move has occurred. Furthermore, the system lacks a Macro Trend Filter, often signaling Long in a structural bear market or Short in a macro bull market, leading to a lower win rate during strong trending regimes.



Suggested Improvements



Institutional Trend Filter: Add a 200-period EMA filter. Only allow Long signals if price is above the 200 EMA to align with macro liquidity.



Dynamic XLL Scaling: Implement a variable XLL factor based on the ADX (Average Directional Index) to tighten the bands during ranges and widen them during strong trends.



Volume Confirmation: Require a volume spike (Relative Volume > 1.5) on the bar of the signal to ensure the crossover is backed by capital commitment.



Improved Code Segment (Reworked Python Implementation)



```python

import pandas as pd

import pandas\_ta as ta

import numpy as np



def calculate\_optimized\_itrade\_signals(df, rsi\_len=14, smooth=5, xll=4.238):

&nbsp;   # 1. Resample and Indicators

&nbsp;   res = df.resample('15T').agg({'Open': 'first', 'High': 'max', 'Low': 'min', 'Close': 'last', 'Volume': 'sum'}).dropna()

&nbsp;   res\['rsi'] = ta.rsi(res\['Close'], length=rsi\_len)

&nbsp;   res\['rsi\_ma'] = ta.ema(res\['rsi'], length=smooth)

&nbsp;   res\['ema\_trend'] = ta.ema(res\['Close'], length=200)

&nbsp;   

&nbsp;   # 2. RSI-Volatility (DAR) Calculation

&nbsp;   wilders\_per = rsi\_len \* 2 - 1

&nbsp;   res\['atr\_rsi'] = abs(res\['rsi\_ma'].shift(1) - res\['rsi\_ma'])

&nbsp;   res\['ma\_atr\_rsi'] = ta.ema(res\['atr\_rsi'], length=wilders\_per)

&nbsp;   res\['dar'] = ta.ema(res\['ma\_atr\_rsi'], length=wilders\_per) \* xll

&nbsp;   

&nbsp;   # 3. Trailing Bands Logic

&nbsp;   long\_band = np.zeros(len(res))

&nbsp;   short\_band = np.zeros(len(res))

&nbsp;   trend = np.zeros(len(res))

&nbsp;   

&nbsp;   for i in range(1, len(res)):

&nbsp;       new\_l = res\['rsi\_ma'].iloc\[i] - res\['dar'].iloc\[i]

&nbsp;       new\_s = res\['rsi\_ma'].iloc\[i] + res\['dar'].iloc\[i]

&nbsp;       long\_band\[i] = max(long\_band\[i-1], new\_l) if res\['rsi\_ma'].iloc\[i-1] > long\_band\[i-1] else new\_l

&nbsp;       short\_band\[i] = min(short\_band\[i-1], new\_s) if res\['rsi\_ma'].iloc\[i-1] < short\_band\[i-1] else new\_s

&nbsp;       

&nbsp;       if res\['rsi\_ma'].iloc\[i] > short\_band\[i-1]:

&nbsp;           trend\[i] = 1

&nbsp;       elif res\['rsi\_ma'].iloc\[i] < long\_band\[i-1]:

&nbsp;           trend\[i] = -1

&nbsp;       else:

&nbsp;           trend\[i] = trend\[i-1]



&nbsp;   res\['long\_signal'] = (trend == 1) \& (res\['Close'] > res\['ema\_trend'])

&nbsp;   return res

```



Conclusion



The itrade signals script is a robust foundational tool for volatility-adjusted trend following. While its QQE-based logic provides exceptional noise filtering, its performance is highly dependent on market context. By adding a 200 EMA trend filter and dynamic volatility scaling, the system shifts from a naive oscillator into a professional-grade momentum participation engine capable of surviving diverse market regimes.



