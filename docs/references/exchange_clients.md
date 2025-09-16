# Exchange Clients Reference

The **Exchange Clients** provide a unified interface for executing futures trades on **Binance** and **Bybit** exchanges.  
Both clients implement identical methods, ensuring consistent behavior across different exchange platforms.

> **Note:** All code examples assume the client is accessed via `self.client`.

## Table of Contents

- [Market Orders](#market-orders)
- [Limit Orders](#limit-orders)
- [Stop Orders](#stop-orders)
- [Order Management](#order-management)
- [Order Monitoring](#order-monitoring)

---

## <a id="market-orders"></a> Market Orders

Market orders execute immediately at the current market price.

### market_open_long()

```python
market_open_long(symbol: str, size: str, margin: str, leverage: int, hedge: bool) -> None
```

Open long position with market order.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Position size ('10%', '100u', etc.)
- `margin` (str): Margin mode ('cross' or 'isolated')
- `leverage` (int): Leverage multiplier
- `hedge` (bool): Use hedge mode for position

**Example:**

```python
# Open long position with 10% of account balance at 10x leverage
self.client.market_open_long('BTCUSDT', '10%', 'cross', 10, False)
```

### market_open_short()

```python
market_open_short(symbol: str, size: str, margin: str, leverage: int, hedge: bool) -> None
```

Open short position with market order.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Position size ('10%', '100u', etc.)
- `margin` (str): Margin mode ('cross' or 'isolated')
- `leverage` (int): Leverage multiplier
- `hedge` (bool): Use hedge mode for position

**Example:**

```python
# Open short position with 500 USDT at 5x leverage in isolated margin
self.client.market_open_short('ETHUSDT', '500u', 'isolated', 5, True)
```

### market_close_long()

```python
market_close_long(symbol: str, size: str, hedge: bool) -> None
```

Close long position with market order.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Amount to close ('100%', '50u', etc.)
- `hedge` (bool): Use hedge mode for position

**Example:**

```python
# Close 50% of long position
self.client.market_close_long('BTCUSDT', '50%', False)

# Close entire long position
self.client.market_close_long('BTCUSDT', '100%', False)
```

### market_close_short()

```python
market_close_short(symbol: str, size: str, hedge: bool) -> None
```

Close short position with market order.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Amount to close ('100%', '50u', etc.)
- `hedge` (bool): Use hedge mode for position

**Example:**

```python
# Close entire short position
self.client.market_close_short('ETHUSDT', '100%', True)
```

---

## <a id="limit-orders"></a> Limit Orders

Limit orders execute only when the market reaches the specified price level.

### limit_open_long()

```python
limit_open_long(symbol: str, size: str, margin: str, leverage: int,
                price: float, hedge: bool) -> int
```

Open long position with limit order.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Position size ('10%', '100u', etc.)
- `margin` (str): Margin mode ('cross' or 'isolated')
- `leverage` (int): Leverage multiplier
- `price` (float): Limit order price
- `hedge` (bool): Use hedge mode for position

**Returns:**

- `int`: Order ID of created limit order

**Example:**

```python
# Place limit buy order for BTC at 30,000 USDT
order_id = self.client.limit_open_long('BTCUSDT', '5%', 'cross', 10, 30000.0, False)
```

### limit_open_short()

```python
limit_open_short(symbol: str, size: str, margin: str, leverage: int,
                price: float, hedge: bool) -> int
```

Open short position with limit order.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Position size ('10%', '100u', etc.)
- `margin` (str): Margin mode ('cross' or 'isolated')
- `leverage` (int): Leverage multiplier
- `price` (float): Limit order price
- `hedge` (bool): Use hedge mode for position

**Returns:**

- `int`: Order ID of created limit order

**Example:**

```python
# Place limit sell order for ETH at 2,000 USDT
order_id = self.client.limit_open_short('ETHUSDT', '500u', 'isolated', 5, 2000.0, True)
```

### limit_close_long()

```python
limit_close_long(symbol: str, size: str, price: float, hedge: bool) -> int
```

Close long position with limit order (take-profit).

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Amount to close ('100%', '50u', etc.)
- `price` (float): Take profit price level
- `hedge` (bool): Use hedge mode for position

**Returns:**

- `int`: Order ID of created take-profit order

**Example:**

```python
# Set take-profit at 35,000 USDT for BTC long position
order_id = self.client.limit_close_long('BTCUSDT', '100%', 35000.0, False)
```

### limit_close_short()

```python
limit_close_short(symbol: str, size: str, price: float, hedge: bool) -> int
```

Close short position with limit order (take-profit).

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Amount to close ('100%', '50u', etc.)
- `price` (float): Take profit price level
- `hedge` (bool): Use hedge mode for position

**Returns:**

- `int`: Order ID of created take-profit order

**Example:**

```python
# Set take-profit at 1,800 USDT for ETH short position
order_id = self.client.limit_close_short('ETHUSDT', '100%', 1800.0, True)
```

---

## <a id="stop-orders"></a> Stop Orders

Stop orders trigger when the market reaches a specified price level.

### market_stop_close_long()

```python
market_stop_close_long(symbol: str, size: str, price: float, hedge: bool) -> int
```

Place stop-loss order to close long position.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Amount to close ('100%', '50u', etc.)
- `price` (float): Stop price trigger level
- `hedge` (bool): Use hedge mode for position

**Returns:**

- `int`: Order ID of created stop order

**Example:**

```python
# Set stop loss at 28,000 USDT for BTC long position
order_id = self.client.market_stop_close_long('BTCUSDT', '100%', 28000.0, False)
```

### market_stop_close_short()

```python
market_stop_close_short(symbol: str, size: str, price: float, hedge: bool) -> int
```

Place stop-loss order to close short position.

**Parameters:**

- `symbol` (str): Trading symbol
- `size` (str): Amount to close ('100%', '50u', etc.)
- `price` (float): Stop price trigger level
- `hedge` (bool): Use hedge mode for position

**Returns:**

- `int`: Order ID of created stop order

**Example:**

```python
# Set stop loss at 2,100 USDT for ETH short position
order_id = self.client.market_stop_close_short('ETHUSDT', '100%', 2100.0, True)
```

---

## <a id="order-management"></a> Order Management

Methods for managing active orders - bulk cancellation, cancellation by order type or trade direction.

### cancel_all_orders()

```python
cancel_all_orders(symbol: str) -> None
```

Cancel all open orders for specified symbol.

**Parameters:**

- `symbol` (str): Trading symbol

**Example:**

```python
# Cancel all orders for BTC
self.client.cancel_all_orders('BTCUSDT')
```

### cancel_orders()

```python
cancel_orders(symbol: str, side: str) -> None
```

Cancel all orders for specified symbol and side.

**Parameters:**

- `symbol` (str): Trading symbol
- `side` (str): Order side ('buy' or 'sell')

**Example:**

```python
# Cancel all buy orders for ETH
self.client.cancel_orders('ETHUSDT', 'buy')
```

### cancel_limit_orders()

```python
cancel_limit_orders(symbol: str, side: str) -> None
```

Cancel limit orders for specified symbol and side.

**Parameters:**

- `symbol` (str): Trading symbol
- `side` (str): Order side ('buy' or 'sell')

**Example:**

```python
# Cancel all limit buy orders for BTC
self.client.cancel_limit_orders('BTCUSDT', 'buy')
```

### cancel_stop_orders()

```python
cancel_stop_orders(symbol: str, side: str) -> None
```

Cancel stop orders for specified symbol and side.

**Parameters:**

- `symbol` (str): Trading symbol
- `side` (str): Order side ('buy' or 'sell')

**Example:**

```python
# Cancel all stop sell orders for ETH
self.client.cancel_stop_orders('ETHUSDT', 'sell')
```

---

## <a id="order-monitoring"></a> Order Monitoring

Methods for tracking the status of limit and stop orders.

### check_limit_orders()

```python
check_limit_orders(symbol: str, order_ids: list) -> list
```

Check status of limit orders and update alerts.

**Parameters:**

- `symbol` (str): Trading symbol
- `order_ids` (list): List of order IDs to check

**Returns:**

- `list`: List of order IDs that are still active

**Example:**

```python
# Check status of limit orders
active_orders = self.client.check_limit_orders('BTCUSDT', [12345, 67890])
```

### check_stop_orders()

```python
check_stop_orders(symbol: str, order_ids: list) -> list
```

Check status of stop orders and update alerts.

**Parameters:**

- `symbol` (str): Trading symbol
- `order_ids` (list): List of order IDs to check

**Returns:**

- `list`: List of order IDs that are still active

**Example:**

```python
# Check status of stop orders
active_orders = self.client.check_stop_orders('ETHUSDT', [54321, 98765])
```
