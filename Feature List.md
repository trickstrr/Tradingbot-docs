# Documentation: Configuration Parameters for the Crypto Trading Bot Strategy

## Table of Contents
1. [General Settings](#general-settings)
2. [Signal Weights](#signal-weights)
3. [Position Size Parameters](#position-size-parameters)
4. [Signal Thresholds](#signal-thresholds)
5. [RSI Parameters](#rsi-parameters)
6. [Momentum Parameters](#momentum-parameters)
7. [Volume Parameters](#volume-parameters)
8. [Moving Average Parameters](#moving-average-parameters)
9. [Indicator Weights](#indicator-weights)
10. [Confidence Level Parameters](#confidence-level-parameters)
11. [Volatility Parameters](#volatility-parameters)
12. [Trend Parameters](#trend-parameters)
13. [Cooling Period Parameters](#cooling-period-parameters)
14. [Position Management Parameters](#position-management-parameters)
15. [Trailing Stop Parameters](#trailing-stop-parameters)
16. [Profit-Taking Parameters](#profit-taking-parameters)
17. [Profit Protection Parameters](#profit-protection-parameters)
18. [Divergence Parameters](#divergence-parameters)
19. [Market Timing Parameters](#market-timing-parameters)
20. [Time Filter Parameters](#time-filter-parameters)
21. [Loss Limitation Parameters](#loss-limitation-parameters)

---

## General Settings

### UPDATE_INTERVAL
**Description:** Time interval in seconds at which the strategy reanalyzes the market and generates trading signals.  
**Values:** Specified in seconds (e.g., 3600 = 1 hour, 1800 = 30 minutes)  
**Impact:** Smaller values allow faster reactions to market movements but consume more resources and may result in more noise. Higher values reduce noise and API calls but may miss critical trading opportunities.  
**Optimal Use:** Should align with the selected timeframe. For a 1-hour chart, an interval of 30-60 minutes is recommended.

### LOOKBACK_WINDOW
**Description:** Number of historical data points used for analysis.  
**Values:** Integer (e.g., 300 candles)  
**Impact:** Determines how far back the strategy looks to identify patterns and calculate indicators.  
**Optimal Use:** Should be large enough to detect long-term trends but not so large that outdated data skews the analysis.

### PREDICTION_HORIZON
**Description:** Number of future time units for which a forecast is made.  
**Values:** Integer (e.g., 12 periods)  
**Impact:** Influences how far into the future the strategy attempts to predict.  
**Optimal Use:** Shorter horizons are generally more accurate, but longer ones can be helpful for strategic decisions.

## Signal Weights

### SIGNAL_WEIGHTS
**Description:** Weights assigned to various signal sources contributing to the overall signal.  
**Components:**
- **hybrid_weight:** Weight of the hybrid signal combining various technical indicators.
- **technical_weight:** Weight of pure technical analysis.
- **finrl_weight:** Weight of the FinRL model (Reinforcement Learning).
- **sentiment_weight:** Weight of sentiment analysis.
- **lstm_weight:** Weight of the LSTM model for price forecasting.
- **rl_weight:** Weight of the general Reinforcement Learning model.

**Impact:** Determines how strongly each signal source influences the final trading signal.  
**Optimal Use:** Higher weights should be assigned to more reliable signal sources. In volatile markets, the hybrid_weight should be increased.

## Position Size Parameters

### MAX_POSITION_SIZE
**Description:** Maximum proportion of available capital that can be used for a position.  
**Values:** Decimal between 0 and 1 (e.g., 0.7 = 70% of capital)  
**Impact:** Limits risk by capping position size.  
**Optimal Use:** More conservative values (0.5-0.7) for risk-averse strategies, higher values for more aggressive strategies.

### BUY_POSITION_SIZE
**Description:** Proportion of available capital to be used for buy positions.  
**Values:** Decimal between 0 and 1  
**Impact:** Determines how much capital is allocated for buy signals.  
**Optimal Use:** Should be adjusted to market conditions - higher in bull markets, lower in uncertain markets.

### SELL_POSITION_SIZE
**Description:** Proportion of an existing position to be liquidated on sell signals.  
**Values:** Decimal between 0 and 1 (1.0 = sell 100%)  
**Impact:** Determines whether positions are fully or partially closed.  
**Optimal Use:** Typically 1.0 to fully close positions, but can be reduced for partial profit-taking.

## Signal Thresholds

### BUY_THRESHOLD
**Description:** Minimum signal value required to generate a buy signal.  
**Values:** Decimal between 0 and 1 (higher values = more selective signals)  
**Impact:** Determines how strong a signal must be to trigger a buy.  
**Optimal Use:** Higher values (0.4-0.6) for more selective and higher-quality buy signals.

### STRONG_BUY_THRESHOLD
**Description:** Signal value at which a strong buy signal is generated.  
**Values:** Decimal between 0 and 1 (higher than BUY_THRESHOLD)  
**Impact:** Can be used to adjust position size for particularly strong signals.  
**Optimal Use:** Should be significantly higher than BUY_THRESHOLD (e.g., 0.7-0.85).

### SELL_THRESHOLD
**Description:** Maximum signal value below which a sell signal is generated.  
**Values:** Decimal between -1 and 0 (lower values = more selective signals)  
**Impact:** Determines how strong a negative signal must be to trigger a sell.  
**Optimal Use:** Lower values (-0.4 to -0.6) for more selective and higher-quality sell signals.

### STRONG_SELL_THRESHOLD
**Description:** Signal value below which a strong sell signal is generated.  
**Values:** Decimal between -1 and 0 (lower than SELL_THRESHOLD)  
**Impact:** Can be used to adjust position size for particularly strong sell signals.  
**Optimal Use:** Should be significantly lower than SELL_THRESHOLD (e.g., -0.7 to -0.85).

### CONFIDENCE_THRESHOLD
**Description:** Minimum confidence level required for a signal to trigger a trade.  
**Values:** Decimal between 0 and 1  
**Impact:** A higher value means only signals with high confidence result in trades.  
**Optimal Use:** Higher values (0.4-0.65) for a more conservative strategy with fewer but higher-quality trades.

## RSI Parameters

### RSI_OVERSOLD_THRESHOLD
**Description:** RSI value below which an asset is considered strongly oversold.  
**Values:** Integer between 0 and 30 (typically 20-30)  
**Impact:** Lower values result in rarer but stronger buy signals.  
**Optimal Use:** Lower values (18-20) are effective in bear markets, while higher values (25-30) work well in bull markets.

### RSI_MODERATE_OVERSOLD
**Description:** RSI value below which an asset is moderately oversold.  
**Values:** Integer between RSI_OVERSOLD_THRESHOLD and 50 (typically 30-40)  
**Impact:** Generates weaker buy signals compared to strong oversold conditions.  
**Optimal Use:** Can be used for earlier entries with smaller position sizes.

### RSI_OVERBOUGHT_THRESHOLD
**Description:** RSI value above which an asset is considered strongly overbought.  
**Values:** Integer between 70 and 100 (typically 70-80)  
**Impact:** Higher values result in rarer but stronger sell signals.  
**Optimal Use:** Higher values (75-80) are effective in bull markets, while lower values (70-75) work well in bear markets.

### RSI_MODERATE_OVERBOUGHT
**Description:** RSI value above which an asset is moderately overbought.  
**Values:** Integer between 50 and RSI_OVERBOUGHT_THRESHOLD (typically 60-70)  
**Impact:** Generates weaker sell signals compared to strong overbought conditions.  
**Optimal Use:** Can be used for earlier exits with partial position closures.

### RSI_NEUTRAL_LOW
**Description:** Lower bound of the neutral RSI range.  
**Values:** Integer between 30 and 50 (typically 40)  
**Impact:** Affects confidence levels for signals in the neutral range.  
**Optimal Use:** Can be adjusted to define the "neutral zone" where signals are treated with caution.

### RSI_NEUTRAL_HIGH
**Description:** Upper bound of the neutral RSI range.  
**Values:** Integer between 50 and 70 (typically 60)  
**Impact:** Affects confidence levels for signals in the neutral range.  
**Optimal Use:** Can be adjusted to define the "neutral zone" where signals are treated with caution.
