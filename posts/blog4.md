The strategy uses two layers of confirmation before it even thinks about taking a trade:

Price Envelopes – the “rubber band” boundary
It starts with an 8‑period moving average (a short-term “fair price” line), then draws a small channel around it, shifted by about 0.22% above and below.

The upper band is “slightly too expensive” territory.

The lower band is “slightly too cheap” territory.
When the price bar touches or crosses one of these bands, it’s a sign price may have stretched away from its recent normal.

RSI (8) – the “momentum mood”
At the same time, it checks an 8‑period RSI, a fast oscillator that tells if recent buying or selling has been unusually strong.

RSI above 80 = overbought, buyers may have overdone it.

RSI below 25 = oversold, sellers may have overdone it.

A trade only appears when both agree:

Price is at or beyond an envelope band, and

RSI is in an extreme zone.

That’s the dual-filter logic: price is far from normal and momentum is stretched.

What’s strong and what’s risky about it
What’s good about this design:

The double filter (envelope + RSI) avoids many of the false signals that show up if you use RSI alone. You don’t trade just because RSI is low or high; price has to also be far from its average.

Using the median price (high+low)/2 as the source makes the calculations less sensitive to random spikes or long wicks.

There is a built‑in take‑profit and stop‑loss structure, defined as a small percentage move (about ±0.15%), so it does not leave exits completely to discretion.

Where it can easily struggle:

The envelope width of 0.22% is extremely narrow. On very volatile instruments, price will hit these bands constantly; on very quiet ones, almost never. A fixed percentage like this is not flexible enough across different markets or volatility regimes.

The TP/SL of ±0.15% is tiny. In many real markets, normal spread, fees, and tiny intrabar noise can easily hit these levels before a genuine move happens, causing a lot of small, annoying losses.

Like most mean‑reversion systems, it is fragile in strong trends. If a market is trending hard, price can keep riding the upper or lower side of the band and RSI can stay extreme for a long time. In those conditions, you get many small losing attempts instead of the neat snap-backs you want.

So, as-is, this is a sharp mean-reversion detector, but it needs better adaptation to volatility and trend to be truly robust.

How to make it more adaptive and robust
To upgrade this idea closer to a professional standard, several improvements help:

Make the bands volatility-based instead of fixed
Instead of always using a 0.22% distance, the envelope can be built using ATR (Average True Range) or a Bollinger/Keltner-style approach:

In high volatility, the channel widens automatically.

In quiet times, it narrows.
This keeps the band at a more consistent statistical distance from price across different conditions.

Don’t just check “RSI is low” – check if it’s turning
Rather than entering as soon as RSI drops below 25, wait for it to bend back toward the middle, which suggests the extreme move is actually losing strength. That reduces the chance of stepping in front of a move that’s still accelerating.

Detect when mean reversion is even appropriate
In markets that are clearly trending, mean reversion often performs poorly. A regime detector (for example, something that distinguishes trending vs. ranging conditions) can help turn this strategy off when the environment is wrong, and on when sideways, elastic behavior is more likely.

These changes turn a rigid, fixed-percentage script into a more context-aware system that responds to the market it’s trading.

The core logic in everyday language
If you explain the rules without any math symbols, they sound like this:

Buy idea:

Price has fallen to or below the lower edge of its short-term “fair value” channel.

RSI shows the market has been strongly sold recently (oversold zone).

The candle closes back above the lower band, suggesting sellers may be backing off.
→ That’s a signal the price might snap back up toward its average.

Sell idea:

Price has risen to or above the upper edge of its short-term channel.

RSI shows strong recent buying (overbought zone).

The candle closes back below the upper band, suggesting buyers may be losing steam.
→ That’s a signal the price might snap back down toward its average.

When a trade is taken, the original version aims for a very small gain (about +0.15%) and accepts a similarly small loss (about −0.15%). The intention is to take many tiny mean-reversion trades rather than waiting for big swings.

What the Python version does behind the scenes
The Python implementation simply turns this idea into something a computer can scan for:

It resamples price data into a 15‑minute rhythm, so that an 8‑period average actually covers a meaningful period of time.

It computes:

A median price and its 8‑period moving average (the basis).

Upper and lower envelopes around that average at the chosen percentage.

The 8‑period RSI on the same median price.

It then marks:

Buy signals when RSI is oversold and the bar reaches the lower envelope and closes back above it.

Sell signals when RSI is overbought and the bar reaches the upper envelope and closes back below it.

The result is a time series where each candle is tagged as potential buy, sell, or no trade, according to this mean reversion logic.

What this means for a normal trader
For a regular trader, you can think of this Envelope–RSI strategy as a tool that:

Tries to buy when price looks slightly too cheap for the recent range and mood.

Tries to sell when price looks slightly too expensive for the recent range and mood.

Works best in markets that like to revert to the middle rather than trend strongly in one direction.