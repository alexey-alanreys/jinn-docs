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

- **Operating System**: Windows 10+, macOS 10.14+, or Linux (Ubuntu 18.04+)
- **Python Version**: 3.13 (required)
- **RAM**: 2 GB minimum (4 GB recommended)
- **Storage**: 2 GB of free space
- **Internet**: Stable broadband connection

### Prerequisites

- API credentials from Binance and/or Bybit
- Telegram bot token (for notifications)
- Basic familiarity with the command line

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

To run the **Jinn** locally on your machine, use the following server configuration:

```env
# --- SERVER CONFIGURATION ---
BASE_URL=
SERVER_PORT=1001
CORS_ORIGINS=http://localhost:5173
```

**Configuration Details:**

- **BASE_URL**: Leave empty. The **Jinn** will automatically use `http://127.0.0.1`.
- **SERVER_PORT**: You can change this port if needed (default: 1001).
- **CORS_ORIGINS**: Keep as `http://localhost:5173` (used for frontend development).

Access the **Jinn** at: `http://127.0.0.1:1001`.

#### Setup 2: Remote Access _(For automation or monitoring)_

To expose the server publicly via a tunneling service (e.g., ngrok):

1. Install and configure ngrok or a similar tool.
2. Start ngrok on the same port as the server:

```bash
ngrok http 1001
```

3. Use the public URL provided by ngrok in your `.env` file:

```env
# --- Server configuration ---
BASE_URL=https://your-ngrok-url.ngrok.io
SERVER_PORT=1001
CORS_ORIGINS=https://your-ngrok-url.ngrok.io
```

**Important:** Replace `your-ngrok-url.ngrok.io` with the actual URL provided by your tunneling service.

### API Key Setup Instructions

Add your exchange and notification credentials to the `.env` file:

```env
# --- Bybit credentials ---
# Get from: https://www.bybit.com/app/user/api-management
BYBIT_API_KEY=your-bybit-api-key
BYBIT_API_SECRET=your-bybit-api-secret

# --- Binance credentials ---
# Get from: https://www.binance.com/en/my/settings/api-management
BINANCE_API_KEY=your-binance-api-key
BINANCE_API_SECRET=your-binance-api-secret

# --- Telegram Integration ---
# Bot token from @BotFather | Chat ID from @userinfobot
TELEGRAM_BOT_TOKEN=your-telegram-token
TELEGRAM_CHAT_ID=your-chat-id
```

#### Binance

- Log in to [binance.com](https://www.binance.com).
- Go to _Account → API Management_.
- Create an API key with **"Enable Reading"** and **"Enable Futures"**.

#### Bybit

- Log in to [bybit.com](https://www.bybit.com).
- Go to _API Management_.
- Create an API key with **"Read-Write"** and **"Unified Trading"**.

#### Telegram Bot

- Contact [@BotFather](https://t.me/botfather) to create a bot.
- Use `/newbot` to generate a token.
- Find your chat ID via [@userinfobot](https://t.me/userinfobot).

---

## <a id="troubleshooting"></a> 🛠️ Troubleshooting

### Python PATH (Windows)

If `python` is not recognized in Command Prompt:

- Reinstall Python and ensure **"Add to PATH"** is selected.
- Alternatively, use `py` instead of `python`.
- Restart your terminal session after installation.

### Poetry Not Recognized

If the `poetry` command is not found:

- Ensure the installation directory (usually `~/.local/bin`) is in your PATH.
- On Unix-like systems, run the following command:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

- On Windows, add `%APPDATA%\Python\Scripts` to your environment variables.

### Virtual Environment Activation Issues

- Use Poetry's built-in shell:

```bash
poetry shell
```

- Or run commands via Poetry explicitly:

```bash
poetry run python run.py
```

### API Connectivity Issues

- Double-check that all API keys are correct.
- Ensure your keys are enabled for trading access.
- Confirm IP whitelist settings (if applicable).
- Check your internet connection and firewall settings.

### Server Access Issues

- Verify the server port is not blocked by firewall.
- Check that BASE_URL matches your actual access method.
- For remote access, ensure the tunneling service is running correctly.

---

**🎉 Installation Complete!**  
You are now ready to use **Jinn** for algorithmic trading.
