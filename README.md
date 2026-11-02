# MyseBot

An experimental crypto trading bot and backtester, written in Python. It combines machine learning models, a Q-learning agent, portfolio optimization, and Discord alerts.

> **Archived early project.** I wrote this when I was 16 with my boy wyz back in high school and I'm posting it as-is to show where I started. It is not a profitable strategy and should not be used with real money. See [Known issues](#known-issues) for what I'd do differently now.

(Despite the name, it doesn't use the Zipline library.)

## What it does

- Downloads daily prices for BTC, ETH and XRP (in CAD) with `yfinance`
- Builds technical indicators: RSI, MACD, EMAs, Bollinger Bands, ATR
- Trains an LSTM and a Random Forest on price data
- Trades with a tabular Q-learning agent, with stop-loss and take-profit rules
- Runs a grid search over strategy parameters
- Includes a Sharpe-ratio portfolio optimizer (SciPy, SLSQP)
- Reports total return, win rate, Sharpe ratio, Sortino ratio and max drawdown
- Sends trade alerts and charts to Discord

## Setup

```bash
pip install pandas numpy scipy scikit-learn tensorflow yfinance matplotlib seaborn joblib discord-webhook pyimgur
```

Set your own credentials as environment variables. Never commit real keys.

```bash
export DISCORD_WEBHOOK="your-webhook-url"
export IMGUR_CLIENT_ID="your-imgur-client-id"
```

## Run

```bash
python ZIPLINEBOT.py
```

This downloads data, tunes parameters, trains the models, runs the backtest, prints a performance report, and posts results to Discord. The parameter search is slow.

## Known issues

I've reviewed this code since writing it. Main problems:

- The LSTM and Random Forest are trained but never used to make trading decisions
- Data leakage: `close` is both a feature and the target for the Random Forest, and scalers are fit on all data
- Parameter tuning is scored on the same data it's tuned on (overfitting)
- No transaction fees or slippage in the backtest
- The Q-learning state contains continuous values, so it barely learns
- Duplicate method definitions and some unused code

## What I'd do differently

- Use a walk-forward backtest and report only out-of-sample results
- Fix the leakage and fit scalers on training data only
- Add fees and slippage
- Split the code into modules and add tests
- Keep secrets out of the code

## Disclaimer

For learning only. This is not financial advice.
