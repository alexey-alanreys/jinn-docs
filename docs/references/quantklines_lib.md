# QuantKlines Library Reference

The **QuantKlines** library provides a comprehensive suite of high-performance technical analysis functions.

> **Note:** All code examples assume `import src.core.quantklines as qk`.

## Table of Contents

- [Filters](#filters)
- [Math Operations](#math-operations)
- [Momentum Indicators](#momentum-indicators)
- [Timeframe Utilities](#timeframe-utilities)
- [Trend Indicators](#trend-indicators)
- [Utility Functions](#utility-functions)
- [Volatility Indicators](#volatility-indicators)

---

## <a id="filters"></a> Filters

Functions for detecting crossover and crossunder events between data series.

### cross()

```python
cross(source1: np.ndarray, source2: np.ndarray) -> np.ndarray
```

Detects crossover points between two data series (both above and below crossings).

**Parameters:**

- `source1` (np.ndarray): First input data series
- `source2` (np.ndarray): Second input data series

**Returns:**

- `np.ndarray[bool]`: Boolean array with True values at crossover points

**Example:**

```python
# Detect when price crosses moving average (both directions)
ma20 = qk.sma(close, 20)
crosses = qk.cross(close, ma20)
```

### crossover()

```python
crossover(source1: np.ndarray, source2: np.ndarray) -> np.ndarray
```

Detects upward crossover points where `source1` crosses above `source2`.

**Parameters:**

- `source1` (np.ndarray): First input data series
- `source2` (np.ndarray): Second input data series

**Returns:**

- `np.ndarray[bool]`: Boolean array with True at upward crossover points

**Example:**

```python
# Detect when RSI crosses above 30 (oversold exit)
rsi_values = qk.rsi(close, 14)
oversold_exit = qk.crossover(rsi_values, np.full(len(rsi_values), 30))
```

### crossunder()

```python
crossunder(source1: np.ndarray, source2: np.ndarray) -> np.ndarray
```

Detects downward crossover points where `source1` crosses below `source2`.

**Parameters:**

- `source1` (np.ndarray): First input data series
- `source2` (np.ndarray): Second input data series

**Returns:**

- `np.ndarray[bool]`: Boolean array with True at downward crossover points

**Example:**

```python
# Detect when RSI crosses below 70 (overbought exit)
rsi_values = qk.rsi(close, 14)
overbought_exit = qk.crossunder(rsi_values, np.full(len(rsi_values), 70))
```

---

## <a id="math-operations"></a> Math Operations

Core mathematical functions for data manipulation and moving averages.

### change()

```python
change(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate the difference between current values and past values in a data series.

**Parameters:**

- `source` (np.ndarray): Input data series
- `length` (np.int16): Number of periods to look back

**Returns:**

- `np.ndarray`: Array with computed differences (first `length` elements are NaN)

**Example:**

```python
# Calculate 5-period price change
price_change = qk.change(close, 5)
```

### cum()

```python
cum(source: np.ndarray) -> np.ndarray
```

Calculate the cumulative sum of elements in the input array.

**Parameters:**

- `source` (np.ndarray): Input data series

**Returns:**

- `np.ndarray`: Array containing cumulative sum of input elements

**Example:**

```python
# Calculate cumulative volume
cumulative_volume = qk.cum(volume)
```

### ema()

```python
ema(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate the Exponential Moving Average (EMA) of a data series.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): EMA period length

**Returns:**

- `np.ndarray`: EMA values array

**Example:**

```python
# Calculate 21-period EMA
ema21 = qk.ema(close, 21)
```

### hma()

```python
hma(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate HMA (Hull Moving Average) for reduced lag and improved smoothing.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): HMA period length

**Returns:**

- `np.ndarray`: HMA values array

**Example:**

```python
# Calculate 14-period Hull Moving Average
hma14 = qk.hma(close, 14)
```

### rma()

```python
rma(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate Wilder's RMA (Rolling Moving Average) used in RSI calculations.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): RMA period length

**Returns:**

- `np.ndarray`: RMA values array

**Note:** Uses `alpha = 1 / length`. First value is SMA of first `length` periods.

**Example:**

```python
# Calculate 14-period RMA (used internally in RSI)
rma14 = qk.rma(close, 14)
```

### sma()

```python
sma(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate SMA (Simple Moving Average) of a data series.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): SMA period length

**Returns:**

- `np.ndarray`: SMA values array

**Example:**

```python
# Calculate 50-period simple moving average
sma50 = qk.sma(close, 50)
```

### stdev()

```python
stdev(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate rolling standard deviation over a specified window length.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): Window size for standard deviation calculation

**Returns:**

- `np.ndarray`: Array of standard deviation values

**Example:**

```python
# Calculate 20-period standard deviation
price_volatility = qk.stdev(close, 20)
```

### vwap()

```python
vwap(time: np.ndarray, high: np.ndarray, low: np.ndarray,
     close: np.ndarray, volume: np.ndarray) -> np.ndarray
```

Calculate VWAP (Volume-Weighted Average Price) on a daily basis.

**Parameters:**

- `time` (np.ndarray): Timestamps in milliseconds
- `high` (np.ndarray): High prices for each period
- `low` (np.ndarray): Low prices for each period
- `close` (np.ndarray): Close prices for each period
- `volume` (np.ndarray): Trading volume for each period

**Returns:**

- `np.ndarray`: VWAP values array

**Note:** VWAP resets at each new trading day.

**Example:**

```python
# Calculate daily VWAP
daily_vwap = qk.vwap(timestamps, high, low, close, volume)
```

### wma()

```python
wma(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate WMA (Weighted Moving Average) with linear weights.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): WMA period length

**Returns:**

- `np.ndarray`: WMA values array

**Example:**

```python
# Calculate 10-period weighted moving average
wma10 = qk.wma(close, 10)
```

---

## <a id="momentum-indicators"></a> Momentum Indicators

Functions for measuring price momentum and market strength.

### rsi()

```python
rsi(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate RSI (Relative Strength Index) momentum oscillator.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): RSI period length

**Returns:**

- `np.ndarray`: RSI values array (0-100 scale)

**Example:**

```python
# Calculate 14-period RSI
rsi14 = qk.rsi(close, 14)

# Identify overbought/oversold conditions
overbought = rsi14 > 70
oversold = rsi14 < 30
```

### stoch()

```python
stoch(source: np.ndarray, high: np.ndarray, low: np.ndarray,
      length: np.int16) -> np.ndarray
```

Calculate stochastic oscillator values.

**Parameters:**

- `source` (np.ndarray): Closing price series
- `high` (np.ndarray): High price series
- `low` (np.ndarray): Low price series
- `length` (np.int16): Lookback period for high/low calculation

**Returns:**

- `np.ndarray`: Stochastic oscillator values (0-100 scale)

**Example:**

```python
# Calculate 14-period stochastic oscillator
stoch14 = qk.stoch(close, high, low, 14)
```

### wpr()

```python
wpr(close: np.ndarray, high: np.ndarray, low: np.ndarray,
    length: np.int16) -> np.ndarray
```

Calculate Williams Percent Range (WPR) indicator values.

**Parameters:**

- `close` (np.ndarray): Closing price series
- `high` (np.ndarray): High price series
- `low` (np.ndarray): Low price series
- `length` (np.int16): Lookback period for high/low calculation

**Returns:**

- `np.ndarray`: WPR values (-100 to 0 scale)

**Example:**

```python
# Calculate 14-period Williams %R
wpr14 = qk.wpr(close, high, low, 14)

# Identify oversold conditions (typically < -80)
oversold = wpr14 < -80
```

---

## <a id="timeframe-utilities"></a> Timeframe Utilities

Functions for adapting data between different timeframes.

### shrink()

```python
shrink(lower_tf_source: np.ndarray, lower_tf_time: np.ndarray,
       target_tf_time: np.ndarray) -> np.ndarray
```

Adapt lower timeframe data to current timeframe by aligning values.

**Parameters:**

- `lower_tf_source` (np.ndarray): Data values from lower timeframe
- `lower_tf_time` (np.ndarray): Timestamps of lower (source) timeframe
- `target_tf_time` (np.ndarray): Timestamps of main (target) timeframe

**Returns:**

- `np.ndarray`: 2D array with lower TF values aligned to main TF

**Use Case:** Map 1-minute data to 5-minute bars for intrabar analysis.

**Example:**

```python
# Map 1m RSI values to 5m timeframe
rsi_1m = qk.rsi(close_1m, 14)
rsi_mapped = qk.shrink(rsi_1m, time_1m, time_5m)
```

### stretch()

```python
stretch(higher_tf_source: np.ndarray, higher_tf_time: np.ndarray,
        target_tf_time: np.ndarray) -> np.ndarray
```

Adapt higher timeframe data to current timeframe by expanding values.

**Parameters:**

- `higher_tf_source` (np.ndarray): Data values from higher timeframe
- `higher_tf_time` (np.ndarray): Timestamps of higher (source) timeframe
- `target_tf_time` (np.ndarray): Timestamps of target (main) timeframe

**Returns:**

- `np.ndarray`: Array with higher TF values expanded to main TF

**Use Case:** Use daily levels on hourly charts.

**Example:**

```python
# Stretch daily VWAP to hourly timeframe
daily_vwap = qk.vwap(time_1d, high_1d, low_1d, close_1d, volume_1d)
hourly_vwap = qk.stretch(daily_vwap, time_1d, time_1h)
```

---

## <a id="trend-indicators"></a> Trend Indicators

Functions for identifying and measuring market trends.

### dmi()

```python
dmi(high: np.ndarray, low: np.ndarray, close: np.ndarray,
    di_length: np.int16, adx_length: np.int16) -> tuple[np.ndarray, np.ndarray, np.ndarray]
```

Calculate Directional Movement Index (DMI) indicators.

**Parameters:**

- `high` (np.ndarray): Array of high prices
- `low` (np.ndarray): Array of low prices
- `close` (np.ndarray): Array of closing prices
- `di_length` (np.int16): Period for DI calculations
- `adx_length` (np.int16): Smoothing period for ADX calculation

**Returns:**

- `tuple`: Three arrays containing (+DI, -DI, ADX)

**Example:**

```python
# Calculate 14-period DMI with 14-period ADX smoothing
plus_di, minus_di, adx = qk.dmi(high, low, close, 14, 14)

# Identify trend strength
strong_trend = adx > 25
bullish_trend = (plus_di > minus_di) & strong_trend
```

### donchian()

```python
donchian(high: np.ndarray, low: np.ndarray,
         length: np.int16) -> tuple[np.ndarray, np.ndarray, np.ndarray]
```

Calculate Donchian Channel indicators (upper, lower, middle bands).

**Parameters:**

- `high` (np.ndarray): Array of high prices
- `low` (np.ndarray): Array of low prices
- `length` (np.int16): Lookback period for calculations

**Returns:**

- `tuple`: Three arrays containing (upper_band, lower_band, middle_band)

**Example:**

```python
# Calculate 20-period Donchian Channel
upper, lower, middle = qk.donchian(high, low, 20)

# Identify breakouts
breakout_up = close > upper
breakout_down = close < lower
```

### dst()

```python
dst(high: np.ndarray, low: np.ndarray, close: np.ndarray,
    factor: np.float32, atr_length: np.int16) -> tuple[np.ndarray, np.ndarray]
```

Calculate Double SuperTrend (DST) indicator bands.

**Parameters:**

- `high` (np.ndarray): High price series
- `low` (np.ndarray): Low price series
- `close` (np.ndarray): Close price series
- `factor` (np.float32): Multiplier for ATR band width
- `atr_length` (np.int16): Period length for ATR calculation

**Returns:**

- `tuple`: Two arrays containing (upper_band, lower_band)

**Note:** Unlike standard SuperTrend, maintains both bands simultaneously.

**Example:**

```python
# Calculate DST with 3.0 factor and 10-period ATR
upper_band, lower_band = qk.dst(high, low, close, 3.0, 10)
```

### supertrend()

```python
supertrend(high: np.ndarray, low: np.ndarray, close: np.ndarray,
           factor: np.float32, atr_length: np.int16) -> tuple[np.ndarray, np.ndarray]
```

Calculate SuperTrend indicator values and direction.

**Parameters:**

- `high` (np.ndarray): High price series
- `low` (np.ndarray): Low price series
- `close` (np.ndarray): Close price series
- `factor` (np.float32): Multiplier for ATR band width
- `atr_length` (np.int16): Period length for ATR calculation

**Returns:**

- `tuple`: Two arrays containing (indicator_values, direction)
  - `direction`: -1 for uptrend, 1 for downtrend

**Example:**

```python
# Calculate SuperTrend with 3.0 factor and 10-period ATR
st_values, st_direction = qk.supertrend(high, low, close, 3.0, 10)

# Identify trend changes
trend_up = st_direction == -1
trend_down = st_direction == 1
```

---

## <a id="utility-functions"></a> Utility Functions

Helper functions for data analysis and pivot point detection.

### highest()

```python
highest(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate the highest value over a sliding window.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): Lookback window length

**Returns:**

- `np.ndarray`: Array of highest values

**Example:**

```python
# Find highest high over last 20 periods
highest_high = qk.highest(high, 20)
```

### lowest()

```python
lowest(source: np.ndarray, length: np.int16) -> np.ndarray
```

Calculate the lowest value over a sliding window.

**Parameters:**

- `source` (np.ndarray): Input series (leading NaNs are skipped)
- `length` (np.int16): Lookback window length

**Returns:**

- `np.ndarray`: Array of lowest values

**Example:**

```python
# Find lowest low over last 20 periods
lowest_low = qk.lowest(low, 20)
```

### pivothigh()

```python
pivothigh(source: np.ndarray, leftbars: np.int16,
          rightbars: np.int16) -> np.ndarray
```

Identify pivot highs in a series over specified left and right lookback windows.

**Parameters:**

- `source` (np.ndarray): Input price series
- `leftbars` (np.int16): Number of bars to look back (left window size)
- `rightbars` (np.int16): Number of bars to look forward (right window size)

**Returns:**

- `np.ndarray`: Array with pivot high values marked at their right window edges, NaN elsewhere

**Example:**

```python
# Identify pivot highs with 5 bars left and right
pivot_highs = qk.pivothigh(high, 5, 5)
```

### pivotlow()

```python
pivotlow(source: np.ndarray, leftbars: np.int16,
         rightbars: np.int16) -> np.ndarray
```

Identify pivot lows in a series over specified left and right lookback windows.

**Parameters:**

- `source` (np.ndarray): Input price series
- `leftbars` (np.int16): Number of bars to look back (left window size)
- `rightbars` (np.int16): Number of bars to look forward (right window size)

**Returns:**

- `np.ndarray`: Array with pivot low values marked at their right window edges, NaN elsewhere

**Example:**

```python
# Identify pivot lows with 3 bars left and 3 bars right
pivot_lows = qk.pivotlow(low, 3, 3)
```

---

## <a id="volatility-indicators"></a> Volatility Indicators

Functions for measuring market volatility and price ranges.

### atr()

```python
atr(high: np.ndarray, low: np.ndarray, close: np.ndarray,
    length: np.int16) -> np.ndarray
```

Calculate the Average True Range (ATR) indicator.

**Parameters:**

- `high` (np.ndarray): High price series
- `low` (np.ndarray): Low price series
- `close` (np.ndarray): Close price series
- `length` (np.int16): Period length for smoothing

**Returns:**

- `np.ndarray`: ATR values array

**Note:** Uses Wilder's smoothing method (RMA) for calculation.

**Example:**

```python
# Calculate 14-period ATR
atr14 = qk.atr(high, low, close, 14)
```

### bb()

```python
bb(source: np.ndarray, length: np.int16,
   mult: np.float32) -> tuple[np.ndarray, np.ndarray, np.ndarray]
```

Calculate Bollinger Bands indicator values.

**Parameters:**

- `source` (np.ndarray): Input price series
- `length` (np.int16): Period length for moving average and standard deviation
- `mult` (np.float32): Multiplier for standard deviation bands width

**Returns:**

- `tuple`: Three arrays containing (middle_band, upper_band, lower_band)

**Example:**

```python
# Calculate 20-period Bollinger Bands with 2.0 standard deviation
middle, upper, lower = qk.bb(close, 20, 2.0)

# Identify squeeze conditions
bb_squeeze = (upper - lower) / middle < 0.1
```

### bbw()

```python
bbw(source: np.ndarray, length: np.int16,
    mult: np.float32) -> np.ndarray
```

Calculate Bollinger Bands Width (BBW) indicator values.

**Parameters:**

- `source` (np.ndarray): Input price series
- `length` (np.int16): Period length for moving average and standard deviation
- `mult` (np.float32): Multiplier for standard deviation bands width

**Returns:**

- `np.ndarray`: Array containing Bollinger Bands Width values

**Example:**

```python
# Calculate Bollinger Bands Width
bbw_values = qk.bbw(close, 20, 2.0)

# Identify low volatility periods
low_volatility = bbw_values < np.percentile(bbw_values, 20)
```

### tr()

```python
tr(high: np.ndarray, low: np.ndarray, close: np.ndarray,
   handle_nan: np.bool_) -> np.ndarray
```

Calculate True Range (TR) values from price series.

**Parameters:**

- `high` (np.ndarray): High price series
- `low` (np.ndarray): Low price series
- `close` (np.ndarray): Close price series
- `handle_nan` (np.bool\_): If True, computes first bar TR using current close; if False, returns NaN for first bar

**Returns:**

- `np.ndarray`: True Range values array

**Example:**

```python
# Calculate True Range
tr_values = qk.tr(high, low, close, True)

# Use for volatility analysis
high_volatility = tr_values > np.percentile(tr_values, 80)
```
