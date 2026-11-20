<img align="right" height="75px" src="../images/image.png">

# 🌲 Pine Script to Python transpiler

> TradingView v4 / v5 / v6 to PyneCore

**pine2pyne** converts TradingView **Pine Script** indicators, strategies and libraries into **Python** scripts for the open-source [PyneCore](https://pynecore.org) runtime, so you can **backtest TradingView strategies offline**, on your own candles, without a TradingView account, a chart, an API key or a compile quota. It is a free, open-source (Apache-2.0) **Pine Script to Python converter** that runs on your machine: a real compiler front end for Pine Script v4, v5 and v6, not a regex translator and not an online service.

## What it does

- **Pine Script → Python transpiler / compiler**: parses the full Pine Script language (v4, v5, v6), checks it the way the TradingView compiler does, and writes a PyneCore module.
- **Indicators, strategies and libraries**: `indicator()`, `strategy()`, `study()` (v4), `library()` with `export`, `import user/Library/N as alias`.
- **Pine semantics preserved**: bar-by-bar execution, series history (`close[1]`, `x[n]`), `var` / `varip` persistence, `na` handling, strict `and`/`or` of v4/v5, `if`/`switch`/`for`/`while` as values, user-defined types (`type`), `enum`, `method`, arrays, matrices, maps, tuples.
- **Strategy orders**: `strategy.entry`, `strategy.exit`, `strategy.close`, `strategy.order`, `strategy.cancel`, position sizing and the strategy settings become a PyneCore strategy with trades and statistics.
- **Technical analysis built-ins**: `ta.sma`, `ta.ema`, `ta.rsi`, `ta.macd`, `ta.atr`, `ta.crossover`, `ta.pivothigh`, `ta.valuewhen`, `ta.barssince` and the rest of `ta.*`, `math.*`, `str.*`, `array.*`, `matrix.*`, `map.*`, `request.security` (multi-timeframe), `input.*`, `plot`, `plotshape`, `plotchar`, `bgcolor`, `fill`, `label`, `line`, `box`, `table`, `polyline`, `alertcondition`.
- **Inputs become parameters**: every `input.*()` is a parameter of the generated `main()`, so a strategy can be parameterised and optimised from Python.
- **TradingView-grade error reporting**: what the TradingView compiler refuses is refused here too, with the Pine line and column; what it compiles with a caveat is converted with a warning.
- **No cloud, no key, no quota, no network**: `pip install pine2pyne` and convert thousands of scripts in CI or in bulk.

## Install and convert

```bash
pip install pine2pyne                       # Python 3.11+
pine2pyne strategy.pine -o strategy.py      # one file
pine2pyne "scripts/*.pine" -o out/          # a folder of Pine scripts
pine2pyne strategy.pine --validate          # check without writing
```

## Run the result (backtest in Python)

The output is an ordinary PyneCore script:

```bash
pip install "pynesys-pynecore[cli,ccxt]"
pyne data download ccxt:BINANCE:BTC/USDT@60 --from 2025-01-01
pyne run strategy ccxt_BINANCE_BTC_USDT_60
```

You get CSV files with every `plot()` value per bar, and for a strategy the list of trades and the statistics (net profit, drawdown, profit factor, win rate). Any market CCXT or a CSV file gives you works: crypto, forex, stocks, futures, including exchanges TradingView does not list.

## Supported Pine Script versions

| Version | Status |
|---|---|
| Pine Script v6 | full language, including `method`, `enum`, `type`, `switch`, `for ... in`, typed collections |
| Pine Script v5 | full language; `var`, strict `and`/`or`, three-state `bool`, `switch` values |
| Pine Script v4 | `study()`, unprefixed functions (`sma`, `rsi`, `security`), `transp`, `iff`, `input(..., type=)` |

## Limitations

- Libraries are not downloaded from TradingView: convert the library's source yourself to `scripts/lib/<Publisher>/<Name>/v<N>.py`.
- `request.security` needs candle data for every symbol and timeframe the script requests.
- The transpiler reproduces Pine semantics as faithfully as it can, but it does not run your script on TradingView: compare a few bars of your own output before trusting a backtest.

## FAQ

**Can I run Pine Script outside TradingView?** Yes: convert it with pine2pyne and run the Python on PyneCore, locally or on a server.

**Is this a Pine Script interpreter for Python?** No. It is a transpiler: the Pine source is compiled once into readable Python that PyneCore executes bar by bar.

**Does it convert TradingView strategies to Python for backtesting?** Yes, including orders, position sizing and strategy settings; the backtest writes trades and statistics to CSV.

**Which TradingView indicator functions are supported?** The `ta.*` library and the drawing objects listed above; the generated code calls the PyneCore implementations of the same functions.

**Is it free?** Yes, Apache-2.0, open source, no account and no API key.

Links: [PyPI](https://pypi.org/project/pine2pyne/) · [PyneCore runtime](https://pynecore.org) · [PyneCore documentation](https://pynecore.org/docs)

<!--# Bogdanoff Twin

> The call has been made. 📞 The subscription is being dumped. 📉

## Buy the dip

```bash
pip install pine2pyne
```

## Dump it

```bash
pine2pyne strategy.pine -o strategy.py
```

## Consider this your TradingView Black Friday sale

> Every day is Black Friday now. Your strategy runs on your machine, and nobody calls you about Premium.

- **Discount:** 100%. Not 60%, not "up to 70%", one hundred.
- **Duration:** forever. The sale ends when the candles stop printing.
- **Fine print:** none. No account, no API key, no quota, no network.
- **What you get:** your Pine Script indicators, strategies and libraries (v4, v5, v6) as Python for [PyneCore](https://pynecore.org), backtesting offline on any candles you own.-->
