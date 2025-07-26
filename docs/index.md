# Overview

**Jinn** is a high-performance algorithmic trading framework designed for developing, optimizing, and automating trading strategies across cryptocurrency exchanges. **Jinn** streamlines the entire trading workflow from strategy development to live execution.

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

- 📈 **Optimization** — Identify strategy parameters to maximize profitability
- 🔍 **Backtesting** — Evaluate strategies on historical data with robust metrics
- 🤖 **Live Execution** — Deploy strategies in real-time market environments
- 📊 **Web Dashboard** — Centralized monitoring and performance analytics
- 🔔 **Telegram Integration** — Real-time trade alerts and system notifications
- 🏪 **Multi-Exchange Support** — Unified API access to Binance and Bybit

---

## <a id="architecture"></a> Architecture

**Jinn** is built on a modular architecture that separates concerns and promotes scalability and maintainability:

```
                       ┌──────────────────┐    ┌────────────────┐
                       │   Data Layer     │    │   Exchange     │
                       │   (OHLCV Data)   │◄───┤   Connectors   │
                       │                  │    │                │
                       └─────────┬────────┘    └────────────────┘
         ┌───────────────────────│──────────────────────┐
         │                       │                      │
         ▼                       ▼                      ▼
┌─────────────────┐    ┌─────────────────┐    ┌────────────────┐
│   Optimization  │    │   Backtesting   │    │   Automation   │
│   Service       │───►│   Service       │◄───│   Service      │
│                 │    │                 │    │                │
└────────┬────────┘    └─────────┬───────┘    └─────────┬──────┘
         │                       │                      │
         ▼                       ▼                      ▼
         │             ┌─────────────────┐              │
         │             │   Strategy      │              │
         └────────────►│   Modules       │◄─────────────┘
                       │                 │
                       └─────────┬───────┘
                                 │
                                 ▼
                       ┌──────────────────┐
                       │   Web interface  │
                       │                  │
                       └──────────────────┘
```

### Core Components

#### Optimization Service

Systematically searches for optimal parameter sets by evaluating strategies on historical data.

#### Backtesting Service

Evaluates the performance and reliability of optimized strategies using historical datasets.

#### Automation Service

Executes validated strategies in real-time using live market data from connected exchanges.

---

## <a id="supported-exchanges"></a> 🏪 Supported Exchanges

**Jinn** integrates with the latest official APIs of major cryptocurrency exchanges to support both data retrieval and live trading.

### Binance

- **Spot** — Used for collecting historical market data only
- **Futures** — Used for both data collection and automated trading

### Bybit

- **Spot** — Used for collection historical market data only
- **Futures** — Used for both data collection and automated trading

---

## <a id="use-cases"></a> 💼 Use Cases

**Jinn** provides a robust set of features for systematic and scalable algorithmic trading:

- **Strategy Development** — Design and test custom trading algorithms using a modular, extensible architecture
- **Parameter Optimization** — Systematically identify optimal parameter combinations to maximize profitability
- **Historical Backtesting** — Evaluate strategy performance against historical data with comprehensive metrics
- **Live Trading Execution** — Deploy strategies in real-time market environments with automated controls
- **Centralized Monitoring** — Track all trading activities through a unified dashboard with real-time updates
- **Quantitative Research** — Explore market data and strategy behavior through an intuitive web interface
- **Performance Analytics** — Access detailed trading statistics, reports, and visual performance analysis

---

## <a id="technology-stack"></a> 🛠️ Technology Stack

### Backend

- **Python 3.13+** — Core runtime with enhanced performance
- **Flask** — Lightweight web framework for REST API and dashboard
- **NumPy** — High-performance numerical computing for trading logic
- **Numba** — JIT compilation for performance-critical calculations
- **SQLite** — Lightweight embedded database for local data persistence

### Frontend

- **Vite** — Lightning-fast build tool and development server
- **Vanilla JavaScript** — Efficient, framework-free client-side logic
- **HTML/CSS** — Responsive UI with modern styling
- **Lightweight Charts** — Professional financial charting library

---

## <a id="web-interface"></a> 📊 Web Interface

**Jinn** includes a modern, responsive web interface for real-time monitoring, multi-strategy control, and performance analysis.

See [Screenshots Gallery](media/screenshots.md) for a full overview.

#### Light Theme

![Full Interface - Light Theme](media/light-theme/01.png)

#### Dark Theme

![Full Interface - Dark Theme](media/dark-theme/01.png)

### Dashboard Features

- **Live Monitoring** — Real-time tracking of open positions, orders, and trading metrics
- **Interactive Charts** — Zoomable charts with indicator overlays and drawing tools
- **Performance Analytics** — Full suite of trading metrics, including equity curve
- **Trade History** — Chronological log of all executed trades with full details
- **Strategy Management** — Configure or remove strategies directly via UI
- **Light & Dark Themes** — Professional themes for day and night sessions

---

## <a id="next-steps"></a> ✅ Next Steps

Ready to start trading with **Jinn**? Follow these steps:

- **[Installation Guide](guides/installation.md)** — Complete setup instructions
- **[Using Built-in Strategies](guides/workflow.md)** — Workflow for built-in strategies

---

**⚡ Start Your Algorithmic Trading Journey**  
Transform your trading strategies from ideas into profitable, automated systems with **Jinn's** comprehensive framework.
