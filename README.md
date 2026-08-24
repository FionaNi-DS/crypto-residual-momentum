# Crypto Residual Momentum Strategy

A cross-sectional residual momentum strategy across eight cryptocurrencies, designed and validated using a strict development / out-of-sample framework.

## Overview

This project investigates whether momentum remains predictive after removing common cryptocurrency market exposure.

Rather than trading conventional total-return momentum, the strategy first estimates each asset's exposure to a common crypto-market factor using rolling betas. Momentum signals are then constructed from the residual component of returns.

The final strategy was developed using 2020–2023 data, with all model and portfolio parameters locked before evaluation on untouched 2024–2025 out-of-sample data.

## Key Results

### 2024–2025 Untouched Out-of-Sample Performance

| Metric                |    Result |
| --------------------- | --------: |
| Annualised Return     | **41.6%** |
| Annualised Volatility | **29.0%** |
| Net Sharpe Ratio      |  **1.44** |
| Maximum Drawdown      | **19.9%** |

Performance is reported **after turnover-based transaction costs**.

Year-by-year OOS Sharpe ratios:

* **2024:** 1.77
* **2025:** 0.98

The strategy remained profitable in both OOS years, although performance varied across market regimes.

---

## Research Question

Cryptocurrencies tend to exhibit strong common market movements, meaning conventional momentum signals may partly reflect exposure to the overall crypto market.

This project asks:

> Does a cross-sectional momentum effect remain after removing common crypto-market exposure?

---

## Methodology

### 1. Market Factor and Residualisation

A common crypto-market return factor is constructed and used to estimate asset-specific market exposure.

For each cryptocurrency:

[
r_{i,t} = \alpha_i + \beta_{i,t} r_{m,t} + \epsilon_{i,t}
]

where:

* (r_{i,t}) is the asset return;
* (r_{m,t}) is the crypto-market return;
* (\beta_{i,t}) is estimated using a **30-day rolling window**;
* (\epsilon_{i,t}) is the residual return.

Residual returns are used to isolate asset-specific price movements from common market exposure.

Following residualisation, individual asset correlations with the crypto-market factor fall from approximately **0.66–0.89 to around zero**.

---

### 2. Residual Momentum Signal

Residual returns are accumulated over different formation horizons to construct cross-sectional momentum signals.

A total of **12 formation-period and holding-period combinations** were evaluated.

Signal quality was assessed using **cross-sectional Spearman Information Coefficient (IC)**, measuring whether higher-ranked signals were associated with higher subsequent asset returns.

The selected specification was:

* **Formation period:** 14 days
* **Holding period:** 7 days

---

### 3. Portfolio Construction

At each rebalance date:

* rank eight cryptocurrencies by residual momentum;
* long the strongest two assets;
* short the weakest two assets;
* use equal weights within the long and short legs;
* hold positions for seven days.

The resulting portfolio is a cross-sectional long-short relative-value strategy rather than a directional crypto-market strategy.

---

### 4. Transaction Costs

Trading costs are incorporated using a turnover-based transaction-cost model.

All headline OOS results therefore represent **net performance after transaction costs**, rather than gross backtest returns.

---

## Development and Out-of-Sample Design

A strict separation is maintained between strategy development and final evaluation.

### Development Period

**2020–2023**

Used for:

* signal construction;
* formation-horizon selection;
* holding-period selection;
* portfolio construction rules;
* parameter selection.

### Out-of-Sample Period

**2024–2025**

The final specification was frozen before evaluating this period.

No strategy parameters were re-optimised using OOS results.

This design is intended to reduce data snooping and overfitting risk.

---

## Development vs Out-of-Sample Performance

| Metric                | Development |       OOS |
| --------------------- | ----------: | --------: |
| Annualised Return     |       40.4% | **41.6%** |
| Annualised Volatility |       38.0% | **29.0%** |
| Sharpe Ratio          |        1.06 |  **1.44** |
| Maximum Drawdown      |       23.9% | **19.9%** |

The strategy did not exhibit the typical pattern of strong development performance followed by a large deterioration out of sample.

However, stronger OOS performance should not itself be interpreted as evidence that the strategy will continue to outperform in future periods.

---

## Statistical Validation

To assess whether average OOS returns were distinguishable from zero while accounting for serial dependence, weekly net strategy returns were evaluated using **Newey-West / HAC standard errors**.

Approximate results:

* **t-statistic:** 1.90
* **p-value:** 0.057

The result is close to the conventional 5% significance threshold but does not meet it.

Accordingly, the project does **not** claim strong statistical significance at the 5% level.

This is treated as an important limitation rather than ignored in the interpretation of the results.

---

## Repository Structure

```text
crypto-residual-momentum/
│
├── README.md
├── 01_data.ipynb
├── 02_signal_search.ipynb
├── 03_portfolio_construction.ipynb
├── 04_out_of_sample.ipynb
├── 05_validation.ipynb
└── net_return_oos.csv
```

### Notebooks

**01_data.ipynb**
Data preparation, cleaning and exploratory analysis.

**02_signal_search.ipynb**
Market-factor residualisation, residual momentum construction and signal-horizon selection.

**03_portfolio_construction.ipynb**
Cross-sectional ranking, long-short portfolio construction and transaction-cost modelling.

**04_out_of_sample.ipynb**
Evaluation of the locked strategy on untouched 2024–2025 data.

**05_validation.ipynb**
Development-vs-OOS comparison, year-by-year robustness analysis and Newey-West statistical testing.

---

## Data

The research uses hourly cryptocurrency data covering the development and out-of-sample periods.

The full cleaned hourly dataset is not included in this repository due to file size.

A small file containing OOS net strategy returns is included to support validation and reproducibility of the statistical analysis.

---

## Main Conclusion

The results suggest that conventional cryptocurrency momentum is not entirely explained by common crypto-market exposure.

After removing market beta, a residual cross-sectional momentum effect remains and produces positive net performance during the untouched 2024–2025 evaluation period.

The strategy achieved a **1.44 net OOS Sharpe ratio**, with positive performance in both 2024 and 2025.

However, the statistical evidence is borderline rather than conclusive, and the results should therefore be interpreted as evidence of a potentially robust signal rather than proof of persistent future profitability.

---

## Tools

* Python
* pandas
* NumPy
* SciPy
* statsmodels
* Matplotlib
* Jupyter Notebook

---

## Disclaimer

This repository is a quantitative research project created for educational and portfolio purposes.

It does not constitute investment advice, and historical backtest performance does not guarantee future results.
