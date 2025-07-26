# Using Built-in Strategies

This guide explains how to use the built-in strategies of **Jinn** across three core phases: optimization, backtesting and automation.

## Table of Contents

- [Getting Started](#getting-started)
- [Optimization](#optimization)
- [Backtesting](#backtesting)
- [Automation](#automation)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## <a id="getting-started"></a> 🚀 Getting Started

### Initial Configuration

Before launching **Jinn**, configure `config/settings.py`, which controls the operation mode and trading parameters.

**Jinn** operates in three distinct modes, controlled by the `MODE` variable:

```python
MODE = enums.Mode.OPTIMIZATION
```

See [Configuration Reference](../references/jinn_configuration.md#enum-reference) for enum definitions.

### Strategic Workflow

**Jinn** follows a systematic three-phase approach:

1. **Optimize** strategies to identify the best parameter combinations.
2. **Backtest** optimized strategies against historical market data.
3. **Automate** validated strategies in live trading environments.

Each mode plays a vital role in the strategy development and deployment lifecycle.

---

## <a id="optimization"></a> 🔧 Optimization

The optimization phase discovers optimal parameter combinations by systematically testing configurations against historical data.

### Single Strategy Optimization

#### 1. Configure Settings

Edit `settings.py` to specify optimization mode and parameters:

```python
MODE = enums.Mode.OPTIMIZATION

OPTIMIZATION_CONFIG = {
    'strategy': enums.Strategy.SANDBOX_V1,
    'exchange': enums.Exchange.BINANCE,
    'market': enums.Market.SPOT,
    'symbol': 'BTCUSDT',
    'interval': '1h',
    'start': '2019-01-01',
    'end': '2023-01-01',
}
```

See [Configuration Reference](../references/jinn_configuration.md#enum-reference) for enum definitions.

#### 2. Run Optimization

```bash
cd path/to/jinn-core
poetry run python run.py
```

### Multiple Strategy Optimization

#### 1. Configure Settings

Edit `settings.py`:

```python
MODE = enums.Mode.OPTIMIZATION
```

#### 2. Create JSON Configuration Files

Generate `optimization.json` files within each strategy directory:

```
src/strategies/[strategy_name]/optimization/optimization.json
```

Example:

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

See [Configuration Reference](../references/jinn_configuration.md#json-enum-reference) for JSON enum definitions.

#### 3. Run Optimization

```bash
poetry run python run.py
```

The system will process all configured strategies in parallel.

---

## <a id="backtesting"></a> 📊 Backtesting

The backtesting phase evaluates optimized strategies using historical data.

### Single Strategy Backtesting

#### 1. Configure Settings

Edit `settings.py` to specify backtesting mode and parameters:

```python
MODE = enums.Mode.BACKTESTING

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

See [Configuration Reference](../references/jinn_configuration.md#enum-reference) for enum definitions.

#### 2. Run Backtesting

```bash
poetry run python run.py
```

#### 3. Analyze Results

Open your web browser and navigate to:

```
http://127.0.0.1:5000
```

### Multiple Strategy Backtesting

#### 1. Configure Settings

Edit `settings.py`:

```python
MODE = enums.Mode.BACKTESTING
```

#### 2. Prepare Parameter Files

Move optimized parameter files:

```
From: src/strategies/[strategy]/optimization/
To:   src/strategies/[strategy]/backtesting/
```

#### 3. Adjust Time Periods (Optional)

Modify date ranges in parameter files if desired.

#### 4. Run Backtesting

```bash
poetry run python run.py
```

#### 5. Analyze Results

Open your web browser and navigate to:

```
http://127.0.0.1:5000
```

---

## <a id="automation"></a> 🤖 Automation

Automation mode runs strategies in real-time with live market data.

### Single Strategy Automation

#### 1. Configure Settings

Edit `settings.py` to specify automation mode and parameters:

```python
MODE = enums.Mode.AUTOMATION

AUTOMATION_CONFIG = {
    'strategy': enums.Strategy.SANDBOX_V1,
    'exchange': enums.Exchange.BINANCE,
    'symbol': 'XRPUSDT',
    'interval': 1
}
```

See [Configuration Reference](../references/jinn_configuration.md#enum-reference) for enum definitions.

#### 2. Run Automation

```bash
poetry run python run.py
```

#### 3. Monitor Performance

Open your web browser and navigate to:

```
http://127.0.0.1:5000
```

### Multiple Strategy Automation

#### 1. Configure Settings

Edit `settings.py`:

```python
MODE = enums.Mode.AUTOMATION
```

#### 2. Prepare Parameter Files

**Option A: Manual Configuration**

Create JSON files in the automation directories:

```
src/strategies/[strategy]/automation/[EXCHANGE]_[SYMBOL]_[INTERVAL].json
```

Example filename:

```
BYBIT_BTCUSDT_1.json
```

**Option B: Reuse Optimized Parameters**

Copy files from `optimization` to `automation`. The system will select the first available parameter set for execution.

See [Configuration Reference](../references/jinn_configuration.md#json-configuration-files) for JSON structure examples.

#### 3. Run Automation

```bash
poetry run python run.py
```

#### 4. Monitor Performance

Open your web browser and navigate to:

```
http://127.0.0.1:5000
```

---

## <a id="best-practices"></a> 📝 Best Practices

### Strategy Development

1. **Always Optimize First:** Never skip the optimization phase.
2. **Use Out-of-Sample Testing:** Test on unseen historical data.
3. **Consider Market Regimes:** Test across various market conditions.
4. **Monitor Drawdowns:** Set acceptable risk thresholds.

### Risk Management

1. **Start Small:** Use minimal capital for new strategies.
2. **Diversify:** Avoid relying on a single strategy or symbol.
3. **Set Stop Losses:** Define maximum acceptable losses.
4. **Regular Monitoring:** Check performance at least once per day.

### System Maintenance

1. **Keep APIs Updated:** Regularly verify exchange connectivity.
2. **Monitor Resources:** Ensure sufficient CPU and memory.
3. **Backup Configurations:** Save successful parameter sets.
4. **Update Dependencies:** Keep all Python packages current.

### Performance Optimization

1. **Use Appropriate Intervals:** High-frequency strategies require more resources.
2. **Limit Concurrent Strategies:** Avoid overloading the system.
3. **Optimize Network:** Ensure low-latency, stable internet.

---

## <a id="troubleshooting"></a> 🛠️ Troubleshooting

### Common Issues

#### Strategy Not Executing

- Check API credentials and permissions.
- Verify symbol availability on the exchange.
- Confirm historical data availability.
- Ensure sufficient account balance.

#### Poor Performance

- Reassess optimization parameters.
- Check for overfitting in backtesting.
- Consider changes in market conditions.

#### Connection Issues

- Test internet connection.
- Verify exchange API status.
- Review firewall settings.

### Getting Help

1. Review the [Configuration Reference](../references/jinn_configuration.md).
2. Check application logs for errors.
3. Contact the author for support.

---

**🎯 Ready to Trade?**  
Start with paper trading or small position sizes to validate your strategy before scaling up.
