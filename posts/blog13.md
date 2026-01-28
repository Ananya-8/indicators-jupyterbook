

Title

Quantifying Market Sentiment: A Review of the Traders Dynamic Index (TDI)



Brief Description

This review examines the TDI (Traders Dynamic Index) with RSI Divergence, a sentiment and volatility oscillator that blends momentum and market structure into one framework. Instead of using RSI alone, the TDI applies moving averages and volatility bands to the RSI itself, helping traders understand not only direction, but also the strength and expansion of sentiment.



The indicator works as both a trend-following and reversal system. It compares a fast RSI average (Price Line) with a slower average (Signal Line) and places volatility bands around a Market Base Line to identify when momentum is stretched or when the market is shifting regimes.



Source Links

Source code: TradingView public Pine Script (TDI - Traders Dynamic Index + RSI Divergences + Buy/Sell Signals by ZyadaCharts)

Pine Script Version: v4



Original Code Segment (Pine Script)

The TDI’s core logic applies volatility bands to RSI rather than to price.



```pinescript

// TDI Foundation: RSI and Volatility Bands

r = rsi(close, 14)

ma = sma(r, 34) // Market Base Line (MBL)

offs = 1.6185 \* stdev(r, 34)

up = ma + offs // Volatility Band Upper

dn = ma - offs // Volatility Band Lower



// Fast and Slow RSI Moving Averages

fast\_ma = sma(r, 2) // RSI Price Line

slow\_ma = sma(r, 7) // Trade Signal Line

```



Current Pros and Cons



Pros



\* Comprehensive Context: The TDI shows momentum relative to its own volatility structure, not just raw RSI values.

\* Multi-Layered Signals: Traders can use short-term crossovers, RSI vs MBL shifts, and volatility band extremes for different styles of entry.

\* Integrated Divergence: Built-in divergence detection helps identify both trend reversals and continuation patterns.



Cons



\* High Multicollinearity: Using multiple moving averages on the same RSI source can cause repeated, noisy signals in sideways markets.

\* Lag in Volatile Markets: The 34-period Market Base Line reacts slowly during fast regime shifts.

\* Signal Overload: Without higher-level filters, crossovers and divergences can conflict and confuse decision-making.



Weak Points Identified

One major weakness is directional ambiguity. The TDI can generate reversal-style signals (like overbought sells) during strong bull trends, leading traders to fight institutional momentum.



Another issue is fixed parameter sensitivity. The 34-period base line and band calculations may not suit both short-term scalping charts and long-term swing charts equally well.



Suggested Improvements



\* Add a Macro Trend Filter: Use a 200 EMA on price. Only take long signals when price is above the 200 EMA and TDI is aligned bullish.

\* Use Adaptive Volatility Scaling: Instead of fixed deviation logic, incorporate ATR-based scaling to better match current market speed.

\* Require Extreme Conditions for Reversals: Only treat crossovers as strong reversal signals if RSI is outside its volatility bands.



Improved Code Segment (Reworked Pine Script)

This version adds macro trend alignment and filters signals for stronger context.



```pinescript

// Added Macro Trend and Signal Filtering

ema200 = ta.ema(close, 200)

is\_bullish\_regime = close > ema200



// High-Conviction Long Entry

long\_signal = ta.crossover(fast\_ma, slow\_ma) and slow\_ma > ma and is\_bullish\_regime



// Exhaustion Filter

is\_overextended = r > up or r < dn

```



Conclusion

The TDI is a powerful sentiment framework that reveals how momentum behaves relative to its own volatility environment. It offers deeper insight than a standalone RSI by showing expansion, contraction, and divergence in one view. However, to avoid false reversals and lag-related noise, it should be paired with macro trend filters and volume context to ensure signals align with broader market direction.



