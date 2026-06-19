# Cross-Sectional Alpha Strategy

A quantitative momentum based equity strategy that ranks U.S. large cap stocks cross sectionally each month and constructs long-only, long-short, and overlapping portfolios. The pipeline spans data ingestion, return computation, signal construction, and full historical backtesting with transaction costs.

## Strategy Overview

The core idea is **cross-sectional momentum (relative strength)**:
- **Lookback**: Compound trailing 12-month returns, skipping the most recent month (to avoid short-term reversal).
- **Ranking**: Each month, rank all 20 equities by momentum score.
- **Portfolio Construction**: Go long the top quintile (top 20%) and optionally short the bottom quintile.
- **Holding Period**: 1-month or 3-month overlapping variants.
- **Transaction Costs**: 5 bps per side friction model applied to measure net returns.

## Universe

20 diversified U.S. large cap tickers spanning Technology, Consumer Discretionary, Healthcare, Energy, and Financials:

> AAPL, MSFT, INTC, IBM, ORCL, AMZN, HD, MCD, DIS, KO, PG, WMT, JNJ, PFE, MRK, XOM, CVX, JPM, BAC, GE

Benchmark: **SPY**  
Period: **Jan 2000 – Dec 2024** (25 years)

## Project Structure

```
cross-sectional-alpha-strategy/
├── notebooks/
│   ├── 1_fetch_historical_data.ipynb   # Pull adj. close prices via yfinance
│   ├── 2_return_aggregations.ipynb     # Monthly log returns & EDA
│   ├── 3_alpha_momentum_signal.ipynb   # Momentum signal & allocation weights
│   └── 4_strategy_simulation.ipynb     # Backtest, benchmarking & risk metrics
├── data/
│   ├── raw/
│   │   └── market_data_20.csv          # Daily adjusted close prices
│   └── processed/
│       ├── monthly_returns_20.csv      # Monthly log return matrix
│       ├── alpha_signal_matrix.csv     # Cross-sectional momentum scores
│       ├── allocations_long_only.csv   # Long-only portfolio weights
│       ├── allocations_market_neutral.csv  # Long-short (market-neutral) weights
│       └── allocations_overlapping.csv # 3-month overlapping weights
├── figures/                            # All generated plots
└── .gitignore
```

## Pipeline (Notebook Sequence)

| Step | Notebook | Description |
|------|----------|-------------|
| 1 | `1_fetch_historical_data` | Downloads 25 years of daily adj. close prices for 20 stocks + SPY via `yfinance` and saves to `data/raw/`. |
| 2 | `2_return_aggregations` | Resamples to month-end, computes log returns, and runs EDA (distributions, correlations, cumulative growth). |
| 3 | `3_alpha_momentum_signal` | Builds the trailing 12-month (skip-1) momentum indicator, ranks stocks cross-sectionally, and produces allocation weight matrices for three portfolio variants. |
| 4 | `4_strategy_simulation` | Applies a 1-period lag to weights, computes realised portfolio returns, benchmarks vs SPY, models transaction costs, and reports Sharpe ratios, drawdowns, and rolling performance. |

## Key Figures

| Figure | Content |
|--------|---------|
| `fig1_price_history` | 25-year price trajectories |
| `fig2_*` | Cumulative returns, return distributions, asset correlations |
| `fig3_*` | Momentum signal heatmap, portfolio exposure, win/loss frequency |
| `fig4_*` | Portfolio growth, rolling Sharpe, drawdowns, turnover, friction impact |

## Tech Stack

- **Python** (pandas, numpy, matplotlib, seaborn)
- **yfinance** for market data
- **Jupyter Notebooks** for reproducible analysis
