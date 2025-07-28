# Strategy Development Guide

This guide shows how to build custom trading strategies for **Jinn**, using `SandboxV1` as a detailed example.

> **Note:** The full implementation of `SandboxV1` is located at `src/strategies/sandbox_v1/sandbox_v1.py`.

## Table of Contents

- [Strategy Integration](#strategy-integration)
- [Strategy Architecture](#strategy-architecture)
- [Strategy Parameters](#strategy-parameters)
- [Core Implementation](#core-implementation)
- [Handling Timeframes](#handling-timeframes)
- [Frontend Integration](#frontend-integration)
- [Best Practices](#best-practices)
- [References](#references)

---

## <a id="strategy-integration"></a> 📁 Strategy Integration

### 1. Create Directory Structure

Create the following folder structure for your strategy:

```text
src/strategies/[strategy_name]/
│   .gitignore
│   [strategy_name].py
│   __init__.py
│
├───automation/
│       .gitkeep
│
├───backtesting/
│       .gitkeep
│
└───optimization/
        .gitkeep
```

> **Note:** Replace `[strategy_name]` with the actual name of your strategy.

### 2. Update Import Files

Add your strategy to `src/strategies/__init__.py`:

```python
from .[strategy_name] import [StrategyClassName]
```

Add to `src/strategies/[strategy_name]/__init__.py`:

```python
from .[strategy_name] import [StrategyClassName]
```

### 3. Register Strategy Enum

Add your strategy to `src/core/enums.py` in the `Strategy` class:

```python
from src.strategies import [StrategyClassName]

class Strategy(Enum):
    """
    Available strategies. Maps strategy names
    to their corresponding implementation classes.
    """

    [STRATEGY_NAME] = [StrategyClassName]
    # ... other strategies
```

> **Note:** `[STRATEGY_NAME]` must be unique within the enum. Uppercase is recommended but not required.

### 4. Configure Settings

Update `config/settings.py` to use your strategy:

```python
OPTIMIZATION_CONFIG = {
    'strategy': enums.Strategy.[STRATEGY_NAME],
    'exchange': enums.Exchange.BINANCE,
    'market': enums.Market.SPOT,
    'symbol': 'BTCUSDT',
    'interval': '1h',
    'start': '2018-01-01',
    'end': '2025-07-01'
}

BACKTESTING_CONFIG = {
    'strategy': enums.Strategy.[STRATEGY_NAME],
    'exchange': enums.Exchange.BINANCE,
    'market': enums.Market.SPOT,
    'symbol': 'BTCUSDT',
    'interval': '1h',
    'start': '2018-01-01',
    'end': '2025-07-01'
}

AUTOMATION_CONFIG = {
    'strategy': enums.Strategy.[STRATEGY_NAME],
    'exchange': enums.Exchange.BINANCE,
    'symbol': 'XRPUSDT',
    'interval': 1
}
```

> **Note:** Make sure `[STRATEGY_NAME]` used in `settings.py` matches the enum key registered in the `Strategy` class.

---

## <a id="strategy-architecture"></a> 🏗️ Strategy Architecture

Custom strategies in **Jinn** inherit from the `BaseStrategy` class and must implement two core methods:

- **`calculate()`**: Computes indicators and generates trading signals.
- **`trade()`**: Executes orders based on calculated signals.

The architecture supports:

- Parameter optimization
- Multi-timeframe analysis
- Frontend visualization
- Real-time trading

---

## <a id="strategy-parameters"></a> ⚙️ Strategy Parameters

Each strategy defines its configuration through two parameter dictionaries:

- `params` — contains default values used during backtesting and live trading
- `opt_params` — defines ranges of values used for parameter optimization

### Strategy Parameters Dictionary

Define default parameters for your strategy:

```python
class SandboxV1(BaseStrategy):
    params = {
        "order_size": 90.0,
        "min_capital": 100.0,
        "stop_type": 1,
        "stop": 2.0,
        "trail_stop": 1,
        "trail_percent": 30.0,
        "take_percent": [2.0, 4.0, 6.0],
        "take_volume": [10.0, 10.0, 50.0],
        "st_atr_period": 6,
        "st_factor": 24.6,
        # ... other params
    }
```

> **Note:** Parameter names must be in double quotes.

`BaseStrategy` provides common parameters that can be overridden in the inheriting strategy:

```python
    base_params = {
        # Core trading settings
        "direction": 0,              # 0 - all, 1 - longs, 2 - shorts
        "margin_type": 0,            # 0 - ISOLATED, 1 - CROSSED
        "leverage": 1,               # Leverage multiplier

        # Capital management
        "initial_capital": 10000.0,  # Starting capital in USDT
        "order_size_type": 0,        # 0 - PERCENT, 1 - CURRENCY
        "order_size": 100.0,         # Size in % or absolute value
        "commission": 0.05,          # Fee percentage (0.05 = 0.05%)
    }
```

### Optimization Parameters

Define parameter ranges for optimization:

```python
opt_params = {
    'stop_type': [1, 2],
    'trail_stop': [1, 2],
    'stop': [i / 10 for i in range(1, 31)],
    'trail_percent': [float(i) for i in range(0, 55, 1)],
    'take_percent': [
        [
            randint(12, 46) / 10,
            randint(48, 86) / 10,
            randint(88, 146) / 10
        ] for _ in range(100)
    ],
    # ... other params
}
```

---

## <a id="core-implementation"></a> 🔧 Core Implementation

### Class Structure

```python
from random import randint

import numpy as np
import numba as nb

import src.core.quantklines as qk
import src.constants.colors as colors
from src.core.strategy.base_strategy import BaseStrategy
from src.core.strategy.deal_logger import update_completed_deals_log
from src.utils.rounding import adjust


class SandboxV1(BaseStrategy):
    # Parameters and optimization settings...

    def __init__(self, client, all_params=None, opt_params=None) -> None:
        super().__init__(client, all_params, opt_params)

    def calculate(self, market_data) -> None:
        # Implementation...
        pass

    def trade(self) -> None:
        # Implementation...
        pass
```

### Calculate Method Implementation

The `calculate()` method computes indicators and generates signals:

```python
def calculate(self, market_data) -> None:
    # Initialize base variables from market data
    super().init_variables(market_data)

    # Initialize strategy-specific arrays
    self.stop_price = np.full(self.time.shape[0], np.nan)
    self.take_price = np.array([
        np.full(self.time.shape[0], np.nan),
        np.full(self.time.shape[0], np.nan),
        np.full(self.time.shape[0], np.nan)
    ])

    # Position tracking variables
    self.liquidation_price = np.nan
    self.qty_take = np.full(5, np.nan)
    self.stop_moved = False

    # Calculate technical indicators
    self.dst = qk.dst(
        high=self.high,
        low=self.low,
        close=self.close,
        factor=self.params['st_factor'],
        atr_length=self.params['st_atr_period']
    )
    self.upper_band_change = qk.change(
        source=self.dst[0],
        length=1
    )
    self.lower_band_change = qk.change(
        source=self.dst[1],
        length=1
    )
    self.rsi = qk.rsi(
        source=self.close,
        length=self.params['rsi_length']
    )

    # Alert flags for signals
    self.alert_cancel = False
    self.alert_open_long = False
    self.alert_open_short = False
    self.alert_long_new_stop = False
    self.alert_short_new_stop = False

    # Main calculation loop (Numba-optimized)
    (
        self.completed_deals_log,
        self.open_deals_log,
        self.take_price,
        self.stop_price,
        # ... other outputs
    ) = self._calculate_loop(
        # Pass all necessary parameters...
    )
```

See [QuantKlines Library Reference](../references/quantklines_lib.md) for a detailed overview of the available functions in the **QuantKlines** library.

### Numba-Optimized Calculation Loop

Calculations that require explicit element-wise iteration should be placed inside the `_calculate_loop` method.  
The method should be decorated with `@staticmethod` and `@nb.njit(cache=True, nogil=True)`.

```python
@staticmethod
@nb.njit(cache=True, nogil=True)
def _calculate_loop(
    direction: int,
    initial_capital: float,
    # ... other parameters
    time: np.ndarray,
    high: np.ndarray,
    low: np.ndarray,
    close: np.ndarray,
    # ... technical indicators
) -> tuple:
    for i in range(1, time.shape[0]):
        # Carry forward previous prices to the current step
        stop_price[i] = stop_price[i - 1]
        take_price[:, i] = take_price[:, i - 1]

        # Reset alerts
        alert_cancel = False
        alert_open_long = False
        alert_open_short = False
        alert_long_new_stop = False
        alert_short_new_stop = False

        ...

        # Long position management
        if deal_type == 0:  # Long position
            # Stop loss check
            if low[i] <= stop_price[i]:
                # Close position and log deal
                completed_deals_log, pnl = update_completed_deals_log(
                    completed_deals_log,
                    commission,
                    deal_type,
                    entry_signal,
                    500,
                    entry_date,
                    time[i],
                    entry_price,
                    stop_price[i],
                    position_size,
                    initial_capital
                )
                equity += pnl
                # Reset position variables
                # ...

            # Take profit checks
            if not np.isnan(take_price[0, i]) and high[i] >= take_price[0, i]:
                # Partial close at first TP level
                # ...

        # Entry signal logic
        entry_long = (
            rsi[i] < rsi_long_upper_limit and
            rsi[i] > rsi_long_lower_limit and
            np.isnan(deal_type) and
            # Additional conditions...
        )

        if entry_long:
            # Initialize long position
            deal_type = 0
            entry_signal = 100
            entry_price = close[i]
            # Calculate position size, stops, targets...

    ...

    return (
        completed_deals_log,
        open_deals_log,
        take_price,
        stop_price,
        # ... other outputs
    )
```

See [Constants Reference](../references/constants.md#signal-codes) for predefined entry and close signal codes.

### Trade Method Implementation

The `trade()` method executes orders based on calculated signals:

```python
def trade(self) -> None:
    # Load cached order IDs
    if self.order_ids is None:
        self.order_ids = self.cache.load(self.symbol)

    # Cancel all orders if needed
    if self.alert_cancel:
        self.client.cancel_all_orders(self.symbol)

    # Check order status
    self.order_ids['stop_ids'] = self.client.check_stop_orders(
        symbol=self.symbol,
        order_ids=self.order_ids['stop_ids']
    )
    self.order_ids['limit_ids'] = self.client.check_limit_orders(
        symbol=self.symbol,
        order_ids=self.order_ids['limit_ids']
    )

    # Update stop-loss
    if self.alert_long_new_stop:
        self.client.cancel_stop_orders(
            symbol=self.symbol,
            side='Sell'
        )
        self.order_ids['stop_ids'] = self.client.check_stop_orders(
            symbol=self.symbol,
            order_ids=self.order_ids['stop_ids']
        )
        order_id = self.client.market_stop_close_long(
            symbol=self.symbol,
            size='100%',
            price=self.stop_price[-1],
            hedge=False
        )

        if order_id:
            self.order_ids['stop_ids'].append(order_id)

    ...

    # Open long position
    if self.alert_open_long:
        self.client.market_open_long(
            symbol=self.symbol,
            size=(
                f'{self.params['order_size']}'
                f'{'u' if self.params['order_size_type'] else '%'}'
            ),
            margin=(
                'cross' if self.params['margin_type'] else 'isolated'
            ),
            leverage=self.params['leverage'],
            hedge=False
        )
        order_id = self.client.market_stop_close_long(
            symbol=self.symbol,
            size='100%',
            price=self.stop_price[-1],
            hedge=False
        )

        if order_id:
            self.order_ids['stop_ids'].append(order_id)

        order_id = self.client.limit_close_long(
            symbol=self.symbol,
            size=f'{self.params['take_volume'][0]}%',
            price=self.take_price[0, -1],
            hedge=False
        )

        if order_id:
            self.order_ids['limit_ids'].append(order_id)

        ...

    # Save order IDs to cache
    self.cache.save(self.symbol, self.order_ids)
```

See [Exchange Clients Reference](../references/exchange_clients.md) for a detailed overview of available methods and usage examples for `self.client`.

---

## <a id="handling-timeframes"></a> ⏳ Handling Timeframes

### Main Timeframe Data

`BaseStrategy` provides data from the main timeframe via NumPy arrays:

```python
def calculate(self, market_data):
    # Initialize base variables from the main timeframe
    super().init_variables(market_data)

    # The following arrays are now available for the main timeframe:
    # self.time   – timestamps
    # self.open   – opening prices
    # self.high   – high prices
    # self.low    – low prices
    # self.close  – closing prices
    # self.volume – trading volume

    # Use main timeframe data directly in calculations
    self.rsi = qk.rsi(source=self.close, length=14)
    self.sma = qk.sma(source=self.close, length=20)
```

### Defining Data Feeds

To use multi-timeframe data, define the required feeds in your strategy's `params` dictionary:

```python
params = {
    # ... other parameters
    "feeds": {
        "klines": {
            "HTF": ["symbol", "1d"],        # Higher timeframe - daily data for current pair
            "LTF": ["symbol", "15m"],       # Lower timeframe - 15-minute data for current pair
            "BTC_HTF": ["BTCUSDT", "1d"],   # Bitcoin daily data
        }
    }
}
```

The configuration format is as follows:

- **Key**: Unique identifier (e.g., "HTF", "LTF", or any custom name).
- **Value**: An array in the format `["symbol_or_pair", "timeframe"]` where:
  - First element is either `"symbol"` (current trading pair) or an explicit symbol like `"BTCUSDT"`.
  - Second element is the timeframe string (e.g., `"1m"`, `"15m"`, `"1h"`, `"1d"`).

### Accessing Feed Data

Once feeds are defined, **Jinn** exposes OHLCV data for each feed via `self.feeds`:

```python
def calculate(self, market_data):
    # Higher timeframe (HTF) data
    htf_time = self.feeds['klines']['HTF']['time']
    htf_open = self.feeds['klines']['HTF']['open']
    htf_high = self.feeds['klines']['HTF']['high']
    htf_low = self.feeds['klines']['HTF']['low']
    htf_close = self.feeds['klines']['HTF']['close']
    htf_volume = self.feeds['klines']['HTF']['volume']

    # Lower timeframe (LTF) data
    ltf_time = self.feeds['klines']['LTF']['time']
    ltf_close = self.feeds['klines']['LTF']['close']

    # External symbol data (e.g., BTC)
    btc_time = self.feeds['klines']['BTC_HTF']['time']
    btc_close = self.feeds['klines']['BTC_HTF']['close']
```

### Timeframe Alignment functions

Time series from different timeframes must be aligned to the main timeframe before being used in calculations.

#### Aligning Higher Timeframes

Use `stretch()` from **QuantKlines** to expand higher timeframe data to match the main timeframe:

```python
self.htf_close = qk.stretch(
    higher_tf_source=self.feeds["klines"]["HTF"]["close"],
    higher_tf_time=self.feeds["klines"]["HTF"]["time"],
    target_tf_time=self.time
)
```

#### Aligning Lower Timeframes

Use `shrink()` from **QuantKlines** to compress lower timeframe data to the main timeframe resolution:

```python
self.ltf_close = qk.shrink(
    lower_tf_source=self.feeds["klines"]["LTF"]["close"],
    lower_tf_time=self.feeds["klines"]["LTF"]["time"],
    target_tf_time=self.time
)
```

### Full Example

Here's how the `SandboxV1` strategy uses multi-timeframe data:

```python
class SandboxV1(BaseStrategy):
    params = {
        # ... other parameters
        "feeds": {
            "klines": {
                "HTF": ["symbol", "1d"],
                "LTF": ["symbol", "15m"],
            }
        }
    }

    def calculate(self, market_data):
        # Initialize base variables
        super().init_variables(market_data)

        # Align higher timeframe close prices
        self.htf_close = qk.stretch(
            higher_tf_source=self.feeds['klines']['HTF']['close'],
            higher_tf_time=self.feeds['klines']['HTF']['time'],
            target_tf_time=self.time
        )

        # Align lower timeframe data
        self.ltf_close = qk.shrink(
            lower_tf_source=self.feeds['klines']['LTF']['close'],
            lower_tf_time=self.feeds['klines']['LTF']['time'],
            target_tf_time=self.time
        )
```

---

## <a id="frontend-integration"></a> 🎨 Frontend Integration

### Indicator Visualization

Configure indicators for frontend rendering:

```python
# Frontend rendering settings
indicator_options = {
    'SL': {
        'pane': 0,
        'type': 'line',
        'color': colors.CRIMSON
    },
    'TP #1': {
        'pane': 0,
        'type': 'line',
        'color': colors.GREEN
    },
    'Volume': {
        'pane': 1,
        'type': 'histogram'
    },
    'ADX': {
        'pane': 2,
        'type': 'line',
        'color': colors.DEEP_SKY_BLUE,
        'lineWidth': 1
    }
}

# Set up indicators dictionary in calculate()
self.indicators = {
    'SL': {
        'options': self.indicator_options['SL'],
        'values': self.stop_price
    },
    'TP #1': {
        'options': self.indicator_options['TP #1'],
        'values': self.take_price[0]
    },
    'Volume': {
        'options': self.indicator_options['Volume'],
        'values': self.volume,
        'colors': self.volume_colors  # Optional color array
    },
    'ADX': {
        'options': self.indicator_options['ADX'],
        'values': self.dmi[2]
    }
}
```

See [Constants Reference](../references/constants.md) for predefined values used in indicator visualization and styling.

### Dynamic Colors

Define dynamic colors for volume bars:

```python
# Define volume colors
volume_color_1 = colors.PEACOCK_GREEN
volume_color_2 = colors.CHERRY_RED

# Assign colors in calculate() method
self.volume_colors = np.where(
    self.close >= self.open,
    self.volume_color_1,
    self.volume_color_2
)
```

---

## <a id="best-practices"></a> 📝 Best Practices

### Code Organization

1. **Optimize Performance**: Apply Numba to speed up intensive calculations.
2. **Separate Concerns**: Keep indicator calculations and order logic isolated.
3. **Handle Edge Cases**: Check for NaNs, empty arrays, and invalid values.
4. **Use Type Hints**: Define parameter types for clarity and IDE support.

### Risk Management

1. **Monitor Drawdowns**: Enforce limits on maximum portfolio drawdown to protect capital.
2. **Use Partial Exits**: Consider scaling out of positions to lock in profits progressively.
3. **Implement Stop-Loss Logic**: Always define clear exit conditions to limit losses.
4. **Avoid Excessive Leverage**: Use leverage carefully to limit losses.

---

## <a id="references"></a> 📚 References

- **Constants**: See [Constants Reference](../references/constants.md) for signal codes, indicator color definitions, and style settings.
- **Technical Analysis**: See [QuantKlines Library Reference](quantklines_lib.md) for technical analysis functions.
- **Exchange Clients**: See [Exchange Clients Reference](exchange_clients.md) for trading API methods.

---

**🎯 Ready to Build?**  
Start with a simple strategy focusing on one or two indicators, then gradually add complexity as you validate each component.
