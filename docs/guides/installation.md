# Installation Guide

This guide provides step-by-step instructions for setting up **Jinn** using Docker.

## Table of Contents

- [System Requirements](#system-requirements)
- [Download the Jinn Framework](#download-the-jinn-framework)
- [Docker Setup & Running Jinn](#docker-setup-running-jinn)
- [Setup Configuration](#setup-configuration)
- [Troubleshooting](#troubleshooting)

---

## <a id="system-requirements"></a> 📋 System Requirements

### Minimum Requirements

- **Docker**: 24.0+ (with Docker Compose)
- **RAM**: 2 GB minimum (4 GB recommended)
- **Storage**: 2 GB free space
- **Internet**: Stable connection

### Prerequisites

- API credentials from Binance and/or Bybit.
- Telegram bot token (for notifications).
- Basic familiarity with the command line.

---

## <a id="download-the-jinn-framework"></a> 📦 Download the Jinn Framework

Clone the repository to get the latest version of the project:

### Windows

```cmd
cd C:\Trading
git clone https://github.com/albert-alanreys/jinn-core.git
cd jinn-core
```

### macOS/Linux

```bash
cd ~/Trading
git clone https://github.com/albert-alanreys/jinn-core.git
cd jinn-core
```

### Strategy Integration

**Jinn** ships without trading strategies (except for examples: ExampleV1, ExampleV2, etc.). Strategies developed in-house or acquired from third parties must be integrated into **Jinn** before use.

To integrate a custom strategy into **Jinn**, place the strategy module in the `jinn-core/src/core/strategies` folder.

---

## <a id="docker-setup-running-jinn"></a> 🐳 Docker Setup & Running Jinn

### Install Docker

1. Go to the official Docker website: https://www.docker.com/get-started
2. Download Docker Desktop for your operating system.
3. Follow the installation instructions.
4. On Linux, install Docker Engine and Docker Compose using your package manager.

### Build the Docker Image

First, build the Docker image:

```bash
docker compose build
```

### Start the Container

Start the container in detached mode:

```bash
docker compose up -d
```

- `-d` runs the container in the background.

### Run Jinn Application

After the container is running, execute the following command to start the **Jinn** application:

```bash
docker exec jinn-core-jinn-1 python run.py
```

You should see output similar to:

```
2025-10-03 00:03:20 - INFO - __main__ - 👉 Open: http://127.0.0.1:1001
```

Access the server at `http://localhost:1001/`.

### Stop the Application

The application runs in the foreground and blocks input. To stop it:

- Press `Ctrl+C` in the terminal where the application is running.

### Stop the Container

To stop the Docker container:

```bash
docker compose down
```

This will stop and remove the container. Your data and configuration will be preserved.

---

## <a id="setup-configuration"></a> ⚙️ Setup Configuration

### Create the Environment File

1. Navigate to the project root directory.
2. Locate the file: `.env.example`.
3. Create a new file named `.env`.
4. Copy the contents of `.env.example` into `.env` and adjust the values according to your setup.

### Server Configuration

Choose one of the following deployment setups:

#### Local Development (Default)

To run **Jinn** locally on your machine, use the following server configuration:

```env
# --- Basic Server Settings ---
BASE_URL=
SERVER_PORT=1001

# --- CORS & Security ---
CORS_ORIGINS=
```

**Configuration Details:**

- **BASE_URL**: Leave empty for local setup.
- **SERVER_PORT**: Default 1001, can be changed.
- **CORS_ORIGINS**: Leave empty for local setup.

Access **Jinn** at: `http://localhost:1001/`.

#### Remote Access (Optional)

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

### Exchange API Credentials

Add your exchange credentials to the `.env` file. For multiple accounts, separate keys and secrets with commas:

```env
# --- Bybit Exchange ---
# Get from: https://www.bybit.com/app/user/api-management
BYBIT_API_KEYS=your-first-bybit-key,your-second-bybit-key
BYBIT_API_SECRETS=your-first-bybit-secret,your-second-bybit-secret

# --- Binance Exchange ---
# Get from: https://www.binance.com/en/my/settings/api-management
BINANCE_API_KEYS=your-first-binance-key,your-second-binance-key
BINANCE_API_SECRETS=your-first-binance-secret,your-second-binance-secret
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

- **MAX_PROCESSES**: Number of parallel optimization processes; leave empty to auto-detect CPU cores.
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

### Docker Issues

- Try rebuilding the image with `docker compose build --no-cache` if build issues occur.
- Check Docker logs with `docker compose logs` for detailed error information.
- Ensure Docker Desktop is running before executing commands.

### API Connectivity Problems

- Verify all API keys and secrets are correctly copied without extra spaces.
- Check IP whitelist settings on your exchange accounts (if configured).
- Ensure your API keys have the required permissions enabled.

### Server Access Issues

- Try accessing via different browsers or in incognito/private mode.
- Verify the server port is not blocked by firewall or antivirus.
- Check that no other applications are using the same port.

### Performance Issues

- Reduce OPTIMIZATION_ITERATIONS and POPULATION_SIZE.
- Increase MAX_PROCESSES only up to your CPU core count.
- Close unnecessary applications during optimization runs.

---

**🎉 Installation Complete!**  
You are now ready to use **Jinn** for algorithmic trading.
