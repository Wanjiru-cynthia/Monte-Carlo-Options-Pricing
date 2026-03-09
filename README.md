# Monte Carlo Options Pricing Model

## Overview
European Call Option pricer built in Python using Monte Carlo simulation 
with Geometric Brownian Motion. Validated using Black-Scholes. Inputs derived entirely from real market data.

## Option Parameters
| Parameter | Value | Source |
|-----------|-------|--------|
| Underlying | AAPL (Apple Inc) | — |
| Current Price (S) | $250.83 | yfinance — last closing price Dec 31 2024 |
| Strike Price (K) | $263.37 | Set at 5% OTM |
| Expiration (T) | 1 Year | — |
| Risk-Free Rate (r) | 5.0% | US 1-year Treasury rate |
| Volatility (σ) | 21.37% | Realized — annualized from 2 years of daily returns |
| Simulations (N) | 10,000 | — |

## Methodology

### Data
Two years of real AAPL daily closing prices (2023–2024) sourced via 
yfinance. Volatility is realized — calculated from historical returns 
and annualized by scaling daily standard deviation by √252.

### Simulation
Stock price paths generated via GBM:

**S_T = S × exp((r - 0.5σ²)T + σ√T × Z)**

Z ~ N(0,1), drawn independently across 10,000 simulations.
Random seed fixed at 42 for reproducibility.

### Pricing
Option price estimated as the discounted expected payoff:

**C = e^(-rT) × E[max(S_T - K, 0)]**

### Validation
Result benchmarked against the Black-Scholes closed-form solution:

**C = S × N(d1) - K × e^(-rT) × N(d2)**

Where:
- d1 = [ln(S/K) + (r + 0.5σ²)T] / (σ√T)
- d2 = d1 - σ√T
- N() = cumulative standard normal distribution

## Results
| Method | Option Price |
|--------|-------------|
| Monte Carlo | $21.49 |
| Black-Scholes | $21.48 |
| Difference | $0.01 |

## Interpretation & Recommendations
- **4,587 / 10,000** paths finished in the money — implied 
  probability of profit at expiration: **45.87%**
- Monte Carlo converges to within **$0.01** of the Black-Scholes 
  exact solution at N=10,000 — consistent with expected convergence 
  behavior under the law of large numbers
- Increasing N to 100,000 would reduce the gap further — 
  computational cost scales linearly, accuracy scales with √N
- Realized volatility of **21.37%** is consistent with AAPL's 
  historical range — options are fairly priced relative to 
  recent market conditions
- At **5% OTM**, the $21.49 premium represents **8.57% of the 
  current stock price** — a meaningful hedging cost for any 
  portfolio carrying AAPL as a core position

## Tools
- Python — NumPy — Pandas — yfinance — Matplotlib — SciPy

## Next Steps
- Extend to Put Options — verify Put-Call Parity
- Implement Option Greeks (Delta, Gamma, Vega, Theta)
- Build implied volatility surface across strikes and maturities
- Increase N to 100,000 — demonstrate convergence improvement

## Author
Cynthia Wanjiru | MS Quantitative Finance | Washington University in St Louis
