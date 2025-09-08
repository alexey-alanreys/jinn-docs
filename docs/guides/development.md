# Strategy Development Guide

This guide shows how to build custom trading strategies for **Jinn**, using `ExampleV2` as a detailed example. Integrating a strategy into **Jinn** is straightforward: custom strategy modules are simply placed in the `jinn-core/src/core/strategies` folder. In addition to reading this guide, it is strongly recommended to independently study the built-in **Jinn** strategies `ExampleV1` and `ExampleV2`.

## Table of Contents

- [Strategy Architecture](#strategy-architecture)
- [Strategy Parameters](#strategy-parameters)
- [Core Implementation](#core-implementation)
- [Handling Timeframes](#handling-timeframes)
- [Frontend Integration](#frontend-integration)
- [Best Practices](#best-practices)
- [References](#references)

---

## <a id="strategy-architecture"></a> 🏗️ Strategy Architecture

Custom strategies in **Jinn** inherit from the `BaseStrategy` class and must implement two core methods:

- **`calculate()`** — Computes indicators and generates trading signals.
- **`trade()`** — Executes orders based on calculated signals.

The architecture supports:

- Parameter optimization across defined ranges.
- Multi-timeframe analysis for comprehensive market view.
- Frontend visualization with custom indicators.
- Real-time trading execution with live market data.

---

## <a id="strategy-parameters"></a> ⚙️ Strategy Parameters

Each strategy defines its configuration through two parameter dictionaries:

- `params` — Contains default values used during backtesting and live trading.
- `opt_params` — Defines ranges of values used for parameter optimization.

### Strategy Parameters Dictionary

Define default parameters for your strategy:

```python
class SandboxV1(BaseStrategy):
    params = {
        'order_size': 90.0,
        'min_capital': 100.0,
        'stop_type': 1,
        'stop': 2.0,
        'trail_stop': 1,
        'trail_percent': 30.0,
        'take_percent_1': 4.0,
        'take_percent_2': 6.0,
        'take_percent_3': 12.8,
        # ... other parameters
    }
```

**Important:** Each parameter must be one of the following types: `bool`, `int`, or `float`.

`BaseStrategy` provides common parameters that can be overridden in the inheriting strategy:

```python
base_params = {
    # Core trading settings
    'direction': 0,              # 0 - all, 1 - longs, 2 - shorts
    'margin_type': 0,            # 0 - ISOLATED, 1 - CROSSED
    'leverage': 1,               # Leverage multiplier

    # Capital management
    'initial_capital': 10000.0,  # Starting capital in USDT
    'position_size_type': 0,     # 0 - PERCENT, 1 - CURRENCY
    'position_size': 100.0,      # Size in % or absolute value
    'commission': 0.05,          # Fee percentage (0.05 = 0.05%)
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
    'take_percent_1': [i / 10 for i in range(12, 47)],
    'take_percent_2': [i / 10 for i in range(48, 87)],
    'take_percent_3': [i / 10 for i in range(88, 147)],
    # ... other parameter ranges
}
```

---

## <a id="core-implementation"></a> 🔧 Core Implementation

### Class Structure

```python
import numpy as np
import numba as nb

from . import (
    BaseStrategy,
    BaseExchangeClient,
    Interval,
    adjust,
    colors,
    quantklines,
    update_completed_deals_log
)


class ExampleV2(BaseStrategy):
    def __init__(self, params: dict | None = None) -> None:
        super().__init__(params)

    def calculate(self) -> None:
        pass

    def trade(self, client: BaseExchangeClient) -> None:
        pass
```

### Calculate Method Implementation

The `calculate()` method computes indicators and generates signals:

```python
def calculate(self) -> None:
    # Exit price levels
    self.stop_price = np.full(self.time.shape[0], np.nan)
    self.take_prices = np.array(
        [
            np.full(self.time.shape[0], np.nan),
            np.full(self.time.shape[0], np.nan),
            np.full(self.time.shape[0], np.nan)
        ]
    )
    self.take_percents = np.array([
        self.params['take_percent_1'],
        self.params['take_percent_2'],
        self.params['take_percent_3'],
    ])
    self.liquidation_price = np.nan
    self.stop_moved = False

    # Quantity management
    self.take_volumes = np.array([
        self.params['take_volume_1'],
        self.params['take_volume_2'],
        self.params['take_volume_3'],
    ])
    self.take_quantities = np.full(5, np.nan)

    # Technical indicators
    self.dst = quantklines.dst(
        high=self.high,
        low=self.low,
        close=self.close,
        factor=self.params['st_factor'],
        atr_length=self.params['st_atr_period']
    )
    self.upper_band_change = quantklines.change(
        source=self.dst[0],
        length=1
    )
    self.lower_band_change = quantklines.change(
        source=self.dst[1],
        length=1
    )
    self.rsi = quantklines.rsi(
        source=self.close,
        length=self.params['rsi_length']
    )

    # Additional market data
    self.htf_close = np.full(self.time.shape[0], np.nan)

    if len(self.feeds_data['klines']['HTF']['close'].shape) == 1:
        self.htf_close = self.feeds_data['klines']['HTF']['close']

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

Calculations that require explicit element-wise iteration should be placed inside the `_calculate_loop` method. The method should be decorated with `@staticmethod` and `@nb.njit(cache=True, nogil=True)`.

```python
@staticmethod
@nb.njit(cache=True, nogil=True)
def _calculate_loop(
    direction: int,
    initial_capital: float,
    min_capital: float,
    commission: float,
    position_size_type: int,
    position_size: float,
    # ... other parameters
) -> tuple:
    for i in range(1, time.shape[0]):
        stop_price[i] = stop_price[i - 1]
        take_prices[:, i] = take_prices[:, i - 1]

        # Reset alerts
        alert_cancel = False
        alert_open_long = False
        alert_open_short = False
        alert_long_new_stop = False
        alert_short_new_stop = False

        # Long position management
        if position_type == 0:
            # Stop loss check
            if low[i] <= stop_price[i]:
                # Close position and log deal
                completed_deals_log, pnl = update_completed_deals_log(
                    completed_deals_log,
                    commission,
                    position_type,
                    order_signal,
                    500,
                    order_date,
                    time[i],
                    order_price,
                    stop_price[i],
                    order_size,
                    initial_capital
                )
                equity += pnl
                # Reset position variables
                # ...

            # Take profit checks
            if (
                not np.isnan(take_prices[0, i]) and
                high[i] >= take_prices[0, i]
            ):
                # Partial close at first TP level
                # ...

        # Entry signal logic
        entry_long = (
            (close[i] / dst_lower_band[i] - 1) * 100 > st_lower_band and
            (close[i] / dst_lower_band[i] - 1) * 100 < st_upper_band and
            rsi[i] < rsi_long_upper_limit and
            rsi[i] > rsi_long_lower_limit and
            np.isnan(position_type) and
            (bb_rsi_upper[i] < bb_long_limit
                if bb_filter else True) and
            (adx[i] < adx_long_upper_limit and
                adx[i] > adx_long_lower_limit
                if adx_filter else True) and
            (equity > min_capital) and
            (direction == 0 or direction == 1)
        )

        if entry_long:
            # Initialize long position
            position_type = 0
            order_signal = 100
            order_price = close[i]
            # Calculate position size, stops, targets...

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
def trade(self, client: BaseExchangeClient) -> None:
    # Cancel all orders if needed
    if self.alert_cancel:
        client.trade.cancel_all_orders(self.symbol)

    # Check order status
    self.order_ids['stop_ids'] = client.trade.check_stop_orders(
        symbol=self.symbol,
        order_ids=self.order_ids['stop_ids']
    )
    self.order_ids['limit_ids'] = client.trade.check_limit_orders(
        symbol=self.symbol,
        order_ids=self.order_ids['limit_ids']
    )

    # Update stop loss
    if self.alert_long_new_stop:
        client.trade.cancel_stop_orders(
            symbol=self.symbol,
            side='sell'
        )
        self.order_ids['stop_ids'] = client.trade.check_stop_orders(
            symbol=self.symbol,
            order_ids=self.order_ids['stop_ids']
        )
        order_id = client.trade.market_stop_close_long(
            symbol=self.symbol,
            size='100%',
            price=self.stop_price[-1],
            hedge=False
        )

        if order_id:
            self.order_ids['stop_ids'].append(order_id)

    # Open long position
    if self.alert_open_long:
        client.trade.market_open_long(
            symbol=self.symbol,
            size=(
                f'{self.params['position_size']}'
                f'{'u' if self.params['position_size_type'] else '%'}'
            ),
            margin=(
                'cross' if self.params['margin_type'] else 'isolated'
            ),
            leverage=self.params['leverage'],
            hedge=False
        )
        order_id = client.trade.market_stop_close_long(
            symbol=self.symbol,
            size='100%',
            price=self.stop_price[-1],
            hedge=False
        )

        if order_id:
            self.order_ids['stop_ids'].append(order_id)

        order_id = client.trade.limit_close_long(
            symbol=self.symbol,
            size=f'{self.take_volumes[0]}%',
            price=self.take_prices[0, -1],
            hedge=False
        )

        if order_id:
            self.order_ids['limit_ids'].append(order_id)
```

See [Exchange Clients Reference](../references/exchange_clients.md) for a detailed overview of available methods and usage examples for `client`.

---

## <a id="handling-timeframes"></a> ⏳ Handling Timeframes

### Main Timeframe Data

`BaseStrategy` provides data from the main timeframe via NumPy arrays:

```python
def calculate(self) -> None:
    # The following arrays are now available for the main timeframe:
    # self.time   – timestamps
    # self.open   – opening prices
    # self.high   – high prices
    # self.low    – low prices
    # self.close  – closing prices
    # self.volume – trading volume

    # Use main timeframe data directly in calculations
    self.rsi = quantklines.rsi(source=self.close, length=14)
    self.sma = quantklines.sma(source=self.close, length=20)
```

### Defining Data Feeds

To use multi-timeframe data, define the required feeds in your strategy's `feeds` dictionary:

```python
# --- Data Feed Configuration ---
# Market data sources required for strategy calculations
feeds = {
    'klines': {
        'HTF': ['symbol', Interval.MIN_5],
        'LTF': ['symbol', Interval.MIN_1],
    },
}
```

The configuration format is as follows:

- **Key**: Unique identifier (e.g., "HTF", "LTF", or any custom name).
- **Value**: An array in the format `["symbol_or_pair", "timeframe"]` where:
  - First element is either `"symbol"` (current trading pair) or an explicit symbol like `"BTCUSDT"`.
  - Second element is the timeframe from the Interval enumeration (e.g., `Interval.MIN_5`).

### Accessing Feed Data

Once feeds are defined, **Jinn** exposes OHLCV data for each feed via `self.feeds_data`:

```python
def calculate(self) -> None:
    # Additional market data
    self.htf_close = np.full(self.time.shape[0], np.nan)

    if len(self.feeds_data['klines']['HTF']['close'].shape) == 1:
        self.htf_close = self.feeds_data['klines']['HTF']['close']
```

---

## <a id="frontend-integration"></a> 🎨 Frontend Integration

### Parameter Visualization

Set appropriate labels for each parameter that will be displayed in the interface:

```python
# --- UI/UX Configuration ---
# Human-readable labels for frontend parameter display
param_labels = {
    'position_size': 'Position Size',
    'min_capital': 'Min Capital',
    'stop_type': 'Stop Type',
    'trail_stop': 'Trailing Stop',
    'stop': 'Stop Loss (%)',
    'trail_percent': 'Trail Percent',
    'take_percent_1': 'Take Profit 1 (%)',
    'take_percent_2': 'Take Profit 2 (%)',
    'take_percent_3': 'Take Profit 3 (%)',
    # ... other parameters
}
```

### Indicator Visualization

Configure indicators for frontend rendering:

```python
# --- Visualization Settings ---
# Chart styling configuration for technical indicators
indicator_options = {
    'HTF': {
        'pane': 0,
        'type': 'line',
        'color': colors.CRIMSON,
    },
    'SL': {
        'pane': 0,
        'type': 'line',
        'color': colors.CRIMSON,
    },
    'TP #1': {
        'pane': 0,
        'type': 'line',
        'color': colors.GREEN,
    },
    'Volume': {
        'pane': 1,
        'type': 'histogram',
    },
    # ... other settings
}

# Set up indicators dictionary in calculate()
self.indicators = {
    'HTF': {
        'options': self.indicator_options['HTF'],
        'values': self.htf_close,
    },
    'SL': {
        'options': self.indicator_options['SL'],
        'values': self.stop_price,
    },
    'TP #1': {
        'options': self.indicator_options['TP #1'],
        'values': self.take_prices[0],
    },
    'Volume': {
        'options': self.indicator_options['Volume'],
        'values': self.volume,
        'colors': np.where(
            self.close >= self.open,
            self.volume_color_1,
            self.volume_color_2
        ),
    },
    # ... other indicators
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
self.indicators = {
    'Volume': {
        'options': self.indicator_options['Volume'],
        'values': self.volume,
        'colors': np.where(
            self.close >= self.open,
            self.volume_color_1,
            self.volume_color_2
        ),
    },
    # ... other indicators
}
```

---

## <a id="best-practices"></a> 📝 Best Practices

### Code Organization

1. **Optimize Performance** — Apply Numba JIT compilation to speed up intensive calculations and loops.
2. **Separate Concerns** — Keep indicator calculations and order execution logic clearly separated and organized.
3. **Handle Edge Cases** — Always check for NaNs, empty arrays, and invalid values before processing.
4. **Use Type Hints** — Define parameter types for improved code clarity and IDE support.

### Strategy Development

1. **Start Simple** — Begin with basic indicators and gradually add complexity after validation.
2. **Validate Logic** — Test each component independently before integrating into the full strategy.
3. **Test Thoroughly** — Use backtesting extensively before deploying in live trading environments.
4. **Monitor Performance** — Regularly review strategy performance and adjust parameters as market conditions change.

### Risk Management

1. **Implement Stop-Loss Logic** — Always define clear exit conditions to limit potential losses.
2. **Monitor Drawdowns** — Enforce strict limits on maximum portfolio drawdown to protect capital.
3. **Avoid Excessive Leverage** — Use leverage carefully and understand its impact on risk exposure.
4. **Position Sizing** — Implement proper position sizing based on account balance and risk tolerance.

---

## <a id="references"></a> 📚 References

- **Constants** — See [Constants Reference](../references/constants.md) for signal codes, indicator color definitions, and style settings.
- **Technical Analysis** — See [QuantKlines Library Reference](../references/quantklines_lib.md) for technical analysis functions and indicators.
- **Exchange Clients** — See [Exchange Clients Reference](../references/exchange_clients.md) for trading API methods and usage examples.

---

**🎯 Ready to Build?**  
Start with a simple strategy focusing on one or two indicators, then gradually add complexity as you validate each component. Always thoroughly backtest your strategies before deploying them in live trading environments.
