# Technical Analysis Automation

A Python-based engine for **automated detection and backtesting of classical chart patterns** in financial markets. Built to systematically evaluate whether traditional technical analysis patterns (Head & Shoulders, Harmonic Patterns, Flags, Pennants, etc.) hold statistical predictive power when applied algorithmically to cryptocurrency market data.

> **Asset Covered:** BTC/USDT (Bitcoin)  
> **Timeframes:** Hourly (3600s) and Daily (86400s) OHLC candlestick data  
> **Data Range:** 2018 – Present

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Modules](#modules)
  - [Price Extreme Detection](#1-price-extreme-detection)
  - [Trendline Fitting](#2-trendline-fitting)
  - [Pattern Recognition](#3-pattern-recognition)
  - [Support & Resistance](#4-support--resistance-analysis)
  - [ML Pattern Mining](#5-ml-based-pattern-mining)
  - [Backtesting & Analysis](#6-backtesting--statistical-analysis)
- [Sample Output](#sample-output)
- [Configuration](#configuration)
- [Tech Stack](#tech-stack)
- [License](#license)

---

## Overview

Traditional technical analysis relies on traders visually identifying chart patterns — a subjective and inconsistent process. This project replaces that with **algorithmic pattern detection**, applying mathematical rules to identify patterns and then **backtesting trade signals** against historical data to measure profitability.

The system implements three independent methods for detecting price turning points, multiple pattern recognition algorithms, and a machine learning pipeline that discovers tradeable patterns without prior assumptions about their shape.

---

## Key Features

- **Multi-method extreme detection** — Rolling window, directional change, and perceptually important points (PIPs) algorithms for identifying tops and bottoms
- **Automated trendline fitting** — Numerical optimization for support/resistance trendlines with least-squared error minimization
- **Classical pattern recognition** — Head & Shoulders (regular + inverted), Flags, Pennants (bull + bear variants)
- **Harmonic pattern detection** — Gartley, Bat, Butterfly, Crab, Deep Crab, Cypher, and Shark with Fibonacci ratio validation
- **Market profile support/resistance** — Kernel density estimation to find statistically significant price levels
- **ML pattern clustering** — K-Means clustering of normalized price shapes with automated cluster selection via silhouette analysis
- **Walk-forward validation** — Out-of-sample testing framework to guard against overfitting
- **Monte Carlo permutation testing** — Statistical significance testing for mined pattern performance
- **Comprehensive backtesting** — Win rate, average return, profit factor, and total return analysis across parameter sweeps

---

## Project Structure

```
TechnicalAnalysisAutomation/
│
├── Core Detection Modules
│   ├── rolling_window.py           # Local top/bottom detection via rolling window
│   ├── perceptually_important.py   # Perceptually Important Points (PIP) extraction
│   ├── directional_change.py       # Sigma-based directional change detection
│   └── trendline_automation.py     # Automated trendline fitting with slope optimization
│
├── Pattern Recognition
│   ├── head_shoulders.py           # Head & Shoulders / Inverse H&S detection
│   ├── flags_pennants.py           # Bull/Bear Flag and Pennant detection
│   └── harmonic_patterns.py        # XABCD Harmonic pattern detection (7 patterns)
│
├── Advanced Analysis
│   ├── mp_support_resist.py        # Market profile-based support/resistance levels
│   ├── retracement_ratios.py       # Fibonacci retracement ratio distribution analysis
│   ├── pip_pattern_miner.py        # ML-based pattern mining with K-Means clustering
│   └── wf_pip_miner.py            # Walk-forward wrapper for pattern miner
│
├── Backtesting & Evaluation
│   ├── test_hs_patterns.py         # H&S parameter sweep and performance analysis
│   └── test_flag_patterns.py       # Flag/Pennant parameter sweep and analysis
│
├── Data
│   ├── BTCUSDT3600.csv             # BTC/USDT hourly OHLC data
│   └── BTCUSDT86400.csv           # BTC/USDT daily OHLC data
│
├── LICENSE
└── README.md
```

---

## Architecture

```
                    ┌──────────────────────────────────────┐
                    │          Raw OHLC Price Data          │
                    │       (Hourly / Daily BTC-USDT)       │
                    └──────────────┬───────────────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              ▼                    ▼                     ▼
     ┌────────────────┐  ┌─────────────────┐  ┌──────────────────┐
     │ Rolling Window  │  │  Directional    │  │  Perceptually    │
     │ (Local Extrema) │  │  Change (σ%)    │  │  Important Pts   │
     └───────┬────────┘  └───────┬─────────┘  └────────┬─────────┘
             │                   │                      │
             ▼                   ▼                      ▼
     ┌───────────────────────────────────────────────────────────┐
     │                   Detected Extremes                       │
     │              (Confirmed Tops & Bottoms)                   │
     └────────┬──────────────┬──────────────────┬───────────────┘
              │              │                  │
              ▼              ▼                  ▼
     ┌──────────────┐ ┌────────────┐  ┌──────────────────┐
     │ Head &       │ │  Flags &   │  │  Harmonic XABCD  │
     │ Shoulders    │ │  Pennants  │  │  (7 Patterns)    │
     └──────┬───────┘ └─────┬──────┘  └────────┬─────────┘
            │               │                   │
            ▼               ▼                   ▼
     ┌───────────────────────────────────────────────────────────┐
     │              Backtesting & Performance Analysis            │
     │   (Win Rate, Avg Return, Profit Factor, Parameter Sweep)  │
     └───────────────────────────────────────────────────────────┘
```

---

## Installation

### Prerequisites

- Python 3.8+
- pip

### Setup

```bash
git clone https://github.com/abubakar668/TechnicalAnalysisAutomation.git
cd TechnicalAnalysisAutomation
```

Install dependencies:

```bash
pip install pandas numpy matplotlib mplfinance scipy pandas_ta
```

For the ML pattern mining module (optional):

```bash
pip install pyclustering
```

> **Note:** `pyclustering` requires `numpy < 2.0`. If you encounter compatibility issues:
> ```bash
> pip install "numpy<2"
> ```

---

## Usage

All scripts are run from the project root directory.

### Quick Start — Visualize Price Extremes

```bash
# Detect and plot local tops/bottoms on BTC daily chart
python rolling_window.py

# Extract key turning points from a price window
python perceptually_important.py

# Detect trend reversals using 2% directional change
python directional_change.py
```

### Pattern Detection

```bash
# Find Head & Shoulders patterns and plot with candlestick charts
python head_shoulders.py

# Detect Flag and Pennant continuation patterns
python flags_pennants.py

# Run Harmonic pattern detection (Gartley, Bat, Butterfly, etc.)
python harmonic_patterns.py
```

### Backtesting & Research

```bash
# Sweep H&S parameters (order 1-48) and plot performance metrics
python test_hs_patterns.py

# Sweep Flag/Pennant parameters and evaluate profitability
python test_flag_patterns.py

# Analyze Fibonacci retracement ratio distributions
python retracement_ratios.py
```

### ML Pattern Mining

```bash
# Cluster price shapes and find tradeable pattern groups
python pip_pattern_miner.py

# Walk-forward out-of-sample pattern mining
python wf_pip_miner.py
```

---

## Modules

### 1. Price Extreme Detection

Three independent algorithms for identifying market turning points:

| Module | Method | Key Parameter | Use Case |
|---|---|---|---|
| `rolling_window.py` | Compares price to neighbors within a window | `order` (window half-width) | General-purpose peak/valley detection |
| `directional_change.py` | Confirms extremes after σ% price retracement | `sigma` (retracement threshold) | Trend reversal detection |
| `perceptually_important.py` | Iteratively selects points maximizing distance from the line connecting adjacent points | `n_pips` (number of points) | Price series simplification |

### 2. Trendline Fitting

`trendline_automation.py` — Automatically fits optimal support and resistance lines using a two-step process:

1. Finds initial pivot points from the line of best fit
2. Optimizes slope via numerical differentiation to minimize squared error while keeping the line valid (support below all prices, resistance above)

Supports both single-series and OHLC (high/low/close) trendline fitting.

### 3. Pattern Recognition

**Head & Shoulders** (`head_shoulders.py`)
- Detects both regular (bearish) and inverted (bullish) H&S patterns
- Validates via balance rules, symmetry constraints, and neckline break confirmation
- Computes pattern R² (goodness of fit) and expected returns using stop/TP rules

**Flags & Pennants** (`flags_pennants.py`)
- Two detection methods: PIP-based and trendline-based
- Validates pole/flag proportions (width and height ratios)
- Confirms patterns on trendline breakout
- Classifies as flag (parallel lines) vs pennant (converging lines)

**Harmonic Patterns** (`harmonic_patterns.py`)
- Detects 7 XABCD patterns: Gartley, Bat, Butterfly, Crab, Deep Crab, Cypher, Shark
- Uses log-ratio error scoring for Fibonacci ratio matching
- Supports configurable error thresholds and multiple sigma values for robustness

### 4. Support & Resistance Analysis

`mp_support_resist.py` — Market Profile approach using Gaussian kernel density estimation on historical closing prices. Identifies statistically significant price levels and generates breakout signals with a trend-following strategy.

`retracement_ratios.py` — Empirical analysis of retracement ratio distributions to evaluate whether Fibonacci ratios (0.618, 1.618) actually correspond to peaks in real market data.

### 5. ML-Based Pattern Mining

`pip_pattern_miner.py` — An unsupervised learning pipeline that:

1. Extracts all PIP patterns from a rolling lookback window
2. Z-score normalizes patterns for shape-based comparison
3. Determines optimal cluster count via silhouette analysis
4. Clusters patterns using K-Means
5. Assigns clusters as long/short/neutral based on forward returns (Martin ratio)
6. Optionally runs Monte Carlo permutation tests for statistical significance

`wf_pip_miner.py` — Walk-forward wrapper that retrains the miner periodically on expanding windows to generate out-of-sample signals.

### 6. Backtesting & Statistical Analysis

Both `test_hs_patterns.py` and `test_flag_patterns.py` perform comprehensive parameter sweeps and evaluate:

- **Number of patterns found** per parameter setting
- **Average log return** per pattern occurrence
- **Win rate** (percentage of profitable trades)
- **Total cumulative return**
- **Hold period vs stop-loss rule** performance comparison

Results are visualized as multi-panel bar charts for easy comparison.

---

## Sample Output

Each detection module produces matplotlib visualizations:

- **Rolling Window:** BTC price chart with green (top) and red (bottom) markers
- **Head & Shoulders:** Candlestick charts with pattern overlay and neckline drawn
- **Harmonic Patterns:** XABCD structure plotted on candlesticks with Fibonacci ratios annotated
- **Backtests:** 2x2 grid charts showing pattern count, average return, total return, and win rate across parameters

---

## Configuration

Key parameters you can tune across modules:

| Parameter | Where Used | Description | Typical Range |
|---|---|---|---|
| `order` | Rolling window, H&S, Flags | Half-width of the rolling window | 3 – 48 |
| `sigma` | Directional change, Harmonics | Retracement % to confirm reversal | 0.01 – 0.04 |
| `n_pips` | PIPs, Pattern miner | Number of important points to extract | 3 – 7 |
| `lookback` | Trendlines, Support/Resist, Miner | Historical window size | 24 – 365 |
| `err_thresh` | Harmonic patterns | Max acceptable Fibonacci ratio error | 0.1 – 0.75 |
| `hold_period` | Pattern miner | Bars to hold after pattern signal | 6 – 24 |

---

## Tech Stack

- **Python 3.8+**
- **pandas** — Data manipulation and time series handling
- **NumPy** — Numerical computation and array operations
- **Matplotlib** — Charting and visualization
- **mplfinance** — Candlestick chart rendering
- **SciPy** — Kernel density estimation, signal processing
- **pandas_ta** — Technical indicators (ATR)
- **pyclustering** — K-Means clustering and silhouette analysis

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
