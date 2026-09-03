# Portfolio Analysis

A Python/pandas backtest of a 4-stock equity portfolio, built to practice the
core building blocks of quantitative portfolio analysis: return calculation,
FX conversion, risk/volatility measurement, correlation analysis, and
risk-adjusted performance (Sharpe ratio).

## Holdings

| Ticker | Company | Exchange | Weight |
|---|---|---|---|
| RR.L | Rolls-Royce Holdings | LSE | 40% |
| BA.L | BAE Systems | LSE | 30% |
| GE | General Electric | NYSE | 20% |
| SAF.PA | Safran | Euronext Paris | 10% |

Entry date: **3 Feb 2022**. Initial investment: **£100,000**.

## Methodology

1. **Data** — 5 years of daily OHLC price history for each stock, plus
   GBP/USD and GBP/EUR FX rates, pulled live via [`yfinance`](https://github.com/ranaroussi/yfinance).
2. **Currency conversion** — GE (USD) and SAF (EUR) prices are converted to
   GBP so all four holdings sit on a common base.
3. **Calendar alignment** — the four exchanges don't share the same holiday
   calendar, so trading dates are trimmed to the intersection common to all
   four before any portfolio-level math is done.
4. **Returns** — daily % returns are compounded into cumulative and
   annualized (252 trading-day) returns per stock and for the weighted
   portfolio.
5. **Risk** — annualized portfolio volatility is derived from the covariance
   matrix of the four return series and the portfolio weights; a
   correlation matrix shows how much diversification benefit the holdings
   actually provide.
6. **Risk-adjusted return** — Sharpe ratio, using the current UK 10-year
   gilt yield (pulled live via `yfinance`) as the risk-free rate proxy.

## Requirements

```
pandas
numpy
matplotlib
yfinance
```

Install with:
```
pip install pandas numpy matplotlib yfinance
```

## Running it

Open `Portfolio_Analysis.ipynb` and run all cells top to bottom. All data is
pulled live, so:
- results will differ from run to run and from what's shown in the committed
  notebook output,
- the entry date, weights, and tickers at the top of the notebook can be
  changed to test other portfolios.

## Results (as of this run)

- **Gilt yield (risk-free rate):** 4.77%
- **Total portfolio return (entry date → now):** ~632%
- **Sharpe ratio:** 1.6

The large total return is driven substantially by Rolls-Royce's post-COVID
share price recovery — RR is the largest single weight (40%) in the
portfolio and had an exceptional run over this specific window. This is a
genuine result of the chosen entry date and weights, not a bug, but it's
worth reading with that context rather than as a "typical" 5-year outcome.

## Notes

- Some plotting/debugging assistance was AI-assisted (noted inline in the
  notebook).
- This is a learning/portfolio project, not investment advice.
