# Overview

**Jinn** is a high-performance algorithmic trading framework designed for developing, optimizing, and executing trading strategies across cryptocurrency exchanges. **Jinn** streamlines the entire trading workflow from strategy development to live execution.

## Table of Contents

- [Highlights](#highlights)
- [Architecture](#architecture)
- [Supported Exchanges](#supported-exchanges)
- [Use Cases](#use-cases)
- [Technology Stack](#technology-stack)
- [Web Interface](#web-interface)
- [Next Steps](#next-steps)

---

## <a id="highlights"></a> 🚀 Highlights

- 📈 **Optimization** — Identify strategy parameters to maximize profitability.
- 🔍 **Backtesting** — Evaluate strategies on historical data with robust metrics.
- 🤖 **Live Trading** — Run and manage strategies in real-time market environments.
- 📊 **Web Dashboard** — Centralized monitoring and performance analytics.
- 📢 **Telegram Integration** — Real-time trade alerts and system notifications.
- 🪙 **Multi-Exchange Support** — Unified API access to Binance and Bybit.

---

## <a id="architecture"></a> Architecture

**Jinn** is built on a modular architecture that separates concerns and promotes scalability and maintainability:

```
                              ┌─────────────────┐    ┌─────────────────┐
                              │   Database      │    │   Exchange      │
                              │                 │    │   Clients       │
                              └─────────┬───────┘    └─────────┬───────┘
                                        │                      │
                    ┌───────────────────┼──────────────────────┼───────────────────┐
                    │                   │                      │                   │
                    ▼                   ▼                      ▼                   ▼
            ┌─────────────────┐ ┌─────────────────┐    ┌─────────────────┐ ┌─────────────────┐
            │ HistoryProvider │ │ HistoryProvider │    │ RealtimeProvider│ │ RealtimeProvider│
            │  (Database)     │ │  (Exchanges)    │    │   (Binance)     │ │    (Bybit)      │
            └─────────┬───────┘ └─────────┬───────┘    └─────────┬───────┘ └─────────┬───────┘
                      │                   │                      │                   │
                      └─────────┬─────────┘                      └─────────┬─────────┘
                                │                                          │
                                ▼                                          ▼
                    ┌─────────────────────┐                    ┌─────────────────────┐
                    │ OptimizationService │                    │  ExecutionService   │
                    │                     │                    │                     │
                    └─────────┬───────────┘                    └─────────┬───────────┘
                              │                                          │
                              │              ┌─────────────────┐         │
                              │              │   Strategy      │         │
                              └─────────────►│   Modules       │◄────────┘
                                             │                 │
                                             └─────────┬───────┘
                                                       │
                                                       ▼
                                             ┌─────────────────┐
                                             │  Web Interface  │
                                             │                 │
                                             └─────────────────┘
```

### Core Components

#### OptimizationService

Systematically searches for optimal parameter sets by evaluating strategies on historical data provided by HistoryProvider.

#### ExecutionService

Handles both backtesting on historical data and live trading execution using data from HistoryProvider and RealtimeProvider respectively.

#### HistoryProvider

Provides historical market data from either local database or exchange clients for optimization and backtesting purposes.

#### RealtimeProvider

Supplies real-time market data directly from exchange clients for live trading execution.

---

## <a id="supported-exchanges"></a> 🪙 Supported Exchanges

**Jinn** integrates with the latest official APIs of major cryptocurrency exchanges to support both data retrieval and live trading.

### Binance

- **Futures** — Used for both data collection and live trading.

### Bybit

- **Futures** — Used for both data collection and live trading.

---

## <a id="use-cases"></a> 💼 Use Cases

**Jinn** provides a robust set of features for systematic and scalable algorithmic trading:

- **Strategy Development** — Design and test custom trading algorithms using a modular, extensible architecture.
- **Parameter Optimization** — Systematically identify optimal parameter combinations to maximize profitability.
- **Historical Backtesting** — Evaluate strategy performance against historical data with comprehensive metrics.
- **Live Trading Execution** — Deploy strategies in real-time market environments with automated controls.
- **Centralized Monitoring** — Track all trading activities through a unified dashboard with real-time updates.
- **Quantitative Research** — Explore market data and strategy behavior through an intuitive web interface.
- **Performance Analytics** — Access detailed trading statistics, reports, and visual performance analysis.

---

## <a id="technology-stack"></a> 🛠️ Technology Stack

### Backend

- **Python 3.13+** — Core runtime with enhanced performance.
- **Flask** — Lightweight web framework for REST API and dashboard.
- **Waitress** — Production-ready WSGI server for Flask applications.
- **NumPy** — High-performance numerical computing for trading logic.
- **Numba** — JIT compilation for performance-critical calculations.
- **SQLite** — Lightweight embedded database for local data persistence.

### Frontend

- **Vite** — Lightning-fast build tool and development server.
- **Vanilla JavaScript** — Efficient, framework-free client-side logic.
- **HTML/CSS** — Responsive UI with modern styling.
- **Lightweight Charts** — Professional financial charting library.

---

## <a id="web-interface"></a> 📊 Web Interface

**Jinn** includes a modern, responsive web interface that serves as the primary control center for all application functions. All optimization, backtesting, and live trading operations are managed exclusively through this comprehensive web dashboard.

See [Screenshots Gallery](media/screenshots.md) for a full overview.

#### Light Theme

![Full Interface - Light Theme](media/light-theme/full_interface.png)

#### Dark Theme

![Full Interface - Dark Theme](media/dark-theme/full_interface.png)

### Dashboard Features

- **Complete Application Control** — Full management of optimization, backtesting, and live trading operations.
- **Live Monitoring** — Real-time tracking of open positions, orders, and trading metrics.
- **Interactive Charts** — Zoomable charts with indicator overlays and drawing tools.
- **Performance Analytics** — Full suite of trading metrics, including equity curve.
- **Trade History** — Chronological log of all executed trades with full details.
- **Strategy Management** — Configure, optimize, backtest, and deploy strategies directly via UI.
- **Light & Dark Themes** — Themes for day and night sessions.

---

## <a id="next-steps"></a> ✅ Next Steps

Ready to start trading with **Jinn**? Follow these steps:

- **[Installation Guide](guides/installation.md)** — Complete setup instructions.
- **[Built-in Strategies Guide](guides/workflow.md)** — Workflow for built-in strategies.

---

**⚡ Start Your Algorithmic Trading Journey**  
Transform your trading strategies from ideas into profitable, automated systems with **Jinn's** comprehensive framework.
