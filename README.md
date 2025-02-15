   # C-TRADING BOT

   𝓑𝓮𝓱𝓲𝓷𝓭 𝓽𝓱𝓲𝓼 𝓫𝓸𝓽, 𝓽𝓱𝓮𝓻𝓮 𝓲𝓼 𝓶𝓸𝓻𝓮 𝓽𝓱𝓪𝓷 𝓳𝓾𝓼𝓽 𝓬𝓸𝓭𝓮. 𝓑𝓮𝓷𝓮𝓪𝓽𝓱 𝓽𝓱𝓲𝓼 𝓫𝓸𝓽, 𝓽𝓱𝓮𝓻𝓮 𝓲𝓼 𝓫𝓵𝓸𝓸𝓭, 𝓼𝔀𝓮𝓪𝓽, 𝓽𝓮𝓪𝓻𝓼, 𝓪𝓷𝓭 𝓪𝓷 𝓲𝓭𝓮𝓪… 𝓐𝓷𝓭 𝓲𝓭𝓮𝓪𝓼 𝓪𝓻𝓮 𝓾𝓷𝓼𝓽𝓸𝓹𝓹𝓪𝓫𝓵𝓮.

>  under development... to be updated...

![image](https://github.com/user-attachments/assets/2025b8aa-4439-40da-b3d5-c4d57d723896)



This project implements an automated cryptocurrency trading bot that interacts with the Binance API to fetch market data, generate trading signals based on technical analysis, manage risk, and execute trades. The bot leverages advanced strategies for both market entry and exit conditions using technical indicators such as RSI, MACD, EMA, ATR, and others.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Logging and Database](#logging-and-database)
- [Error Handling and Risk Management](#error-handling-and-risk-management)
- [License](#license)

---

## Overview

The Binance Trading Bot is designed to automate the trading process with a focus on technical analysis. It continuously monitors the market, analyzes historical and real-time data, and makes decisions to either buy or sell based on customizable strategies. The project integrates several custom modules:

- **Market Data Analysis:** Fetching historical data, calculating indicators, and predicting candle prices.
- **Trade Signal Generation:** Leveraging trend analysis, technical conditions, and dynamic risk management to generate "BUY" or "SELL" signals.
- **Risk and Position Management:** Determining position size based on account balance, stop-loss, and take-profit levels.
- **Trade Execution:** Placing both market and limit orders, managing pending orders, and handling order cancellations.
- **Logging and Tracking:** Recording every transaction into a CSV file and a database for performance tracking and auditing.

The bot is built using organized modules for strategies, risk management, order execution, and logging. This modularity ensures maintainability and scalability of the project.

---

## Features

- **Real-time Market Data:** Retrieves and processes candlestick data from Binance with error handling and time synchronization.
- **Technical Analysis:** Uses multiple technical indicators such as RSI, MACD, EMA, ATR, and SuperTrend to assess market conditions.
- **Automated Trade Execution:** Executes trades based on signals generated by the underlying strategy, including limit and market orders.
- **Risk Management:** Implements dynamic target calculation, trailing stops, and position sizing based on defined risk per trade.
- **Logging and Audit Trail:** Logs trade details and signals to both CSV files and a relational database for further analysis.
- **EMA Rejection Strategy:** Detects rejection at critical EMA levels to trigger sell signals.
- **Portfolio Distribution:** Monitors portfolio balance (USDT vs. trading coin) to adjust trading status accordingly.

---

## Architecture

The bot structure is divided into several key modules:

- **bot.py:** The main entry point that orchestrates data fetching, signal generation, order execution, and active trade management.
- **TrendStrategy:** Handles market analysis and technical indicator calculations, along with generating buy/sell signals.
- **DynamicRiskManager:** Computes dynamic targets and trailing stops based on volatility and ATR.
- **CSVTradeLogger & TradePositionManager:** Responsible for logging trade transactions and managing open positions in the database.
- **LimitSellOrderExecutor and limitBuyOrderExecutor:** Facilitate placing and managing limit orders on Binance.
- **PositionCostCalculator:** Computes current trade profitability, position size, and other cost-related metrics.
- **EMARejectStrategy:** Implements the logic to track EMA rejections to help determine exit decisions.

The project uses internal organization modules, configuration constants (via a `Config` class), and exception handling to ensure resilience during trading.

---

## Installation

1. **Clone the repository:**

    ```
    git@github.com:klncgty/Cryptocurrency-Trading-Bot.git
    ```

2. **Install dependencies:**

    The project depends on several Python packages including `pandas`, `numpy`, `talib`, and the `binance` python client, among others. Install them via pip:

    ```
    pip install -r requirements.txt
    ```

3. **Environment Setup:**

    Create a `.env` file in the project root with your API credentials and other sensitive configurations:

    ```
    API_KEY=your_binance_api_key_here
    API_SECRET=your_binance_api_secret_here
    ```

4. **Database Setup:**

    The bot uses an SQLite database (or other supported SQL databases). Ensure proper permissions for creating and modifying the database file.

---

## Configuration

All configuration settings (such as risk parameters, timeframes, symbol names, and thresholds) are managed in the `Config` class. Key parameters include:

- **SYMBOL:** The trading pair symbol (e.g., `SHIB`).
- **TIMEFRAME:** The candlestick interval (e.g., `5m`).
- **RISK_PER_TRADE:** The percentage of the account balance risked per trade.
- **MIN_RISK_REWARD:** The minimum risk/reward ratio for a trade to be considered.
- **PROFIT_TARGET:** The profit percentage target for triggering limit sell orders.
- **PREDICT_BASED_ORDERS:** Boolean toggle for using predicted price orders.
- **LENGTH_BAR:** The duration between trade cycles, influencing the progress bar timing.

Ensure you customize these values in the configuration file to suit your trading strategy before running the bot.

---

## Usage

To start the trading bot, run the main script:

The bot will:
- Synchronize the local time with the Binance server.
- Load active positions from the database.
- Fetch real-time market data and perform analysis.
- Generate trade signals based on the analyzed data.
- Place buy orders when market conditions are favorable and sell orders based on profit targets, technical conditions, or stop-loss triggers.
- Log all transactions and updates in real-time.

The console outputs provide a detailed status report including market data, portfolio distribution, EMA rejection signals, and a summary of recent trades.

---

## Logging and Database

- **CSV Logging:** All trades are recorded in a CSV file (`trade_log.csv`), capturing details like timestamp, trade type, quantity, price, and profit/loss.
- **Database Management:** The bot uses a `TradePositionManager` to store open positions and update them when trades are executed or closed. This persists historical trade data for performance analysis.

Ensure the logging paths and database connections are correctly configured in the project.

---

## Error Handling and Risk Management

The bot implements robust error handling routines including:
- Retry mechanisms for fetching market data.
- Handling Binance API exceptions and synchronizing time if discrepancies are detected.
- Dynamic calculation of stop losses and trailing stops.
- Trade validation before order execution based on account balance and risk parameters.

These features help ensure that the bot can adapt to volatile market conditions and execute trades without manual intervention.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## Contributing

Contributions and improvements are welcome. Please open an issue or submit a pull request for any enhancements or bug fixes.

---

## Disclaimer

**WARNING:** Trading cryptocurrency involves significant risk. The trading bot is provided "as is" without any warranties. Always test with small amounts and on testnet environments before trading with significant capital. Use at your own risk.



