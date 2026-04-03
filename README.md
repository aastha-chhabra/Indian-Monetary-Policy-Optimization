# Optimizing Indian Monetary Policy Using Metaheuristic Algorithms

Analysed RBI macroeconomic data (2010–2025) with four metaheuristic optimisation algorithms to derive stable, evidence-backed monetary policy parameters.

---

## Overview

This project formulates the RBI monetary-policy setting problem as a **constrained multi-objective optimisation task** and solves it using four bio-inspired algorithms — **GA, PSO, SA, and ACO** — to find the combination of Repo Rate, CRR, and SLR that best balances GDP growth, inflation control, and liquidity management.

The fitness function weights: **60 % GDP · 30 % Inflation · 10 % Liquidity**

---

## Key Results

| Rank | Algorithm | Fitness Score | Repo Rate | CRR | SLR | Time (s) |
|------|-----------|--------------|-----------|-----|-----|----------|
| 1 | **PSO** | **97.9697** | 5.9970 % | 5.7000 % | 18.0000 % | **0.13** |
| 2 | ACO | 97.9553 | 6.0000 % | 5.6667 % | 18.0000 % | 0.47 |
| 3 | GA | 97.9542 | 5.9981 % | 5.5010 % | 18.4973 % | 0.71 |
| 4 | SA | 97.9510 | 6.0152 % | 5.6491 % | 18.0572 % | 0.23 |

**PSO is the most efficient method** — best fitness score at the lowest execution time (0.13 s vs 0.71 s for GA).

### Stable Optimal Parameters

| Parameter | Optimal Value |
|---|---|
| Policy Repo Rate | **~6.0 %** |
| Cash Reserve Ratio (CRR) | **~5.7 %** |
| Statutory Liquidity Ratio (SLR) | **~18 %** |

### Multi-seed Validation (PSO, 5 seeds)

| Seed | Fitness | Repo Rate | CRR | SLR |
|------|---------|-----------|-----|-----|
| 0 | 97.9697 | 5.9970 % | 5.7000 % | 18.0000 % |
| 7 | 97.9697 | 5.9969 % | 5.7000 % | 18.0000 % |
| 13 | 97.9697 | 5.9969 % | 5.7000 % | 18.0000 % |
| 21 | 97.9697 | 5.9970 % | 5.7000 % | 18.0000 % |
| 42 | 97.9697 | 5.9970 % | 5.7000 % | 18.0000 % |

**Fitness std = 0.000000** — fully reproducible and stable across all seeds.

---

## Top GDP Correlations

| Feature | Pearson r |
|---|---|
| Exchange Rate of Indian Rupee (month end) | +0.8922 |
| Statutory Liquidity Ratio | −0.8503 |
| Reverse Repo Rate | −0.7858 |
| M3 (in crore) | +0.7283 |
| Consumer Price Index for Industrial Workers | −0.6272 |

---

## Repository Structure

```
.
├── monetary_policy_optimization.py   # Main source — all 4 algorithms
├── final_data.xlsx                   # RBI macroeconomic dataset (2010–2025)
├── README.md
└── requirements.txt
```

---

## Dataset

`final_data.xlsx` — 184 monthly observations (Jan 2010 – Dec 2025) sourced from RBI publications.

| Column | Description |
|---|---|
| Year - Month | Monthly timestamp (YYYY-MM format) |
| Crude Oil Price | Global crude oil price (USD per barrel) |
| Exchange Rate of Indian Rupee (month end) | INR/USD exchange rate at month-end |
| Non-food Credit | Bank credit excluding food credit; proxy for economic activity |
| M3 (in crore) | Broad money supply (₹ crore) |
| Cash Reserve Ratio | Percentage of deposits banks must hold with RBI (%) |
| Statutory Liquidity Ratio | Percentage of deposits banks must maintain in liquid assets (%) |
| Policy Repo Rate | RBI benchmark policy lending rate (%) |
| Reverse Repo Rate | RBI rate for absorbing liquidity from banks (%) |
| Bank Rate | Long-term RBI lending rate (%) |
| Consumer Price Index for Industrial Workers | CPI-IW measuring retail inflation for industrial workers |
| Wholesale Price Index Inflation | Inflation based on wholesale prices (%) |
| Index of Industrial Production Growth rate | Growth rate of industrial output (%) |
| Primary Articles (inflation) | Inflation rate of primary goods (%) |
| Fuel and Power (inflation) | Inflation rate of fuel and energy components (%) |
| Manufactured Products (Inflation) | Inflation rate of manufactured goods (%) |
| CPI inflation (Base = 2012) | Consumer Price Index inflation (%) with base year 2012 |
| Interpolated GDP | Quarterly GDP converted to monthly estimates (₹ Billion) |

---

## Methodology

### Decision Variables and Constraints

| Variable | Lower Bound | Upper Bound |
|---|---|---|
| Policy Repo Rate | 4.0 % | 8.5 % |
| CRR | 3.0 % | 6.0 % |
| SLR | 18.0 % | 24.0 % |

### Objective Function

```
Fitness = 0.60 × GDP_score
        + 0.30 × Inflation_control
        + 0.10 × Liquidity_score
```

- **GDP score** — penalises high policy rates and reserve ratios (contractionary effects).
- **Inflation control** — Gaussian reward centred at 6 % repo rate, consistent with RBI's 4 % medium-term inflation target band.
- **Liquidity score** — penalises deviations from historical norms (CRR = 4.5 %, SLR = 21 %).

### Algorithm Hyperparameters

| Algorithm | Key Hyperparameters |
|---|---|
| GA | population=50, generations=100, mutation_rate=0.15, crossover_rate=0.8 |
| PSO | particles=40, iterations=100, w=0.7, c1=c2=1.5 |
| SA | T₀=100, cooling=0.95, steps_per_T=50, T_min=0.01 |
| ACO | ants=30, iterations=100, evaporation=0.1, q=10, bins=10 |

---

## Installation & Usage

```bash
pip install -r requirements.txt
python monetary_policy_optimization.py
```

The script will:
1. Load `final_data.xlsx` from the current directory
2. Compute and print GDP correlations
3. Run all four algorithms and print per-algorithm results
4. Display a ranked results summary
5. Run a 5-seed stability validation on PSO
6. Save four charts to the current directory

---

## Output Charts

| File | Description |
|---|---|
| `speed_vs_quality.png` | Scatter — fitness vs execution time |

---

## Conclusions

1. **PSO** delivered the best solution quality (97.9697) in the shortest time (0.13 s), making it the most practical algorithm for this problem class.
2. All four algorithms independently converged to a **repo rate of ~6.0 %**, consistent with RBI's historical stance and its 4 % CPI target corridor.
3. Multi-seed validation confirms **zero variance** in PSO solutions — the optimal point is a global attractor for this fitness landscape.
4. The derived parameters (Repo ≈ 6 %, CRR ≈ 5.7 %, SLR ≈ 18 %) align closely with RBI's actual policy settings over the study period, lending empirical credibility to the framework.
