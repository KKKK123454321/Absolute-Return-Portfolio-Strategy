# Absolute-Return-Portfolio-Strategy

## 1. Introduction
  Main asset allocation: Stocks, Bonds, Gold.
  Allocation weight calculation method: Timing indicators for stocks, bonds, and gold.

  - Timing indicator for stocks and bonds: Utilizing the stock-bond valuation ratio (ERP) and resonance signals generated by stock market volume-price indicators (moving averages + trading volume) to determine whether to increase or decrease positions.
  - Timing indicator for gold: Combining changes in US Treasury bond yields, VIX hedging, and gold price momentum to generate signals for adjusting positions in gold.

## 2. Overflow
### Timing Indicator for Stocks and Bonds
#### Equity Risk Premium Indicator
  ERP refers to the difference between the expected returns of the stock market and the bond market. It represents the excess return that investors anticipate from investing in stocks compared to bonds. However, Chinese index valuations are generally lower, and ERP tends to be greater than 0 in the long term. Therefore, the absolute value reference has limited significance. Instead, we adopt the rolling five-year percentile of ERP as an indicator of the relative attractiveness of stocks compared to bonds.
  - [ ] Signal indicator: Five-year percentile of ERP - Index-weighted moving average (window period = 120 trading days). As ERP is a valuation-based indicator, the market may not immediately reverse when ERP is at a high/low level. Therefore, using weighted ERP percentiles for long-term data smoothing is implemented with a default window period of 120 trading days.
  - [ ] Signal generation:
    - [ ] ERP[70%，30%]; Signal[1，0，-1]
    - [ ] Backtesting period: 2010-2022
    - [ ] Signal frequency: Monthly (M)
#### Stock Volume-Price Indicators
  ERP indicator has an issue of premature entry/exit. Therefore, we use short-term volume-price signals as a starting point for market trends. 
  - Individual Moving Average Signal: Backtesting results of a strategy based on the 5-day moving average on the CSI 800 Index.
Backtesting Results: For monthly holding periods, the short-term moving average timing performs better than the long-term, with a win rate of more than 55%.
  - Individual Volume Signal: If the signal's trading volume on that day is greater than the average volume of the past 5 days, it is considered as increased volume, generating a buy signal. Conversely, if the volume is lower, it generates a bearish signal.
    - [ ] Backtesting Results: The win rate is approximately 60%.
  - Volume-Price Indicator: Increased volume with an upward move + breaking below the moving average: When both the trading volume and moving average generate a bullish signal, it triggers a buy signal. On the other hand, if the moving average generates a bearish signal, it triggers a sell signal.
#### Composited Signal
  *Weighted ERP percentile signal + Volume-Price indicator signal (MA + Volume)* : 
  - If the ERP signal generates a bullish signal for the stock market, and the volume-price indicator shows increased volume with an upward move, the CSI 800 Index generates a signal to add holdings (1). If both signals indicate bearish sentiments, the CSI 800 Index generates a signal to reduce holdings (-1), otherwise, no signal is generated (0).
  - Backtesting Results: The timing effect of the combined timing indicator on the CSI 800 Index is evident. The combination of signals significantly improves the net value compared to using ERP signals alone, and it also provides better drawdown control. The annualized return rate is 12%.

### Gold Timing
#### US Treasury Yields
  There is a clear negative correlation between gold and US Treasury yields.
  - [ ] Signal generation: We use the N-day simple moving average (MA) of the 10-year US Treasury real yield to depict the direction of changes in real yields. If the US Treasury real yield > MA(20), it indicates an upward trend in real yields, triggering a signal to reduce holdings in gold (-1). Conversely, if the real yield is below the moving average, it generates a signal to increase holdings in gold (1).
    - [ ] Backtesting period: 2004 - 2022
    - [ ] Signal Frequency: Monthly (M)
    - [ ] Results: The timing effect of the US Treasury yield moving average (N=20) on gold shows that the timing strategy can outperform a buy-and-hold approach for gold, especially in the past decade where the outperformance is more pronounced. The strategy has an annualized return rate of 11%.

#### VIX
  The CBOE's VIX index measures the 30-day annualized implied volatility of the S&P 500. Implied volatility tends to rise during market turbulence or economic recessions. When the market is volatile, gold's safe haven properties tend to improve, and funds may seek refuge in assets such as gold. It is generally believed that when the VIX is above 20, market risk sentiment begins to heat up.
  - [ ] Signal generation: If the maximum value of the VIX index closing prices over a rolling 15-day period > 20, it indicates an overall increase in market risk, triggering a signal to increase holdings in gold (1). Otherwise, no signal is generated (0).
    - [ ] Backtesting period: 2004 - 2022
    - [ ] Signal Frequency: Monthly (M)
    - [ ] Results: The signal captured the upward movement of gold during the market turmoil around 2008 and avoided the one-sided uptrends in the US stock market from 2012-2014 and 2015-2018. It also captured the volatile trends of gold when market panic subsided.

#### Gold Price Momentum
  The gold price exhibits distinct periodic trends, and we can capture the direction of gold's own movement using gold price momentum.
  - [ ] Signal generation: Gold price N-day momentum = (Gold price today / Gold price N days ago) - 1 , Parameters [20, 60, 250]; Signals [1,0]
    - [ ] Backtesting period: 2004 - 2022
    - [ ] Signal Frequency: Monthly (M)
    - [ ] Results:  The 250-day momentum signal for gold demonstrated the best performance in terms of timing.

#### Composited Signal
  *Moving Average (MA) Signal of US Treasury Yield + VIX Index Signal + Gold Price Momentum Signal* 
  - [ ] Signal Parameters: If the net signal direction of the three indicators mentioned above is >= 2, it generates a signal to increase gold holdings (1). If the net signal is < 0, it generates a signal to reduce gold holdings (-1). Otherwise, no signal is generated (0)
  - [ ] Backtesting period: 2004 - 2022
  - [ ] Signal Frequency: Monthly (M)
  - [ ] Results:  Since 2004, holding gold unilaterally yielded an annualized return of 8%. Using the timing indicator, the annualized return was 10% with a win rate of 63%.

### 3. Results
#### Position before and after incorporating Timing Indicators
  - Before:
    - [ ] Stocks: 97%
    - [ ] Bonds: 2%
    - [ ] Gold: 1%
  - After (Average Position):
    - [ ] CSI 800: 17.61%
    - [ ] ChinaBond New Composite Index: 77.04%
    - [ ] Gold AU9999: 5.35%

#### Portfolio performance
The portfolio achieved an annualized return of 8.22% during the period from 2013 to 2022, with an annualized volatility of 3.8%. The Sharpe ratio reached 1.54, and the maximum drawdown was 4.29%.









