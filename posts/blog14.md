

Title

The Wick Mathematics of Institutional Absorption: A Review of Delta Zones



Brief Description

This review examines the Delta Zones Buy/Sell Pressure indicator, an advanced order flow approximation tool. Instead of using tick-by-tick volume data, it analyzes candle wick structure to estimate where aggressive price movement is being absorbed. By applying statistical outlier detection, the system highlights areas where price rejection is strong enough to suggest institutional participation.



Delta Zones uses a geometric form of “delta” derived from candle shape and filters signals through a 3.5 standard deviation threshold. These extreme events are marked as absorption zones, which often act as future support or resistance levels.



Source Links

Source code: TradingView public Pine Script (Delta Zones Buy/Sell Pressure)

Pine Script Version: v5



Original Code Segment (Pine Script)

The core logic calculates a geometric delta from wick differences and then detects statistical outliers.



```pinescript

// Geometric Delta Calculation via Wick Mathematics

up\_delta = (open - low) - (high - close)

dn\_delta = (high - open) - (close - low)



// Statistical Outlier Detection

upper\_thresh = ta.stdev(pos\_delta, 20) \* 3.5

lower\_thresh = ta.stdev(neg\_delta, 20) \* 3.5



// Zone Trigger for Institutional Absorption

isBuyZone = up\_delta > upper\_thresh

```



Current Pros and Cons



Pros



\* Institutional Footprint Approximation: By focusing on extreme statistical events, the tool filters out minor retail activity and highlights potential smart money absorption.

\* Dynamic Support and Resistance: Delta Zones are plotted as visual areas that often act as future reaction levels.

\* Structural Awareness: ZigZag logic helps determine whether a delta spike occurs near a swing high or low, improving contextual relevance.



Cons



\* Low Signal Frequency: A 3.5 standard deviation threshold makes signals rare, which may not suit very active traders.

\* Wick Sensitivity: In low-liquidity or manipulated conditions, large wicks may produce misleading delta spikes.

\* Confirmation Lag: Zones are confirmed on candle close, introducing delay between the absorption event and signal visibility.



Weak Points Identified

The biggest weakness is the absence of volume confirmation. A large wick without strong volume does not necessarily represent institutional absorption. Without validating that significant trading activity occurred during the spike, the system may misinterpret random liquidity gaps as meaningful pressure.



Suggested Improvements



\* Add Volume Confirmation: Only validate delta spikes when volume is at least 20 percent above its 20-period average.

\* Introduce Zone Expiry Logic: Older zones should lose relevance over time unless retested, reducing chart clutter and focusing attention on fresh levels.

\* Align with Macro Trend: Only treat buy zones as high-probability when price is above a macro filter such as the 200 EMA.



Improved Code Segment (Reworked Pine Script)

This refined logic adds volume and trend filters.



```pinescript

// Added Volume and Macro Trend Confirmation

ema200 = ta.ema(close, 200)

volSMA = ta.sma(volume, 20)



// Refined Logic Gate

volConfirmed = volume > volSMA \* 1.2

trendAligned = (isBuyZone and close > ema200) or (isSellZone and close < ema200)



// Final Filtered Delta Signal

filteredDeltaSignal = (isBuyZone or isSellZone) and volConfirmed and trendAligned

```



Conclusion

Delta Zones Buy/Sell Pressure offers a unique way to estimate institutional absorption using candle structure and statistical extremes. It provides a map of potential reversal or reaction zones based on price rejection behavior. However, to reduce false signals and improve reliability, it should be combined with volume validation and macro trend filters so that identified zones are supported by both structural and capital flow context.





