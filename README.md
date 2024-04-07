# Market-Neutral Trading Strategies

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/vasiliadi/market-neutral-trading-basics/blob/main/Pair_Trading.ipynb)
[![Binder](https://mybinder.org/badge_logo.svg)](https://github.com/vasiliadi/market-neutral-trading-basics/blob/main?labpath=Pair_Trading.ipynb)
[![Launch in Deepnote](https://deepnote.com/buttons/launch-in-deepnote-white-small.svg)](https://deepnote.com/launch?url=https%3A%2F%2Fgithub.com%2Fvasiliadi%2Fmarket-neutral-trading-basics%2Fblob%2Fmain%2FPair_Trading.ipynb)
[![Open In SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/vasiliadi/market-neutral-trading-basics/blob/main/Pair_Trading.ipynb) 

Market-neutral trading strategies aim to generate returns that are not correlated with the overall market movements. 

Here are some common market-neutral trading variants:

1. **Statistical Arbitrage**: This strategy uses mathematical models and quantitative techniques to identify temporary mispricing between related securities or portfolios. It involves taking offsetting long and short positions to capture the pricing discrepancies while maintaining a market-neutral exposure.

2. **Pair Trading**: This strategy involves identifying two stocks or securities with a high historical correlation and taking a long position in the undervalued security while shorting the overvalued one. The goal is to profit from the convergence of the prices when the divergence between the two securities narrows.

3. **Convertible Arbitrage**: This strategy involves taking a long position in a convertible bond and hedging the equity risk by shorting the underlying stock. The goal is to capture the spread between the convertible bond and the underlying stock while remaining market-neutral.

4. **Risk Arbitrage**: This strategy focuses on mergers, acquisitions, or other corporate events that create temporary pricing discrepancies between the target company's stock and the acquirer's stock or cash offer. It involves taking positions to capture the spread while hedging against market risk.

5. **Capital Structure Arbitrage**: This strategy exploits pricing inefficiencies between different securities issued by the same company, such as common stock, preferred stock, and bonds. It involves taking offsetting positions in these securities to capture the mispricing while maintaining a market-neutral exposure.

These are just a few examples of market-neutral trading strategies. The common theme among these strategies is the attempt to generate returns that are uncorrelated with the overall market movements by exploiting pricing inefficiencies and taking offsetting long and short positions.

# Cointegration in time series

Today I'll consider only **statistical arbitrage** and its most common approach of **cointegration** in time series.

Cointegration is a statistical concept used in econometrics and time series analysis to understand the long-term relationship between price series, particularly those that exhibit trends.

Cointegration applies to non-stationary time series, meaning their trends and volatility can change over time. But if a linear combination of these non-stationary series becomes stationary (meaning trends and fluctuations become constant), then they are considered cointegrated.

Cointegration helps us move beyond simple correlation, which can be misleading for trending data. It reveals a price series's inherent connection in the long run.

# Backtesting

You can backtest your strategy with [vectorbt Pro](https://vectorbt.dev/) or [backtrader](https://www.backtrader.com/). Backtesting is a whole other story...

# How to improve results?

## Moving window

Try to use [FLS](https://www2.econ.iastate.edu/tesfatsi/flshome.htm) or [RollingOLS](https://www.statsmodels.org/dev/generated/statsmodels.regression.rolling.RollingOLS.html#statsmodels.regression.rolling.RollingOLS) or even [XGBRegressor](https://xgboost.readthedocs.io/en/latest/python/python_api.html#xgboost).

The moving window problem. Small window = constantly changing hedge ratio. You need to either look for an optimal window or change your exit strategy.

The spread will have a trend component one way or another, and this generates losses. Using the [SMA](https://www.investopedia.com/terms/s/sma.asp) to define a conditionally zero axis will be very laggy. But you should try more advanced versions, such as [exponential smoothing](https://www.statsmodels.org/dev/examples/notebooks/generated/exponential_smoothing.html), [Hodrick-Prescott filter](https://www.statsmodels.org/stable/generated/statsmodels.tsa.filters.hp_filter.hpfilter.html), [Kalman Filter](https://pykalman.github.io/), or other variants of [MA](https://github.com/twopirllc/pandas-ta?tab=readme-ov-file#overlap-33). Curves are tested for the minimization of deviations from the reference.

The position entry should be checked. It's necessary to cut off the intervals where there is a lag from the reference curve.

Try entering with additional indicators.
Try additional indicators like [RSI](https://www.investopedia.com/terms/r/rsi.asp), [CCI](https://www.investopedia.com/terms/c/commoditychannelindex.asp), [Momentum](https://www.investopedia.com/terms/m/momentum.asp), [MACD](https://www.investopedia.com/terms/m/macd.asp) or combinations of indicators like [EMA](https://www.investopedia.com/terms/e/ema.asp)+[BB](https://www.investopedia.com/terms/b/bollingerbands.asp)+[Z-score](https://www.investopedia.com/terms/z/zscore.asp), etc.

## Standartization

If you use [Z-score](https://www.investopedia.com/terms/z/zscore.asp), you can take frequent trades with a small profit in the range -2:2 or less frequent trades -3:3 with a large profit. [Z-score](https://www.investopedia.com/terms/z/zscore.asp) and [BB](https://www.investopedia.com/terms/b/bollingerbands.asp) is good starting point.

## Spread mirroring

Spread mirroring problem. Sometimes (long X, short Y) != (long Y, short X), and then you should look at [ODR](https://docs.scipy.org/doc/scipy/reference/odr.html) to solve this problem.

## Liquidity problem

Go into illiquid paper first, then only into liquid paper.