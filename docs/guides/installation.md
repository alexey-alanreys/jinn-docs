# Installation Guide

This guide provides step-by-step instructions for setting up **Jinn**. Please follow the steps carefully to ensure reliable installation.

## Table of Contents

- [System Requirements](#system-requirements)
- [Install Python](#step-1-install-python-313)
- [Download the Framework](#step-2-download-the-jinn-framework)
- [Install Dependencies](#step-3-install-dependencies-using-poetry)
- [Configuration](#step-4-configuration)
- [Troubleshooting](#troubleshooting)

---

## <a id="system-requirements"></a> 📋 System Requirements

### Minimum Requirements

- **Operating System**: Windows 10+, macOS 10.14+, or Linux (Ubuntu 18.04+).
- **Python Version**: 3.13 (required).
- **RAM**: 2 GB minimum (4 GB recommended).
- **Storage**: 2 GB of free space.
- **Internet**: Stable broadband connection.

### Prerequisites

- API credentials from Binance and/or Bybit.
- Telegram bot token (for notifications).
- Basic familiarity with the command line.

---

## <a id="step-1-install-python-313"></a> 🐍 Step 1: Install Python 3.13

### Windows

1. Download Python 3.13 from [python.org](https://www.python.org/downloads/).
2. Run the installer as **Administrator**.
3. Ensure **"Add Python to PATH"** is checked.
4. Verify installation:

```cmd
python --version
```

### macOS

```bash
# Recommended: install via Homebrew
brew install python@3.13

# Or download and install manually from python.org
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install python3.13 python3.13-pip python3.13-venv
python3.13 --version
```

---

## <a id="step-2-download-the-jinn-framework"></a> 📦 Step 2: Download the Jinn Framework

You can either clone the repository or extract the ZIP archive manually, depending on your preference.

### Windows

1. Open a terminal and navigate to your project directory:

```cmd
cd C:\Trading
```

2. Clone the repository:

```cmd
git clone https://github.com/albert-alanreys/jinn-core.git
```

3. Navigate into the project folder:

```cmd
cd jinn-core
```

### macOS/Linux

1. Open a terminal and navigate to your project directory:

```bash
cd ~/Trading
```

2. Clone the repository:

```bash
git clone https://github.com/albert-alanreys/jinn-core.git
```

3. Navigate into the project folder:

```bash
cd jinn-core
```

---

## <a id="step-3-install-dependencies-using-poetry"></a> 📚 Step 3: Install Dependencies Using Poetry

### 1. Install Poetry (if not already installed)

```bash
pip install --user poetry
```

### 2. Install Project Dependencies

```bash
poetry install --no-root
```

### 3. Verify the Virtual Environment

```bash
poetry env info
```

### 4. Test the Installation

Run a test launch to verify that **Jinn** is running correctly:

```bash
poetry run python run.py
```

The server should start successfully and display startup messages.  
Press `Ctrl+C` to stop the server after confirming it runs without errors.

---

## <a id="step-4-configuration"></a> ⚙️ Step 4: Configuration

### Create the Environment File

1. Navigate to the project root directory.
2. Locate the file: `.env.example`.
3. Create a new file named `.env`.
4. Copy the contents of `.env.example` into `.env` and adjust the values according to your setup.

### Server Configuration

Choose one of the following deployment setups:

#### Setup 1: Local Development _(Standard for everyday use)_

To run **Jinn** locally on your machine, use the following server configuration:

```env
# --- Basic Server Settings ---
BASE_URL=
SERVER_PORT=1001

# --- CORS & Security ---
CORS_ORIGINS=http://localhost:5173
```

**Configuration Details:**

- **BASE_URL**: Leave empty. **Jinn** will automatically use `http://127.0.0.1`.
- **SERVER_PORT**: You can change this port if needed (default: 1001).
- **CORS_ORIGINS**: Keep as `http://localhost:5173` (used for frontend development).

Access **Jinn** at: `http://127.0.0.1:1001`.

#### Setup 2: Remote Access _(For external monitoring or automation)_

To expose the server publicly via a tunneling service (e.g., ngrok):

1. Install and configure ngrok or a similar tunneling service.
2. Start ngrok on the same port as the server:

```bash
ngrok http 1001
```

3. Use the public URL provided by ngrok in your `.env` file:

```env
# --- Basic Server Settings ---
BASE_URL=https://your-ngrok-url.ngrok.io
SERVER_PORT=1001

# --- CORS & Security ---
CORS_ORIGINS=https://your-ngrok-url.ngrok.io
```

**Important:** Replace `your-ngrok-url.ngrok.io` with the actual URL provided by your tunneling service.

### Exchange API Credentials

Add your exchange credentials to the `.env` file:

```env
# --- Bybit Exchange ---
# Get from: https://www.bybit.com/app/user/api-management
BYBIT_API_KEY=your-bybit-api-key
BYBIT_API_SECRET=your-bybit-api-secret

# --- Binance Exchange ---
# Get from: https://www.binance.com/en/my/settings/api-management
BINANCE_API_KEY=your-binance-api-key
BINANCE_API_SECRET=your-binance-api-secret
```

#### Binance API Setup

1. Log in to [binance.com](https://www.binance.com).
2. Navigate to _Account → API Management_.
3. Create a new API key with **"Enable Reading"** and **"Enable Futures"** permissions.
4. Copy the API Key and Secret Key to your `.env` file.

#### Bybit API Setup

1. Log in to [bybit.com](https://www.bybit.com).
2. Navigate to _API Management_.
3. Create a new API key with **"Read-Write"** and **"Unified Trading"** permissions.
4. Copy the API Key and Secret Key to your `.env` file.

### Telegram Integration

Configure Telegram notifications for trade alerts and system messages:

```env
# --- Bot Configuration ---
# Bot token from @BotFather | Chat ID from @userinfobot
TELEGRAM_BOT_TOKEN=your-telegram-token
TELEGRAM_CHAT_ID=your-chat-id
```

#### Setting up Telegram Bot

1. Contact [@BotFather](https://t.me/botfather) on Telegram.
2. Use the `/newbot` command to create a new bot.
3. Follow the prompts to set up your bot and receive the bot token.
4. Find your chat ID by messaging [@userinfobot](https://t.me/userinfobot).

### Optimization Configuration

Configure the optimization engine parameters for strategy development:

```env
# --- Parallel Processing Parameters ---
MAX_PROCESSES=

# --- Optimization Parameters ---
OPTIMIZATION_ITERATIONS=2000
OPTIMIZATION_RUNS=3

# --- Population Parameters ---
POPULATION_SIZE=200
MAX_POPULATION_SIZE=300

# --- Data Splitting Parameters ---
TRAIN_WINDOW=0.7
TEST_WINDOW=0.3
```

#### Parameter Explanations

- **MAX_PROCESSES**: Number of parallel processes for optimization. Leave empty for automatic detection based on CPU cores.
- **OPTIMIZATION_ITERATIONS**: Number of iterations per optimization run (default: 2000).
- **OPTIMIZATION_RUNS**: Number of independent optimization runs to perform (default: 3).
- **POPULATION_SIZE**: Initial population size for genetic algorithm (default: 200).
- **MAX_POPULATION_SIZE**: Maximum population size during optimization (default: 300).
- **TRAIN_WINDOW**: Proportion of data used for training (default: 0.7 = 70%).
- **TEST_WINDOW**: Proportion of data used for testing (default: 0.3 = 30%).

#### ⚠️ Optimization Note

**Quality vs. Overfitting Trade-off**: While higher values for `OPTIMIZATION_ITERATIONS` can lead to more refined results, they simultaneously **increase the risk of overfitting** the strategy to historical noise rather than discovering robust market patterns.

**Always validate** optimized parameters on a completely unseen data set (forward walk-testing) before live deployment. Adjust these parameters based on your hardware capabilities, time constraints, and required robustness.

---

## <a id="troubleshooting"></a> 🛠️ Troubleshooting

### Python PATH Issues (Windows)

If `python` is not recognized in Command Prompt:

- Reinstall Python and ensure **"Add to PATH"** is selected during installation.
- Alternatively, use `py` instead of `python`.
- Restart your terminal session after installation.
- Manually add Python to PATH via System Environment Variables if needed.

### Poetry Installation Issues

If the `poetry` command is not found:

- Ensure the installation directory is in your PATH:
  - Unix-like systems: `~/.local/bin`
  - Windows: `%APPDATA%\Python\Scripts`
- Add to PATH manually:

```bash
# Unix-like systems
export PATH="$HOME/.local/bin:$PATH"

# Windows (add via Environment Variables GUI)
# Add %APPDATA%\Python\Scripts to your PATH
```

### Virtual Environment Issues

If you encounter virtual environment problems:

- Use Poetry's built-in shell activation:

```bash
poetry shell
```

- Run commands via Poetry explicitly:

```bash
poetry run python run.py
```

- Reset the virtual environment if corrupted:

```bash
poetry env remove python
poetry install --no-root
```

### API Connectivity Problems

If you encounter API-related issues:

- Verify all API keys and secrets are correctly copied without extra spaces.
- Ensure your API keys have the required permissions enabled.
- Check IP whitelist settings on your exchange accounts (if configured).
- Confirm your internet connection and firewall settings allow outbound connections.
- Test API connectivity using the exchange's official testing tools.

### Server Access Issues

If the web interface is not accessible:

- Verify the server port (default: 1001) is not blocked by firewall or antivirus.
- Check that no other applications are using the same port.
- Ensure BASE_URL configuration matches your access method.
- For remote access, confirm the tunneling service is active and properly configured.
- Try accessing via different browsers or in incognito/private mode.

### Performance Issues

If optimization runs slowly or fails:

- Reduce OPTIMIZATION_ITERATIONS and POPULATION_SIZE.
- Increase MAX_PROCESSES only up to your CPU core count.
- Close unnecessary applications during optimization runs.

---

**🎉 Installation Complete!**  
You are now ready to use **Jinn** for algorithmic trading.
