# Documentation: Configuration Parameters for the Crypto Trading Bot Strategy

## Table of Contents
1. [General Settings](#general-settings)
2. [Signal Weights](#signal-weights)
3. [Position Size Parameters](#position-size-parameters)
4. [Signal Threshold Parameters](#signal-threshold-parameters)
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
16. [Profit Taking Parameters](#profit-taking-parameters)
17. [Profit Protection Parameters](#profit-protection-parameters)
18. [Divergence Parameters](#divergence-parameters)
19. [Market Timing Parameters](#market-timing-parameters)
20. [Time Filter Parameters](#time-filter-parameters)
21. [Loss Limitation Parameters](#loss-limitation-parameters)

---

## General Settings

### UPDATE_INTERVAL
**Description:** Time interval in seconds after which the strategy re-analyzes the market and generates trading signals.  
**Values:** Specified in seconds (e.g., 3600 = 1 hour, 1800 = 30 minutes)  
**Impact:** Smaller values allow faster reactions to market movements, but consume more resources and can lead to more noise. Higher values reduce noise and API calls, but may miss important trading moments.  
**Optimal Use:** Should be aligned with the chosen timeframe. For a 1h chart, an interval of 30-60 minutes is reasonable.

### LOOKBACK_WINDOW
**Description:** Number of historical data points used for analysis.  
**Values:** Integer (e.g., 300 candles)  
**Impact:** Determines how far back the strategy looks to recognize patterns and calculate indicators.  
**Optimal Use:** Should be large enough to recognize long-term trends, but not so large that outdated data influences the analysis.

### PREDICTION_HORIZON
**Description:** Number of future time units for which a forecast is created.  
**Values:** Integer (e.g., 12 periods)  
**Impact:** Influences how far into the future the strategy attempts to forecast.  
**Optimal Use:** Shorter horizons are generally more accurate, but longer ones can be helpful for strategic decisions.

## Signal Weights

### SIGNAL_WEIGHTS
**Description:** Weights for various signal sources that contribute to the overall signal.  
**Components:**
- **hybrid_weight:** Weight of the custom hybrid signal that combines various technical indicators.
- **technical_weight:** Weight of pure technical analysis.
- **finrl_weight:** Weight of the FinRL model (Reinforcement Learning).
- **sentiment_weight:** Weight of sentiment analysis.
- **lstm_weight:** Weight of the LSTM model for price predictions.
- **rl_weight:** Weight of the general Reinforcement Learning model.

**Impact:** Determines how strongly each signal source influences the final trading signal.  
**Optimal Use:** Higher weights should be given to more reliable signal sources. In volatile markets, the hybrid_weight should be increased.

## Position Size Parameters

### MAX_POSITION_SIZE
**Description:** Maximum proportion of available capital that can be used for a position.  
**Values:** Decimal between 0 and 1 (e.g., 0.7 = 70% of capital)  
**Impact:** Limits risk by limiting position size.  
**Optimal Use:** More conservative values (0.5-0.7) for risk-conscious strategies, higher values for more aggressive strategies.

### BUY_POSITION_SIZE
**Description:** Proportion of available capital used for buy positions.  
**Values:** Decimal between 0 and 1  
**Impact:** Determines how much capital is deployed on buy signals.  
**Optimal Use:** Should be adjusted to market conditions - higher in bull markets, lower in uncertain markets.

### SELL_POSITION_SIZE
**Description:** Proportion of the existing position that is liquidated on sell signals.  
**Values:** Decimal between 0 and 1 (1.0 = sell 100%)  
**Impact:** Determines whether positions are fully or partially closed.  
**Optimal Use:** Typically 1.0 to fully close positions, but can be reduced for partial profit-taking.

## Signal Threshold Parameters

### BUY_THRESHOLD
**Description:** Minimum signal value at which a buy signal is generated.  
**Values:** Decimal between 0 and 1 (higher values = more selective signals)  
**Impact:** Determines how strong a signal must be to trigger a purchase.  
**Optimal Use:** Higher values (0.4-0.6) for more selective and higher quality buy signals.

### STRONG_BUY_THRESHOLD
**Description:** Signal value at which a strong buy signal is generated.  
**Values:** Decimal between 0 and 1 (higher than BUY_THRESHOLD)  
**Impact:** Can be used to adjust position size for particularly strong signals.  
**Optimal Use:** Should be significantly higher than BUY_THRESHOLD (e.g., 0.7-0.85).

### SELL_THRESHOLD
**Description:** Maximum signal value below which a sell signal is generated.  
**Values:** Decimal between -1 and 0 (lower values = more selective signals)  
**Impact:** Determines how strong a negative signal must be to trigger a sale.  
**Optimal Use:** Lower values (-0.4 to -0.6) for more selective and higher quality sell signals.

### STRONG_SELL_THRESHOLD
**Description:** Signal value below which a strong sell signal is generated.  
**Values:** Decimal between -1 and 0 (lower than SELL_THRESHOLD)  
**Impact:** Can be used to adjust position size for particularly strong sell signals.  
**Optimal Use:** Should be significantly lower than SELL_THRESHOLD (e.g., -0.7 to -0.85).

### CONFIDENCE_THRESHOLD
**Description:** Minimum confidence level that a signal must have to trade.  
**Values:** Decimal between 0 and 1  
**Impact:** A higher value means that only signals with high confidence lead to trades.  
**Optimal Use:** Higher values (0.4-0.65) for a more conservative strategy with fewer but higher quality trades.

## RSI Parameters

### RSI_OVERSOLD_THRESHOLD
**Description:** RSI value below which an asset is considered strongly oversold.  
**Values:** Integer between 0 and 30 (typically 20-30)  
**Impact:** Lower values lead to less frequent but stronger buy signals.  
**Optimal Use:** In bear markets, lower values (18-20) should be used; in bull markets, higher values (25-30) can be effective.

### RSI_MODERATE_OVERSOLD
**Description:** RSI value below which an asset is considered moderately oversold.  
**Values:** Integer between RSI_OVERSOLD_THRESHOLD and 50 (typically 30-40)  
**Impact:** Generates weaker buy signals than strong oversold conditions.  
**Optimal Use:** Can be used for earlier entries with smaller position sizes.

### RSI_OVERBOUGHT_THRESHOLD
**Description:** RSI value above which an asset is considered strongly overbought.  
**Values:** Integer between 70 and 100 (typically 70-80)  
**Impact:** Higher values lead to less frequent but stronger sell signals.  
**Optimal Use:** In bull markets, higher values (75-80) should be used; in bear markets, lower values (70-75) can be effective.

### RSI_MODERATE_OVERBOUGHT
**Description:** RSI value above which an asset is considered moderately overbought.  
**Values:** Integer between 50 and RSI_OVERBOUGHT_THRESHOLD (typically 60-70)  
**Impact:** Generates weaker sell signals than strong overbought conditions.  
**Optimal Use:** Can be used for earlier exits with partial position closures.

### RSI_NEUTRAL_LOW
**Description:** Lower bound of the neutral RSI range.  
**Values:** Integer between 30 and 50 (typically 40)  
**Impact:** Influences the confidence level for signals in the neutral range.  
**Optimal Use:** Can be adjusted to define the "neutral zone" where signals should be viewed with caution.

### RSI_NEUTRAL_HIGH
**Description:** Upper bound of the neutral RSI range.  
**Values:** Integer between 50 and 70 (typically 60)  
**Impact:** Influences the confidence level for signals in the neutral range.  
**Optimal Use:** Can be adjusted to define the "neutral zone" where signals should be viewed with caution.

## Momentum Parameters

### MOMENTUM_12H_THRESHOLD
**Description:** Threshold for price movement over 12 hours that is considered significant.  
**Values:** Decimal (e.g., 0.045 = 4.5% price change)  
**Impact:** Lower values generate more, higher values more selective momentum signals.  
**Optimal Use:** Should be adjusted to the typical volatility of the asset - higher for more volatile assets.

### MOMENTUM_24H_THRESHOLD
**Description:** Threshold for price movement over 24 hours that is considered significant.  
**Values:** Decimal (typically higher than MOMENTUM_12H_THRESHOLD)  
**Impact:** Lower values generate more, higher values more selective momentum signals.  
**Optimal Use:** Should be adjusted to the typical volatility of the asset - higher for more volatile assets.

### MOMENTUM_12H_MULTIPLIER
**Description:** Factor by which 12-hour price changes are multiplied to calculate signal strength.  
**Values:** Integer (typically 4-6)  
**Impact:** Higher values amplify the influence of short-term momentum on the overall signal.  
**Optimal Use:** Higher values for short-term strategies, lower for longer-term approaches.

### MOMENTUM_24H_MULTIPLIER
**Description:** Factor by which 24-hour price changes are multiplied to calculate signal strength.  
**Values:** Integer (typically 3-5)  
**Impact:** Higher values amplify the influence of medium-term momentum on the overall signal.  
**Optimal Use:** Should be somewhat lower than the 12H multiplier, as longer-term movements are already larger.

### MOMENTUM_12H_WEIGHT
**Description:** Weight of 12-hour momentum in the combined momentum signal.  
**Values:** Decimal between 0 and 1 (sum with MOMENTUM_24H_WEIGHT should equal 1)  
**Impact:** Higher values emphasize short-term price movements more strongly.  
**Optimal Use:** Higher values (0.6-0.7) for more responsive strategies, lower for more stable signals.

### MOMENTUM_24H_WEIGHT
**Description:** Weight of 24-hour momentum in the combined momentum signal.  
**Values:** Decimal between 0 and 1 (sum with MOMENTUM_12H_WEIGHT should equal 1)  
**Impact:** Higher values emphasize medium-term price movements more strongly.  
**Optimal Use:** Higher values (0.4-0.5) for more stable signals, lower for more responsive strategies.

## Volume Parameters

### VOLUME_RATIO_THRESHOLD
**Description:** Ratio between current volume and average volume that is considered significant.  
**Values:** Decimal (typically 1.5-2.5, where 1.0 is the average volume)  
**Impact:** Lower values lead to more volume-based signals, higher to more selective ones.  
**Optimal Use:** Should be adjusted based on the typical volume volatility of the asset.

## Moving Average Parameters

### MA_PRICE_UPPER_THRESHOLD
**Description:** Ratio between current price and 50-period average that is considered significantly overbought.  
**Values:** Decimal greater than 1 (e.g., 1.06 = 6% above MA50)  
**Impact:** Lower values generate more frequent sell signals based on price distance to MAs.  
**Optimal Use:** In less volatile markets, lower values should be used; in more volatile ones, higher values.

### MA_PRICE_LOWER_THRESHOLD
**Description:** Ratio between current price and 50-period average that is considered significantly oversold.  
**Values:** Decimal less than 1 (e.g., 0.94 = 6% below MA50)  
**Impact:** Higher values generate more frequent buy signals based on price distance to MAs.  
**Optimal Use:** In less volatile markets, higher values should be used; in more volatile ones, lower values.

### MA_TREND_UPPER_THRESHOLD
**Description:** Ratio between shorter and longer moving averages that indicates a strong uptrend.  
**Values:** Decimal greater than 1 (e.g., 1.02 = short-term MA is 2% above longer-term MA)  
**Impact:** Lower values detect uptrends earlier, higher values only at stronger trends.  
**Optimal Use:** Should be adjusted based on the typical trend strength of the asset.

### MA_TREND_LOWER_THRESHOLD
**Description:** Ratio between shorter and longer moving averages that indicates a strong downtrend.  
**Values:** Decimal less than 1 (e.g., 0.98 = short-term MA is 2% below longer-term MA)  
**Impact:** Higher values detect downtrends earlier, lower values only at stronger trends.  
**Optimal Use:** Should be adjusted based on the typical trend strength of the asset.

## Indicator Weights

### RSI_WEIGHT
**Description:** Weight of the RSI signal in the combined overall signal.  
**Values:** Decimal between 0 and 1 (sum of all weights should equal 1)  
**Impact:** Higher values increase the influence of RSI on the overall signal.  
**Optimal Use:** In sideways markets, this value should be higher, as RSI works better there.

### MOMENTUM_WEIGHT
**Description:** Weight of the momentum signal in the combined overall signal.  
**Values:** Decimal between 0 and 1  
**Impact:** Higher values increase the influence of price movements on the overall signal.  
**Optimal Use:** In trending markets, this value should be higher, as momentum works better there.

### VOLUME_WEIGHT
**Description:** Weight of the volume signal in the combined overall signal.  
**Values:** Decimal between 0 and 1  
**Impact:** Higher values increase the influence of volume analyses on the overall signal.  
**Optimal Use:** Should be higher for assets with strong volume-price correlations.

### MA_WEIGHT
**Description:** Weight of the moving average signal in the combined overall signal.  
**Values:** Decimal between 0 and 1  
**Impact:** Higher values increase the influence of trend analyses on the overall signal.  
**Optimal Use:** In strongly trending markets, this value should be higher.

## Confidence Level Parameters

### MIN_CONFIDENCE
**Description:** Minimum confidence level assigned to a signal when no strong signals are present.  
**Values:** Decimal between 0 and 1 (typically 0.1-0.2)  
**Impact:** Higher values cause even weaker signals to receive a certain minimum confidence.  
**Optimal Use:** Should be low to prevent weak signals from leading to trades.

### BASE_CONFIDENCE
**Description:** Base value for confidence level before adjustments based on signal agreement are made.  
**Values:** Decimal between 0 and 1 (typically 0.2-0.3)  
**Impact:** Higher values generally lead to more trades, as the base confidence is higher.  
**Optimal Use:** Should be higher in markets with clear trends, lower in uncertain markets.

### SIGNAL_AGREEMENT_WEIGHT
**Description:** Weighting factor for adjusting confidence based on agreement between different signals.  
**Values:** Decimal between 0 and 1 (typically 0.6-0.8)  
**Impact:** Higher values increase the influence of signal agreement on the confidence level.  
**Optimal Use:** Higher values lead to more confidence in signals where multiple indicators agree.

### SIGNAL_STRENGTH_WEIGHT
**Description:** Weighting factor for adjusting confidence based on absolute signal strength.  
**Values:** Decimal between 0 and 1 (typically 0.3-0.6)  
**Impact:** Higher values increase the influence of signal strength on the confidence level.  
**Optimal Use:** Higher values lead to more confidence in very strong signals, even if not all indicators agree.

### NEUTRAL_CONFIDENCE_FACTOR
**Description:** Factor by which the confidence level is multiplied when RSI is in the neutral range.  
**Values:** Decimal between 0 and 1 (typically 0.6-0.8)  
**Impact:** Lower values reduce confidence more strongly when RSI is in the neutral range.  
**Optimal Use:** Should be lower in volatile markets, as neutral RSI values are less meaningful there.

## Volatility Parameters

### VOLATILITY_THRESHOLD
**Description:** Standard deviation of price changes above which a market is considered volatile.  
**Values:** Decimal (e.g., 1.8 = 1.8% average deviation from the mean price change)  
**Impact:** Lower values cause more market conditions to be classified as volatile.  
**Optimal Use:** Should be adjusted to the typical volatility of the traded asset.

### VOLATILITY_POSITION_MODIFIER
**Description:** Factor by which the position size is multiplied when the market is considered volatile.  
**Values:** Decimal between 0 and 1 (e.g., 0.5 = halving the position size)  
**Impact:** Lower values lead to stronger position size reductions in volatile markets.  
**Optimal Use:** Should be adjusted based on risk tolerance - more conservative strategies use lower values.

### EXTREME_VOLATILITY_THRESHOLD
**Description:** Standard deviation of price changes above which a market is considered extremely volatile.  
**Values:** Decimal (higher than VOLATILITY_THRESHOLD)  
**Impact:** Lower values cause more situations to be classified as extremely volatile.  
**Optimal Use:** Should be significantly higher than the normal volatility threshold.

### MIN_PROFIT_FOR_VOL_EXIT
**Description:** Minimum profit in percent before a position is closed due to extreme volatility.  
**Values:** Decimal (e.g., 0.2 = 0.2% profit)  
**Impact:** Lower values lead to earlier exits during volatility.  
**Optimal Use:** Should be adjusted based on the typical volatility of the asset.

## Trend Parameters

### UPTREND_THRESHOLD
**Description:** Ratio between short-term (MA5) and medium-term (MA20) average that indicates an uptrend.  
**Values:** Decimal greater than 1 (e.g., 1.01 = MA5 is 1% above MA20)  
**Impact:** Lower values identify uptrends earlier, but can generate more false signals.  
**Optimal Use:** In strongly trending markets, higher values should be used to ignore temporary pullbacks.

### DOWNTREND_THRESHOLD
**Description:** Ratio between short-term (MA5) and medium-term (MA20) average that indicates a downtrend.  
**Values:** Decimal less than 1 (e.g., 0.99 = MA5 is 1% below MA20)  
**Impact:** Higher values identify downtrends earlier, but can generate more false signals.  
**Optimal Use:** In strongly trending markets, lower values should be used to ignore temporary bounces.

### TREND_SIGNAL_REDUCTION
**Description:** Factor by which a signal is multiplied when it runs counter to the current market trend.  
**Values:** Decimal between 0 and 1 (e.g., 0.5 = signal is reduced by 50%)  
**Impact:** Lower values weaken signals more strongly that run counter to the prevailing trend.  
**Optimal Use:** In strongly trending markets, lower values should be used to suppress counter-trend signals more strongly.

### TREND_CONFIDENCE_REDUCTION
**Description:** Factor by which a signal's confidence level is multiplied when it runs counter to the current market trend.  
**Values:** Decimal between 0 and 1 (e.g., 0.7 = confidence is reduced by 30%)  
**Impact:** Lower values reduce confidence in signals more strongly that run counter to the prevailing trend.  
**Optimal Use:** In strongly trending markets, lower values should be used to reduce confidence in counter-trend signals.

### EXPANSION_TREND_STRENGTH
**Description:** Minimum trend strength required to identify a market phase as expansion.  
**Values:** Decimal between 0 and 1 (typically 0.6-0.8)  
**Impact:** Higher values require stronger uptrends to recognize the expansion phase.  
**Optimal Use:** In volatile markets, higher values should be used to avoid misinterpretations.

### EXPANSION_RECENT_RANGE_RATIO
**Description:** Minimum ratio of the price range of the last 20 periods to the total price range of the last 50 periods to identify an expansion phase.  
**Values:** Decimal between 0 and 1 (typically 0.5-0.7)  
**Impact:** Higher values require a stronger recent price movement relative to the medium-term movement.  
**Optimal Use:** In markets with slow trends, lower values should be used.

### EXPANSION_VOLATILITY
**Description:** Minimum volatility (standard deviation of price changes) required to identify a market phase as expansion.  
**Values:** Decimal (e.g., 1.5 = 1.5% standard deviation)  
**Impact:** Higher values require more price volatility to recognize the expansion phase.  
**Optimal Use:** Should be adjusted to the typical volatility of the traded asset.

## Cooling Period Parameters

### COOLING_PERIOD_HOURS
**Description:** Time in hours to wait after a loss before opening new positions.  
**Values:** Integer (e.g., 8 = 8 hours waiting time)  
**Impact:** Longer periods reduce trading frequency after losses.  
**Optimal Use:** Should be longer for more conservative strategies and in volatile markets.

## Position Management Parameters

### MAX_UNPROFITABLE_HOLD_HOURS
**Description:** Maximum time in hours an unprofitable position is held before being closed.  
**Values:** Integer (e.g., 12 = 12 hours maximum holding time for unprofitable positions)  
**Impact:** Lower values lead to faster exits from unprofitable trades.  
**Optimal Use:** Should be shorter in volatile markets and for short-term strategies.

### UNPROFITABLE_LONG_THRESHOLD
**Description:** Price threshold in relation to entry price below which a long position is considered unprofitable.  
**Values:** Decimal greater than 1 (e.g., 1.002 = price must be 0.2% above entry price)  
**Impact:** Values closer to 1 place stricter requirements on profitability.  
**Optimal Use:** Stricter values (closer to 1) for more aggressive profit-taking, more generous for patient strategies.

### UNPROFITABLE_SHORT_THRESHOLD
**Description:** Price threshold in relation to entry price above which a short position is considered unprofitable.  
**Values:** Decimal less than 1 (e.g., 0.998 = price must be 0.2% below entry price)  
**Impact:** Values closer to 1 place stricter requirements on profitability.  
**Optimal Use:** Stricter values (closer to 1) for more aggressive profit-taking, more generous for patient strategies.

## Trailing Stop Parameters

### TRAILING_STOP_ACTIVATION
**Description:** Minimum profit in percent before the trailing stop is activated.  
**Values:** Decimal (e.g., 0.008 = 0.8% profit)  
**Impact:** Lower values activate trailing stops earlier.  
**Optimal Use:** In volatile markets, this value should be lower to secure profits more quickly.

### TRAILING_STOP_DISTANCE
**Description:** Distance between current price and trailing stop price.  
**Values:** Decimal (e.g., 0.005 = 0.5% distance)  
**Impact:** Lower values lead to tighter stops that can be triggered earlier.  
**Optimal Use:** Should be adjusted to the typical volatility of the asset - tighter for less volatile assets.

### TRAILING_ADJUST_8H, TRAILING_ADJUST_24H, TRAILING_ADJUST_48H
**Description:** Factors by which the trailing stop distance is multiplied after certain holding times.  
**Values:** Decimal between 0 and 1 (e.g., 0.9 = 10% reduction in distance)  
**Impact:** Lower values lead to tighter stops after longer holding times.  
**Optimal Use:** Should be adjusted based on the expected holding time of the strategy.

### TRAILING_MIN_8H, TRAILING_MIN_24H, TRAILING_MIN_48H
**Description:** Minimum trailing stop distance after certain holding times.  
**Values:** Decimal (e.g., 0.006 = 0.6% minimum distance)  
**Impact:** Lower values allow tighter stops after longer holding times.  
**Optimal Use:** Should be adjusted to the typical volatility of the asset.

## Profit Taking Parameters

### QUICK_PROFIT_6H, QUICK_PROFIT_12H
**Description:** Profit thresholds in percent for quick profit-taking within certain time windows.  
**Values:** Decimal (e.g., 1.5 = 1.5% profit)  
**Impact:** Lower values lead to earlier profit-taking.  
**Optimal Use:** In volatile markets, these values should be lower to secure profits more quickly.

## Profit Protection Parameters

### PROFIT_PROTECTION_MIN
**Description:** Minimum profit in percent before profit protection is activated.  
**Values:** Decimal (e.g., 1.5 = 1.5% profit)  
**Impact:** Lower values activate profit protection earlier.  
**Optimal Use:** In volatile markets, this value should be lower to protect profits more quickly.

### PROFIT_PROTECTION_FACTOR
**Description:** Factor that determines how much profit decline is tolerated before a position is closed.  
**Values:** Decimal between 0 and 1 (e.g., 0.8 = position is closed when 20% of maximum profit is lost)  
**Impact:** Higher values lead to faster exits when profits shrink.  
**Optimal Use:** In volatile markets, this value should be higher to protect profits more aggressively.

## Divergence Parameters

### DIVERGENCE_WEIGHT
**Description:** Weight of the divergence signal (difference between price and RSI trend) in the overall signal.  
**Values:** Decimal between 0 and 1  
**Impact:** Higher values increase the influence of divergences on the overall signal.  
**Optimal Use:** Should be higher for strategies that aim for trend reversals.

## Market Timing Parameters

### LOCAL_MIN_CONFIDENCE_BOOST, LOCAL_MAX_CONFIDENCE_BOOST
**Description:** Factors by which the confidence level is multiplied when the current price represents a local minimum/maximum.  
**Values:** Decimal greater than 1 (e.g., 1.25 = 25% increase in confidence)  
**Impact:** Higher values increase the influence of market timing on the confidence level.  
**Optimal Use:** Higher values for strategies that rely on precise timing.

### NON_EXTREME_CONFIDENCE_PENALTY
**Description:** Factor by which the confidence level is multiplied when the current price does not represent a local extreme.  
**Values:** Decimal between 0 and 1 (e.g., 0.8 = 20% reduction in confidence)  
**Impact:** Lower values reduce confidence more strongly when timing is not optimal.  
**Optimal Use:** Lower values for strategies that rely on precise timing.

## Time Filter Parameters

### AVOID_TRADING_HOURS
**Description:** List of hours (UTC) during which no trading should take place.  
**Values:** List of integers between 0 and 23  
**Impact:** Prevents trading during certain hours.  
**Optimal Use:** Can be used to avoid times with low liquidity or historically poor performance.

## Loss Limitation Parameters

### MAX_CONSECUTIVE_LOSSES
**Description:** Maximum number of consecutive losses before trading is paused.  
**Values:** Integer (e.g., 2 = pause after 2 consecutive losses)  
**Impact:** Lower values lead to more frequent trading pauses after losses.  
**Optimal Use:** Should be adjusted based on the expected win rate of the strategy.

### CONSECUTIVE_LOSSES_COOLDOWN
**Description:** Time in hours to wait after reaching the maximum number of consecutive losses before trading resumes.  
**Values:** Integer (e.g., 24 = 24 hours waiting time)  
**Impact:** Longer periods reduce trading frequency after consecutive losses.  
**Optimal Use:** Should be longer for more conservative strategies and in volatile markets.

---
