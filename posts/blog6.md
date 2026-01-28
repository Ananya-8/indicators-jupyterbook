

Title

Mapping Institutional Footprints: A Review of Buyside \& Sellside Liquidity



Brief Description

This script is based on Smart Money Concepts (SMC). It identifies high-interest zones where liquidity is clustered, specifically “Equal Highs” and “Equal Lows.” These are areas where many retail traders place stop-loss orders, which institutions often target to enter large positions. The tool also identifies Fair Value Gaps (FVGs), which represent price imbalances that act as magnets for future price movement.



Source Links

Source code: TradingView public Pine Script (Buyside \& Sellside Liquidity by LuxAlgo)

Pine Script Version: v5



Original Code Segment (Pine Script)

The script identifies liquidity by detecting pivot points (local highs and lows) and checking whether new pivots align with previous ones within a defined margin.



```pinescript

// Pivot Detection for Liquidity

liqLen = 7 // Detection Length

ph = ta.pivothigh(high, liqLen, liqLen)

pl = ta.pivotlow(low, liqLen, liqLen)



// Margin logic: Checking if new pivots align with old ones

is\_within\_margin = math.abs(ph - existing\_level) < margin\_threshold

```



It also tracks liquidity voids or imbalances where price moved too fast to be efficiently filled.



```pinescript

// Fair Value Gap (FVG) Detection

is\_fvg\_up = low > high\[2]  // Bullish Imbalance

is\_fvg\_down = high < low\[2]  // Bearish Imbalance

```



Current Pros and Cons



Pros



\* Institutional Alignment: Instead of using lagging averages, this tool highlights where large players are likely to hunt for stop orders, allowing investors to align with institutional behavior.

\* Advanced Visual Profiling: The script highlights unmitigated zones (levels not yet revisited), which often act as precise price targets.

\* Dynamic Gap Analysis: Fair Value Gaps show market imbalances that price often returns to fill, giving traders logical target areas.



Cons



\* High Subjectivity: The margin and detection length settings are sensitive. Small changes can drastically change which zones appear on the chart.

\* Contextual, Not Signal-Based: The tool shows where price may react, but it does not provide clear entry timing.

\* Visual Overload: On lower timeframes, too many levels and zones can clutter the chart and confuse newer traders.



Weak Points Identified

The primary weakness is the lack of execution timing. Just because a liquidity pool exists does not mean price will reverse immediately; price can easily break through it before reacting.



The script also lacks volume-at-price confirmation. It shows price levels but not how much actual trading activity occurred there. Finally, there is no trend filter, so traders might try to buy a liquidity zone even when the overall market is strongly bearish.



Suggested Improvements



\* Add a Trend Bias Filter (EMA 200): Only consider sellside liquidity sweeps (buy setups) when price is above the 200 EMA. This aligns trades with the macro trend.

\* Integrate Volume Profile: Combine liquidity zones with a Volume Profile indicator. A zone that overlaps with a high-volume node is more reliable.

\* Implement a Reversal Trigger: Instead of entering immediately at a zone, require a reversal candlestick pattern (Pin Bar, Engulfing candle, etc.) inside the liquidity area.



Improved Code Segment (Reworked Pine Script)

This improved logic adds a trend filter and a sweep confirmation requirement.



```pinescript

// Institutional Filter

ema200 = ta.ema(close, 200)

is\_bullish\_regime = close > ema200



// Sweep Confirmation

// Price must dip into Sellside Liquidity and then close back above it

price\_swept\_low = low < sellside\_level and close > sellside\_level



// Refined Institutional Entry

buy\_trigger = is\_bullish\_regime and price\_swept\_low and ta.bullish\_reversal\_pattern

```



Conclusion

The Buyside \& Sellside Liquidity tool provides a deep understanding of market structure and institutional behavior. It shifts focus from retail indicators to professional price action. However, since it is a profiling tool rather than a signal generator, disciplined execution is required. By adding a macro trend filter and waiting for a sweep confirmation with a reversal pattern, traders can turn these liquidity zones into high-probability, risk-controlled trade setups.





