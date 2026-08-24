# crypto-residual-momentum
Cross-sectional residual momentum strategy across cryptocurrencies with strict out-of-sample validation.

# Crypto Residual Momentum Strategy

A cross-sectional statistical arbitrage strategy that tests whether cryptocurrency momentum persists after removing common crypto-market exposure.

## Strategy Overview

Cryptocurrencies exhibit strong common market movements. This project removes the common market component using rolling market betas and constructs momentum signals from residual returns.

The strategy ranks eight cryptocurrencies cross-sectionally and forms a market-neutral long-short portfolio.

## Methodology

1. Construct crypto market factor
2. Estimate 30-day rolling market betas
3. Compute residual returns
4. Construct residual momentum signals
5. Test 12 formation/holding-period combinations using Spearman IC
6. Select 14-day formation / 7-day holding period
7. Long top 2 / short bottom 2 cryptocurrencies
8. Apply turnover-based transaction costs
9. Lock parameters using 2020–2023 data
10. Evaluate on untouched 2024–2025 OOS data

## Results

| Metric | Development | OOS |
|---|---:|---:|
| Annualised Return | 40.4% | 41.6% |
| Annualised Volatility | 38.0% | 29.0% |
| Sharpe Ratio | 1.06 | 1.44 |
| Maximum Drawdown | 23.9% | 19.9% |

Year-by-year OOS Sharpe:

- 2024: 1.77
- 2025: 0.98

## Statistical Validation

Newey-West / HAC test on weekly OOS net returns:

- t-statistic: ~1.90
- p-value: ~0.057

The evidence is therefore borderline at the conventional 5% significance level and should not be interpreted as strong statistical significance.

## Research Design

All model selection and parameter tuning were performed using development data only.

2024–2025 data were kept untouched until the final strategy specification had been locked.

## Repository Structure

- `01_EDA.ipynb` – exploratory analysis
- `02_signal_search.ipynb` – residualisation and signal selection
- `03_portfolio_construction.ipynb` – portfolio rules and transaction costs
- `04_out_of_sample.ipynb` – OOS evaluation
- `05_validation.ipynb` – robustness and statistical tests

## Disclaimer

This project is for research and educational purposes only and does not constitute investment advice.
