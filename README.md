# Forever Model

By Canto Lab on TradingView.

A from-scratch Pine Script v6 implementation of the FVG + SMT model concept, as publicly taught across ICT-derived methodology (SMT divergence, fair value gaps, order blocks, external range liquidity). This is an independent, open source tool built for personal study and for anyone who does not want to pay a monthly fee to plot an FVG, an SMT line, and an order block.

## Disclaimer

I do not recommend trading this strategy as is. Read the rest of this README before you download the indicator for better understanding.

## How this was built

This script was written independently, from scratch, based on publicly available educational content explaining SMT divergence, fair value gaps, and order block confirmation (YouTube explanations, public discussion of the methodology, ICT/SMT explanations etc). No proprietary source code from any commercial indicator was accessed, copied, or referenced in building this. The logic, structure, and code here are my own.

## Why something like this does not work

### Fair Value Gaps

Fair value gaps are extremely common. Take any price leg that isn't choppy (candles giving too many overlapping wicks) and it will have a fair value gap on it close to 99% of the time. That frequency is the problem. A pattern that shows up almost every time price moves in a straight line for three candles isn't rare enough to carry information on its own, even when you're referencing an HTF gap down onto a lower timeframe. HTF context narrows down where you're looking, it doesn't change how common the underlying pattern is at the timeframe it's actually detected on.

You can verify this yourself with a simple script. Ask Claude to build something using:

```
bullish FVG = low[1] > high[3]
bearish FVG = high[1] < low[3]
```

Note the `1` and `3` offsets, not `0` and `2` — you want the candle fully confirmed before checking the gap, and it's an easy off-by-one for an LLM to get wrong. Also explicitly tell it not to include any mitigation or deletion rules, so gaps stay plotted once they form instead of disappearing the moment price fills them. Run that on any chart and count how often it fires. That count is your answer.

### SMT

Divergence trading has real roots. Going back to strategies from the 1980s, some divergence-based approaches were genuinely profitable, but only because the people running them understood why the divergence existed in the first place. Take the S&P 500 and the Nasdaq: both track the US economy, but the Nasdaq is far more concentrated in tech. If bad news hits tech specifically, the Nasdaq drops faster than the S&P because the S&P has less exposure to that sector. Given enough time, the two tend to converge again as the broader economy moves and the Nasdaq recovers. Even so, this isn't a great trading strategy on its own, because simply buying and holding the S&P or the Nasdaq tends to outperform trying to time the reconvergence. Investing for the long run beats trying to trade the divergence.

ICT/SMC traders apply this same concept intraday. The issue is that on an intraday timeframe, there is almost always some divergence between two correlated instruments if you look closely enough. If there's specific news on a single name, Nvidia or Apple, an earnings call, a rumor, anything, that alone can create a "divergence" between correlated indices or pairs for a reason that has nothing to do with the broader directional thesis you're trying to trade. Sometimes it happens for no discernible reason at all.

So when you combine an intraday divergence that may not carry any real information with a pattern like a fair value gap that happens constantly, you end up with a setup that is, at best, mediocre.

### Order Blocks

I won't argue against order blocks themselves. As an entry pattern, they're common and reasonable, provided your narrative or research is right first. The mistake is treating the entry tool as if it were the model itself.

Think of it like a pinbar. A pinbar is a well documented reversal candle pattern, but if you take every single pinbar that forms on a chart with no other context, you lose money, because the narrative behind the move isn't there most of the time. The pinbar looks the same whether it's a genuine reversal or just noise, and without something outside the candle telling you which one you're looking at, you can't tell them apart in advance. An order block has the same issue. The shape on the chart doesn't tell you if the move behind it was driven by something real or if it's just where price happened to consolidate before continuing.

### So does the strategy have any value?

Maybe. My honest suggestion is to build your directional bias from information outside the chart entirely, news, upcoming earnings expectations, whatever is actually driving the instrument, and then use any simple entry tool to act on that bias. Stacking FVGs, SMT, and order blocks together can make the process feel more rigorous than it is. If your bias is genuinely right, you don't need a complex entry, you could enter close to blindly off that bias and it would still work more often than not. Take that with a grain of salt, it's my personal take and not a claim I'm backtesting for you in this README.

## Why open source

This should not be a paid product. There is nothing secretive about this indicator, it's a fair value gap detector, an SMT confirmation, and an order block, wired together with some conditional logic on top of a couple of `request.security` calls. If a tool like this actually had the edge it's marketed as having, it wouldn't be sold for $45 a month, it would be worth something closer to $10,000, and it wouldn't be for sale at all. This repo is my attempt at a clean, properly organized version of the same tool, for anyone who wants to study it or build on top of it for free.

## Features

- HTF fair value gap and SMT based detection
- LTF order block detection
- Auto SMT pair detection
- Standard deviation projections for targets
- ERL and standard deviation tracking markers
- On chart dashboard

## Installation

1. Open TradingView and go to the Pine Editor.
2. Create a new indicator and paste in the contents of `foreverModel.pine`.
3. Save and add it to your chart.

## Contributing

Pull requests welcome, especially around cleaner SMT pair resolution for less common instruments or additional invalidation logic.

## License

MIT. Do whatever you want with it
