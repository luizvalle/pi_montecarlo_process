# Pi Monte Carlo Process

Estimating π using Monte Carlo simulation, with statistical analysis of accuracy and convergence.

## Overview

Random points are sampled uniformly in the unit square [0,1]×[0,1]. A point falls inside the quarter-circle of radius 1 with probability **p = π/4**. By averaging many Bernoulli indicator variables, we estimate π/4, then multiply by 4 to recover π.

```
H_i = 1  if  X_i² + Y_i² ≤ 1,  else 0
π ≈ W_N = 4 · (1/N) Σ H_i
```

## Key Takeaways

### 1. The estimator is unbiased and statistically valid
One-sample t-tests (α = 0.05) across three simulation scales all fail to reject H₀: μ = π/4, confirming the estimator is centered on the true value:

| Sample Size N | T-Statistic | P-Value | Decision |
|---|---|---|---|
| 10⁶ | 0.796 | 0.426 | Fail to reject H₀ |
| 10⁵ | 1.467 | 0.142 | Fail to reject H₀ |
| 10⁴ | 1.220 | 0.223 | Fail to reject H₀ |

### 2. Variance shrinks as 1/N (CLT applies)
By the Central Limit Theorem:

```
W_N ~ N(π, 16·π(1 - π/4) / N)   for large N
```

Larger samples produce tighter distributions around π while the mean stays constant.

### 3. Reaching high precision is expensive
Using the chi-squared sample size formula `N = k·σ²/ε²`, the required number of points grows rapidly:

| Confidence | ε = 0.01 | ε = 0.001 |
|---|---|---|
| 90% | 72,963 | 7,296,219 |
| 95% | 103,596 | 10,359,517 |
| 99% | 178,928 | 17,892,765 |

**A 10× tighter error bound requires 100× more samples.**

### 4. Theory matches simulation
For ε = 0.001 and 90% confidence, the formula predicts N = 7,296,219. Running 10,000 independent simulations at that
sample size, **90.11%** of estimates fell within ±0.001 of π, matching the theoretical guarantee.

## Project Structure

```
notebooks/analysis.ipynb   # All simulations, statistical tests, and visualizations
requirements.txt           # Python dependencies
```

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/analysis.ipynb
```

## Dependencies

- numpy, matplotlib, scipy, pandas, seaborn, jupyter
