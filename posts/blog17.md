

Title

Geometric Precision: A Review of Intraday Gann Angles (Square of 9)



Brief Description

This review examines the Intraday Buy/Sell using Gann Angles indicator, a mathematical pivot framework inspired by W.D. Gann’s Square of Nine theory. Instead of relying on lagging indicators, this system projects support and resistance levels from a single anchor price, usually the current day’s open.



The method assumes price behaves in geometric cycles. By taking the square root of the anchor price and applying fixed increments, the indicator projects rotational price levels such as 45°, 90°, and 180°. These levels act as predefined intraday supply and demand zones.



Source Links

Source code: TradingView public Pine Script (Intraday Buy/Sell using Gann Angles - RiTz by Keanu\_ritz)

Pine Script Version: v5



Original Code Segment (Pine Script)

The core logic calculates price rotations using the square root of the anchor price.



```pinescript

// The Mathematical Seed (Square Root of Price)

float seed = request.security(syminfo.tickerid, "D", open)

float sq\_root = math.sqrt(seed)



// Square of 9 Increments (Geometric Rotations)

buy\_entry  = math.pow(sq\_root + 0.125, 2) // 45-degree rotation

buy\_target = math.pow(sq\_root + 0.25,  2) // 90-degree rotation

sell\_entry = math.pow(sq\_root - 0.125, 2)

```



Current Pros and Cons



Pros



\* Fixed Anchoring: Levels are defined at the market open, allowing structured planning without emotional intraday adjustments.

\* Mathematical Consistency: The levels do not repaint or shift during the session, offering a stable reference framework.

\* Confluence Potential: Gann levels often align with volume nodes, Fibonacci levels, or psychological price zones.



Cons



\* Volatility Blindness: The geometric increments are static and do not adapt to changing volatility conditions.

\* No Directional Bias: Both buy and sell levels are presented simultaneously, which can lead to overtrading in sideways markets.

\* Anchor Sensitivity: If the session open is distorted by a gap or irregular print, all projected levels may be misaligned.



Weak Points Identified

The key structural weakness is static scaling. The fixed rotational increments do not account for the asset’s Average True Range. On high-volatility days, price may slice through levels easily. On quiet sessions, price may not reach even the first projected level.



Suggested Improvements



\* Volatility-Adjusted Rotations: Scale the square root increments relative to ATR so projected levels expand or contract with market conditions.

\* Trend Filter Integration: Use a 200 EMA to only take buy signals above the macro trend and sell signals below it.

\* Relative Volume Confirmation: Validate level reactions only when accompanied by elevated trading volume.



Improved Code Segment (Reworked Python Logic)



```python

import pandas as pd

import numpy as np



def calculate\_gann\_refined(df):

&nbsp;   daily\_open = df\['Open'].resample('D').first()

&nbsp;   df\['anchor'] = df.index.normalize().map(daily\_open)

&nbsp;   

&nbsp;   df\['sq\_root'] = np.sqrt(df\['anchor'])

&nbsp;   

&nbsp;   df\['gann\_buy'] = (df\['sq\_root'] + 0.125)\*\*2

&nbsp;   df\['gann\_t1']  = (df\['sq\_root'] + 0.25)\*\*2

&nbsp;   

&nbsp;   df\['atr'] = df\['High'].rolling(14).max() - df\['Low'].rolling(14).min()

&nbsp;   df\['is\_reachable'] = (df\['gann\_buy'] - df\['Close']) < (df\['atr'] \* 1.5)

&nbsp;   

&nbsp;   return df

```



Conclusion

The Intraday Gann Angles indicator offers a structured, geometry-based roadmap for intraday trading. It provides predefined pivot levels rooted in mathematical symmetry rather than reactive calculations. However, its effectiveness increases significantly when paired with volatility-based scaling and trend filters to ensure the projected levels remain realistic under current market conditions.





