# Log Module Reference

The **log** module provides efficient functions for managing trading position records during backtesting and strategy execution. All functions are optimized with Numba JIT compilation for maximum performance.

> **Note:** All code examples assume the module is imported as `log`.

## Table of Contents

- [Deal Management](#deal-management)
- [Log Operations](#log-operations)
- [Position Analysis](#position-analysis)

---

## <a id="deal-management"></a> Deal Management

Functions for creating, closing, and modifying deal records in trading log.

### open()

```python
open(open_deals_log: np.ndarray, position_type: float, order_signal: float,
     order_date: float, order_price: float, order_size: float) -> np.ndarray
```

Adds a new deal to the open deals log, expanding the array if necessary.

**Parameters:**

- `open_deals_log` (np.ndarray): Current open deals log array [n,5]
- `position_type` (float): Type of deal (0 for long, 1 for short)
- `order_signal` (float): Signal value at entry
- `order_date` (float): Timestamp of entry
- `order_price` (float): Price at entry (USDT)
- `order_size` (float): Size of the order (units)

**Returns:**

- `np.ndarray`: Updated open deals log [n+1,5] with fields:
  - `[:, 0]` - position_type (0=long, 1=short)
  - `[:, 1]` - order_signal (signal code)
  - `[:, 2]` - order_date (timestamp)
  - `[:, 3]` - order_price (USDT)
  - `[:, 4]` - order_size (units)

**Example:**

```python
# Open a long position
position_type = 0
order_signal = 100
order_date = time[i]
order_price = close[i]
order_size = 5.0

open_deals_log = log.open(
    open_deals_log,
    position_type,
    order_signal,
    order_date,
    order_price,
    order_size
)
```

### close()

```python
close(completed_deals_log: np.ndarray, commission: float, position_type: float,
      order_signal: float, exit_signal: float, order_date: float,
      exit_date: float, order_price: float, exit_price: float,
      order_size: float, initial_capital: float) -> tuple[np.ndarray, float]
```

Creates and appends a new deal record to completed deals log. Returns updated log and calculated PnL.

**Parameters:**

- `completed_deals_log` (np.ndarray): Existing log array [n,13] or empty [0,13]
- `commission` (float): Broker commission rate in percent
- `position_type` (float): Type of deal (0 for long, 1 for short)
- `order_signal` (float): Signal value at entry
- `exit_signal` (float): Signal value at exit
- `order_date` (float): Timestamp of entry
- `exit_date` (float): Timestamp of exit
- `order_price` (float): Price at entry (USDT)
- `exit_price` (float): Price at exit (USDT)
- `order_size` (float): Size of the order (units)
- `initial_capital` (float): Initial capital for PnL calculations

**Returns:**

- `tuple`: (updated_log, pnl) where:
  - `updated_log`: New log array [n+1,13] with fields:
    - `[:, 0]` - position_type (0=long, 1=short)
    - `[:, 1]` - order_signal (signal code)
    - `[:, 2]` - exit_signal (signal code)
    - `[:, 3]` - order_date (timestamp)
    - `[:, 4]` - exit_date (timestamp)
    - `[:, 5]` - order_price (USDT)
    - `[:, 6]` - exit_price (USDT)
    - `[:, 7]` - order_size (units)
    - `[:, 8]` - pnl (absolute USDT)
    - `[:, 9]` - pnl (percentage)
    - `[:,10]` - cumulative_pnl (USDT)
    - `[:,11]` - cumulative_pnl (%)
    - `[:,12]` - total_commission (USDT)
  - `pnl`: Calculated profit/loss for this deal in USDT

**Example:**

```python
# Close position at stop loss
commission = 0.05
exit_signal = 500
exit_date = time[i]
exit_price = stop_price[i]

completed_deals_log, pnl = log.close(
    completed_deals_log,
    commission,
    position_type,
    order_signal,
    exit_signal,
    order_date,
    exit_date,
    order_price,
    exit_price,
    order_size,
    initial_capital
)
equity += pnl
```

---

## <a id="log-operations"></a> Log Operations

Functions for modifying and maintaining deal log.

### remove()

```python
remove(open_deals_log: np.ndarray, deal_index: int) -> np.ndarray
```

Removes a deal from open deals log by setting its row to NaN.

**Parameters:**

- `open_deals_log` (np.ndarray): Current open deals log array
- `deal_index` (int): Index of the deal to remove

**Returns:**

- `np.ndarray`: Updated open deals log with specified deal removed

**Example:**

```python
# Remove specific deal after full closure
open_deals_log = log.remove(open_deals_log, 0)
```

### resize()

```python
resize(open_deals_log: np.ndarray, deal_index: int, new_size: float) -> np.ndarray
```

Updates the position size for a specific deal in open deals log.

**Parameters:**

- `open_deals_log` (np.ndarray): Current open deals log array [n,5]
- `deal_index` (int): Index of the deal to resize
- `new_size` (float): New position size (units)

**Returns:**

- `np.ndarray`: Updated open deals log with resized position

**Notes:**

- If new size is 0.0 or negative, removes the deal from log
- Used for partial position closures at take-profit levels

**Example:**

```python
# Partial close - reduce position size after first take profit
remaining_size = order_size - take_quantities[0]
open_deals_log = log.resize(open_deals_log, 0, remaining_size)
```

### clear()

```python
clear(open_deals_log: np.ndarray) -> np.ndarray
```

Clears all deals from open deals log by setting all values to NaN.

**Parameters:**

- `open_deals_log` (np.ndarray): Current open deals log array

**Returns:**

- `np.ndarray`: Cleared open deals log (all NaN)

**Example:**

```python
# Clear all open positions after liquidation or full exit
open_deals_log = log.clear(open_deals_log)
position_type = np.nan
# Reset other position variables...
```

---

## <a id="position-analysis"></a> Position Analysis

Functions for analyzing current open positions and calculating aggregate metrics.

### avg_price()

```python
avg_price(open_deals_log: np.ndarray) -> float
```

Calculates the average entry price weighted by position size from open deals log.

**Parameters:**

- `open_deals_log` (np.ndarray): Open deals log array [n,5]

**Returns:**

- `float`: Weighted average entry price, or NaN if no open deals

**Example:**

```python
# Get average entry price for open positions
avg_entry_price = log.avg_price(open_deals_log)
if not np.isnan(avg_entry_price):
    liquidation_price = adjust(
        avg_entry_price * (1 - (1 / leverage)),
        p_precision
    )
    take_price[i] = adjust(
        avg_entry_price * (100 + take_profit) / 100,
        p_precision
    )
```

### size()

```python
size(open_deals_log: np.ndarray) -> float
```

Calculates the total position size from open deals log.

**Parameters:**

- `open_deals_log` (np.ndarray): Open deals log array [n,5]

**Returns:**

- `float`: Total position size across all open deals, or 0.0 if no open deals

**Example:**

```python
# Check current position size
current_size = log.size(open_deals_log)
if current_size > 0:
    # Position is open - manage exits
    # ...
```

### count()

```python
count(open_deals_log: np.ndarray) -> int
```

Counts the number of active (non-NaN) deals in open deals log.

**Parameters:**

- `open_deals_log` (np.ndarray): Open deals log array [n,5]

**Returns:**

- `int`: Number of active open deals

**Example:**

```python
# Limit maximum number of concurrent positions
active_positions = log.count(open_deals_log)
if active_positions < max_positions and entry_signal:
    # Allow new position entry
    # ...
```
