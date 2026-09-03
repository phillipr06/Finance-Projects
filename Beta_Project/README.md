# CAPM & Beta Calculator

Calculates a stock's beta, CAPM-implied expected return, and Jensen's alpha
against a market benchmark, using live daily price data. Built as a
standalone tool so the same beta/CAPM logic can be reused elsewhere — e.g.
to feed the cost-of-equity assumption in a DCF, or to check how a stock in
a portfolio has performed relative to what its risk level would predict.

## Inputs

| Input | Value |
|---|---|
| Stock | RR.L (Rolls-Royce Holdings) |
| Benchmark | ^FTSE (FTSE 100) |
| Period | 5 years, daily |
| Risk-free rate | UK 10-year gilt yield, manually entered from [Trading Economics](https://tradingeconomics.com/united-kingdom/government-bond-yield) |

## Methodology

1. **Data** — 5 years of daily closing prices for the stock and benchmark,
   pulled live via [`yfinance`](https://github.com/ranaroussi/yfinance);
   daily % returns computed for both, then explicitly aligned to their
   common trading dates via `index.intersection()`.
2. **Beta** — covariance of the stock's daily returns with the market's,
   divided by the market's variance. Beta > 1 means the stock is more
   volatile than the market; beta < 1 means less.
3. **CAPM expected return** — risk-free rate + beta × (market return −
   risk-free rate), using the annualized market return over the same window.
4. **Jensen's alpha** — the stock's actual annualized return minus its
   CAPM-expected return: the return earned above or below what its risk
   level alone would predict.

## Requirements

```
pandas
matplotlib
yfinance
```

Install with:
```
pip install pandas matplotlib yfinance
```

## Running it

Open `CAPM_Beta.ipynb` and run all cells top to bottom. To analyze a
different stock or benchmark, change `Stockticker` and the `^FTSE` argument
near the top, and update `Rfr` with a current risk-free rate before
re-running — it's a manual input, not pulled live.

## Results (as of this run)

- **Risk-free rate:** 5.16%
- **Market (FTSE 100) annual return:** 8.68%
- **Beta:** 1.63
- **CAPM expected return:** 10.88%
- **Stock annual return:** 67.68%
- **Alpha:** 56.79%

RR's beta of 1.63 means it moved roughly 1.6x as much as the FTSE 100 over
this window. The large alpha reflects Rolls-Royce's exceptional post-COVID
share price recovery over this specific period — a real result of the
chosen stock and window, not a typical alpha figure or a calculation error.

## Notes

- All price data is pulled live at run time, so results will differ on
  re-run and from what's shown here.
- This is a learning/portfolio project, not investment advice.
