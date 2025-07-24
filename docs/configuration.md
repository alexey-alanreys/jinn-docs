# Configuration Reference

This reference provides an overview of all configuration mechanisms used in **Jinn**.

## Table of Contents

- [Core Settings (settings.py)](#core-settings)
- [Enum Reference](#enum-reference)
- [JSON Configuration Files](#json-configuration-files)
- [JSON Enum Reference](#json-enum-reference)

---

## <a id="core-settings"></a> Core Settings (settings.py)

### Operation Mode

```python
MODE = enums.Mode.OPTIMIZATION
```

### Mode-Specific Configuration

#### Optimization Mode

```python
OPTIMIZATION_CONFIG = {
    'strategy': enums.Strategy.SANDBOX_V1,
    'exchange': enums.Exchange.BINANCE,
    'market': enums.Market.SPOT,
    'symbol': 'BTCUSDT',
    'interval': '1h',
    'start': '2019-01-01',
    'end': '2025-01-01',
}
```

#### Backtesting Mode

```python
BACKTESTING_CONFIG = {
    'strategy': enums.Strategy.SANDBOX_V1,
    'exchange': enums.Exchange.BINANCE,
    'market': enums.Market.SPOT,
    'symbol': 'BTCUSDT',
    'interval': '1h',
    'start': '2023-01-01',
    'end': '2025-01-01',
}
```

#### Automation Mode

```python
AUTOMATION_CONFIG = {
    'strategy': enums.Strategy.SANDBOX_V1,
    'exchange': enums.Exchange.BINANCE,
    'symbol': 'XRPUSDT',
    'interval': 1
}
```

---

## <a id="enum-reference"></a> Enum Reference

### Operation Modes

| Value                     | Description       |
| ------------------------- | ----------------- |
| `enums.Mode.OPTIMIZATION` | Optimization mode |
| `enums.Mode.BACKTESTING`  | Backtesting mode  |
| `enums.Mode.AUTOMATION`   | Automation mode   |

### Strategies

| Value                            | Description              |
| -------------------------------- | ------------------------ |
| `enums.Strategy.DAILY_PROFIT_V1` | DailyProfit strategy, v1 |
| `enums.Strategy.DEVOURER_V3`     | Devourer strategy, v3    |
| `enums.Strategy.MEAN_STRIKE_V1`  | MeanStrike strategy, v1  |
| `enums.Strategy.NUGGET_V2`       | Nugget strategy, v2      |
| `enums.Strategy.NUGGET_V4`       | Nugget strategy, v4      |
| `enums.Strategy.NUGGET_V5`       | Nugget strategy, v5      |
| `enums.Strategy.SANDBOX_V1`      | Sandbox strategy, v1     |
| `enums.Strategy.SISTER_V1`       | Sister strategy, v1      |

### Exchanges

| Value                    | Description      |
| ------------------------ | ---------------- |
| `enums.Exchange.BINANCE` | Binance exchange |
| `enums.Exchange.BYBIT`   | Bybit exchange   |

### Markets

| Value                  | Description    |
| ---------------------- | -------------- |
| `enums.Market.SPOT`    | Spot market    |
| `enums.Market.FUTURES` | Futures market |

### Intervals — Binance

| Value   | Description |
| ------- | ----------- |
| `"1m"`  | 1 minute    |
| `"5m"`  | 5 minutes   |
| `"15m"` | 15 minutes  |
| `"30m"` | 30 minutes  |
| `"1h"`  | 1 hour      |
| `"2h"`  | 2 hours     |
| `"4h"`  | 4 hours     |
| `"6h"`  | 6 hours     |
| `"12h"` | 12 hours    |
| `"1d"`  | 1 day       |

### Intervals — Bybit

| Value | Description |
| ----- | ----------- |
| `1`   | 1 minute    |
| `5`   | 5 minutes   |
| `15`  | 15 minutes  |
| `30`  | 30 minutes  |
| `60`  | 1 hour      |
| `120` | 2 hours     |
| `240` | 4 hours     |
| `360` | 6 hours     |
| `720` | 12 hours    |
| `"D"` | 1 day       |

---

## <a id="json-configuration-files"></a> JSON Configuration Files

### Optimization Files

**Path**:  
`src/strategies/[strategy_name]/optimization/optimization.json`

**Structure Example**:

```json
[
  {
    "exchange": "BINANCE",
    "market": "SPOT",
    "symbol": "BTCUSDT",
    "interval": "1h",
    "start": "2020-01-01",
    "end": "2025-01-01"
  },
  {
    "exchange": "BINANCE",
    "market": "SPOT",
    "symbol": "ETHUSDT",
    "interval": "1h",
    "start": "2020-01-01",
    "end": "2025-01-01"
  }
]
```

### Automation Files

**Option A — Manual Configuration**

**Path**:  
`src/strategies/[strategy_name]/automation/[EXCHANGE]_[SYMBOL]_[INTERVAL].json`  
_Example_: `BYBIT_BTCUSDT_1.json`

**Structure Example**:

```json
{
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
  "st_upper_band": 5.8,
  "st_lower_band": 2.9,
  "rsi_length": 6,
  "rsi_long_upper_limit": 29.0,
  "rsi_long_lower_limit": 28.0,
  "rsi_short_upper_limit": 69.0,
  "rsi_short_lower_limit": 58.0,
  "bb_filter": false,
  "ma_length": 22,
  "bb_mult": 2.7,
  "bb_long_limit": 24.0,
  "bb_short_limit": 67.0,
  "adx_filter": false,
  "adx_length": 6,
  "di_length": 14,
  "adx_long_upper_limit": 44.0,
  "adx_long_lower_limit": 28.0,
  "adx_short_upper_limit": 77.0,
  "adx_short_lower_limit": 1.0
}
```

Parameters must match those declared in the strategy’s `.py` module.

**Option B — Optimized Parameters**

**Path**:  
`src/strategies/[strategy_name]/automation/[EXCHANGE]_[MARKET]_[SYMBOL]_[INTERVAL].json`  
_Example_: `BINANCE_SPOT_BTCUSDT_1h.json`

**Structure Example**:

```json
[
  {
    "period": {
      "start": "2020-01-01",
      "end": "2024-01-01"
    },
    "params": {
      "stop_type": 2,
      "stop": 2.8,
      "trail_stop": 2,
      "trail_percent": 47.0,
      "take_percent": [4.1, 8.2, 11.4],
      "take_volume": [10.0, 20.0, 70.0],
      "st_atr_period": 7,
      "st_factor": 15.7,
      "st_upper_band": 5.8,
      "st_lower_band": 1.4,
      "rsi_length": 5,
      "rsi_long_upper_limit": 33.0,
      "rsi_long_lower_limit": 10.0,
      "rsi_short_upper_limit": 76.0,
      "rsi_short_lower_limit": 65.0,
      "bb_filter": false,
      "ma_length": 16,
      "bb_mult": 1.6,
      "bb_long_limit": 22.0,
      "bb_short_limit": 67.0,
      "adx_filter": true,
      "adx_length": 5,
      "di_length": 17,
      "adx_long_upper_limit": 43.0,
      "adx_long_lower_limit": 5.0,
      "adx_short_upper_limit": 82.0,
      "adx_short_lower_limit": 39.0
    }
  }
]
```

---

## <a id="json-enum-reference"></a> JSON Enum Reference

### Exchanges

| Value       | Description      |
| ----------- | ---------------- |
| `"BINANCE"` | Binance exchange |
| `"BYBIT"`   | Bybit exchange   |

### Markets

| Value       | Description    |
| ----------- | -------------- |
| `"SPOT"`    | Spot market    |
| `"FUTURES"` | Futures market |

### Intervals — Binance

| Value   | Description |
| ------- | ----------- |
| `"1m"`  | 1 minute    |
| `"5m"`  | 5 minutes   |
| `"15m"` | 15 minutes  |
| `"30m"` | 30 minutes  |
| `"1h"`  | 1 hour      |
| `"2h"`  | 2 hours     |
| `"4h"`  | 4 hours     |
| `"6h"`  | 6 hours     |
| `"12h"` | 12 hours    |
| `"1d"`  | 1 day       |

### Intervals — Bybit

| Value | Description |
| ----- | ----------- |
| `1`   | 1 minute    |
| `5`   | 5 minutes   |
| `15`  | 15 minutes  |
| `30`  | 30 minutes  |
| `60`  | 1 hour      |
| `120` | 2 hours     |
| `240` | 4 hours     |
| `360` | 6 hours     |
| `720` | 12 hours    |
| `"D"` | 1 day       |
