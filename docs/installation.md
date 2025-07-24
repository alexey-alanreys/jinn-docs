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

---

## <a id="step-4-configuration"></a> ⚙️ Step 4: Configuration

### Create the Environment File

1. Go to the project root directory.
2. Locate the file `data/.env.example`.
3. Create a new file named `.env`.
4. Copy the contents from `.env.example` and provide your credentials:

```env
# Example (replace with actual credentials)
SERVER_URL=http://127.0.0.1:5000

BYBIT_API_KEY=your-bybit-api-key
BYBIT_API_SECRET=your-bybit-api-secret

BINANCE_API_KEY=your-binance-api-key
BINANCE_API_SECRET=your-binance-api-secret

TELEGRAM_BOT_TOKEN=your-telegram-token
TELEGRAM_CHAT_ID=your-chat-id
```

### API Key Setup Instructions

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

---

**🎉 Installation Complete!**  
You are now ready to use **Jinn** for algorithmic trading.
