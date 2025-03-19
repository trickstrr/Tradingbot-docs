### **Documentation: Configuration Parameters for the Crypto Trading Bot Strategy**  

## **Table of Contents**  
1. [General Settings](#general-settings)  
2. [Signal Weighting](#signal-weighting)  
3. [Position Sizing Parameters](#position-sizing-parameters)  
4. [Signal Thresholds](#signal-thresholds)  
5. [Volatility Parameters](#volatility-parameters)  
6. [Trend Parameters](#trend-parameters)  
7. [Market Phases Parameters](#market-phases-parameters)  
8. [Position Management Parameters](#position-management-parameters)  
9. [Trailing Stop Parameters](#trailing-stop-parameters)  
10. [Profit Protection Parameters](#profit-protection-parameters)  
11. [Loss Limiting Parameters](#loss-limiting-parameters)  

---

## **6. Trend Parameters**  

### **TREND_SIGNAL_REDUCTION**  
**Description:** Factor by which a trading signal is reduced when it moves against the current market trend.  
**Values:** Decimal value between `0` and `1` (e.g., `0.2` means the signal is reduced by **80%**).  
**Effect:** The lower this value, the stronger the reduction of signals against the trend.  
**Recommended Values:**  
- **Conservative strategy:** `0.1` (90% reduction)  
- **Moderate strategy:** `0.2` (80% reduction)  
- **Aggressive strategy:** `0.4` (60% reduction)  

### **TREND_CONFIDENCE_REDUCTION**  
**Description:** Factor by which the confidence level is reduced when the signal moves against the trend.  
**Values:** Decimal value between `0` and `1` (e.g., `0.5` means confidence is reduced by **50%**).  
**Effect:** Reduces the likelihood of executing trades against the trend.  
**Recommended Values:**  
- **Safe strategy:** `0.3` (70% reduction)  
- **Balanced strategy:** `0.5` (50% reduction)  
- **Risky strategy:** `0.7` (30% reduction)  

---

## **7. Market Phases Parameters**  

### **EXPANSION_TREND_STRENGTH**  
**Description:** Minimum trend strength required to identify an expansion phase.  
**Values:** Decimal value between `0` and `1` (e.g., `0.7` means at least **70% of indicators must show an uptrend**).  
**Effect:** Higher values lead to fewer but more precise expansion phases.  
**Recommended Values:**  
- **Strict detection:** `0.8` (only very strong trends count as expansion)  
- **Standard:** `0.7`  
- **More flexible detection:** `0.6` (more expansion phases, but higher risk of false signals)  

### **EXPANSION_RECENT_RANGE_RATIO**  
**Description:** Minimum price range change relative to the 50-candle range required to confirm an expansion phase.  
**Values:** Decimal value between `0` and `1` (e.g., `0.5` means the **price range must have changed by at least 50%**).  
**Effect:** Higher values ensure more accurate expansion detection, while lower values allow for more frequent expansion trades.  
**Recommended Values:**  
- **Strict detection:** `0.6`  
- **Standard:** `0.5`  
- **More flexible detection:** `0.4`  

### **EXPANSION_VOLATILITY**  
**Description:** Minimum volatility (%) required to recognize an expansion phase.  
**Values:** Decimal value between `0` and `5` (e.g., `1.5` means the standard deviation of price movements must be **at least 1.5%**).  
**Effect:** Higher values reduce false signals, lower values detect more expansion phases.  
**Recommended Values:**  
- **Strict detection:** `2.0`  
- **Standard:** `1.5`  
- **More flexible detection:** `1.2`  

---

## **8. Position Management Parameters**  

### **MAX_UNPROFITABLE_HOLD_HOURS**  
**Description:** Maximum number of hours an unprofitable position is held before being forcibly closed.  
**Values:** Integer (e.g., `3` means an unprofitable position is held for **a maximum of 3 hours**).  
**Recommended Values:**  
- **Short-term trading:** `2-4 hours`  
- **Medium-term trading:** `6-12 hours`  
- **Long-term trading:** `24+ hours`  

---

## **9. Trailing Stop Parameters**  

### **TRAILING_STOP_ACTIVATION**  
**Description:** Minimum profit (%) before a trailing stop is activated.  
**Values:** Decimal value between `0` and `5` (e.g., `2.5` means a trailing stop is activated when the position is **at least 2.5% in profit**).  
**Recommended Values:**  
- **Safe strategy:** `1.5%`  
- **Standard:** `2.5%`  
- **Aggressive strategy:** `3.5%`  

### **TRAILING_STOP_DISTANCE**  
**Description:** Distance of the trailing stop from the current price after activation.  
**Values:** Decimal value between `0` and `5` (e.g., `1.0` means the trailing stop is set **1% below the highest price**).  
**Recommended Values:**  
- **Tight stop:** `0.5%`  
- **Standard:** `1.0%`  
- **Wider stop:** `1.5%`  

---

## **10. Profit Protection Parameters**  

### **PROFIT_PROTECTION_MIN**  
**Description:** Minimum profit (%) before profit protection is activated.  
**Values:** Decimal value between `0` and `5` (e.g., `1.2` means profit protection is activated at **1.2% profit**).  
**Recommended Values:**  
- **Safe strategy:** `1.0%`  
- **Standard:** `1.2%`  
- **Aggressive strategy:** `1.5%`  

### **PROFIT_PROTECTION_FACTOR**  
**Description:** Factor that determines how much profit decline is tolerated before a position is closed.  
**Values:** Decimal value between `0` and `1` (e.g., `0.8` means a position is closed if **20% of the maximum profit is lost**).  
**Recommended Values:**  
- **Conservative:** `0.9` (allows a 10% drawdown of max profit)  
- **Standard:** `0.8` (allows a 20% drawdown of max profit)  
- **Riskier:** `0.7` (allows a 30% drawdown of max profit)  

---

## **11. Loss Limiting Parameters**  

### **MAX_CONSECUTIVE_LOSSES**  
**Description:** Maximum number of consecutive losing trades before trading is paused.  
**Values:** Integer (e.g., `2` means the bot pauses trading after **2 consecutive losses**).  
**Recommended Values:**  
- **Safe strategy:** `1`  
- **Standard:** `2`  
- **Riskier:** `3`  

---

### **Summary of the new parameters**  
- **TREND_SIGNAL_REDUCTION** and **TREND_CONFIDENCE_REDUCTION** control how much signals are weakened when they go against the trend.  
- **EXPANSION_TREND_STRENGTH**, **EXPANSION_RECENT_RANGE_RATIO**, and **EXPANSION_VOLATILITY** define how expansion phases are detected.  
- **MAX_UNPROFITABLE_HOLD_HOURS** prevents holding unprofitable positions for too long.  
- **TRAILING_STOP_ACTIVATION** and **TRAILING_STOP_DISTANCE** help manage trailing stop behavior.  
- **PROFIT_PROTECTION_MIN** and **PROFIT_PROTECTION_FACTOR** ensure profitable trades are secured.  
- **MAX_CONSECUTIVE_LOSSES** prevents excessive losses by pausing trading after repeated losses.  

These parameters allow for better control over trend and market phase analysis, improved adaptability to market conditions, and reduce the need for manual code changes by making everything configurable in `config.json`.
