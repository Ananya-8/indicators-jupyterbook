Title

The Anti-Spam Sentinel: Auditing Cleaned EMA Signals \& ATR Risk Framework



Brief Description

This indicator is a Dual-Horizon momentum system built to prevent signal clustering during choppy market conditions. It uses a fast EMA (9) for trade execution and a slow EMA (200) as a global trend filter. A built-in state-management rule forces signals to alternate (Buy → Sell → Buy), reducing repeated entries in the same direction. The system also includes ATR-based risk management that calculates multiple take-profit tiers based on market volatility.



Source Links

Source code: TradingView Public Pine Script (Cleaned EMA Signals by unknown author)

Pine Script Version: v5



Original Code Segment (Pine Script)

The logic combines a fast EMA trigger, a slow EMA filter, and an anti-spam state variable.



```pinescript

// Trend Filter \& Execution Logic

emaFast = ta.ema(close, 9)

emaSlow = ta.ema(close, 200)



// Anti-Spam State Management

var int lastSignal = 0 // 0: none, 1: buy, -1: sell



crossBuy  = ta.crossover(close, emaFast) and emaFast > emaSlow

crossSell = ta.crossunder(close, emaFast) and emaFast < emaSlow



if crossBuy and lastSignal != 1

&nbsp;   lastSignal := 1

&nbsp;   // Trigger Buy Signal

```



Current Pros and Cons



Pros



• Anti-Spam Protection: The state variable prevents multiple same-direction signals during a single trend, improving discipline.

• Volatility-Normalized Exits: ATR-based targets and stops adjust to changing market conditions.

• Trend Bias Filter: Requiring EMA 9 to align with EMA 200 keeps trades in the macro trend direction.



Cons



• Slow Trend Recognition: Waiting for EMA alignment can delay entries into new macro trends.

• Fixed Profit Multipliers: Static ATR reward levels may limit gains in strong trending moves.

• Close-Price Dependency: EMA crosses based on close can still be influenced by intrabar fakeouts.



Weak Points Identified

The main weakness is horizontal, range-bound markets. When EMA 200 is flat, EMA 9 can cross price frequently, creating whipsaws. Even with anti-spam logic, repeated reversals can slowly erode capital.



Suggested Improvements



• ADX Trend Filter: Only allow trades when ADX > 20 to confirm trending conditions.

• Breakeven Rule: Move stop-loss to entry after the first profit target is reached.

• Volume Confirmation: Validate crossovers only when volume exceeds the recent average.



Improved Code Segment (Reworked Python Implementation)

This version adds ADX filtering and preserves the anti-spam state logic.



```python

import pandas as pd

import pandas\_ta as ta



def calculate\_cleaned\_ema\_signals(df, fast=9, slow=200, adx\_min=20):

&nbsp;   df\['ema\_fast'] = ta.ema(df\['Close'], length=fast)

&nbsp;   df\['ema\_slow'] = ta.ema(df\['Close'], length=slow)

&nbsp;   df\['adx'] = ta.adx(df\['High'], df\['Low'], df\['Close'])\['ADX\_14']

&nbsp;   

&nbsp;   df\['cross\_up'] = (df\['Close'] > df\['ema\_fast']) \& (df\['Close'].shift(1) <= df\['ema\_fast'].shift(1))

&nbsp;   df\['cross\_dn'] = (df\['Close'] < df\['ema\_fast']) \& (df\['Close'].shift(1) >= df\['ema\_fast'].shift(1))

&nbsp;   

&nbsp;   signals = \[]

&nbsp;   last\_signal = 0

&nbsp;   

&nbsp;   for i in range(len(df)):

&nbsp;       if df\['cross\_up'].iloc\[i] and (df\['ema\_fast'].iloc\[i] > df\['ema\_slow'].iloc\[i]) and \\

&nbsp;          (df\['adx'].iloc\[i] > adx\_min) and last\_signal != 1:

&nbsp;           signals.append(1)

&nbsp;           last\_signal = 1

&nbsp;       elif df\['cross\_dn'].iloc\[i] and (df\['ema\_fast'].iloc\[i] < df\['ema\_slow'].iloc\[i]) and \\

&nbsp;            (df\['adx'].iloc\[i] > adx\_min) and last\_signal != -1:

&nbsp;           signals.append(-1)

&nbsp;           last\_signal = -1

&nbsp;       else:

&nbsp;           signals.append(0)

&nbsp;           

&nbsp;   df\['refined\_signal'] = signals

&nbsp;   return df

```



Conclusion

Cleaned EMA Signals is a strong trend-following foundation that reduces overtrading and improves discipline. It performs best in clearly trending markets with expanding volatility. However, during sideways conditions, additional filters like ADX and volume confirmation are essential to avoid repeated small losses.



