# Bayesian Hierarchical Modeling of the Asthma–Smoking Association (CDC, 2011–2021)

## Overview
This project analyzes adult asthma prevalence across **50 U.S. states + DC (2011–2021)** and answers:
1) **Which states show statistically significant linear asthma trends over time** (with multiple-testing correction)?
2) **How do state baseline asthma prevalences differ after partial pooling**, and what is the **population-level association** between smoking prevalence and asthma prevalence?

We combine per-state OLS + multiple hypothesis testing with a **Bayesian hierarchical (partial pooling) model in PyMC** to produce stable state-level estimates and quantify uncertainty.

## Key Findings (Results Snapshot)
- **Multiple testing (RQ1):** Significant increasing asthma trends were limited to a small subset of states after correction:
  - Bonferroni: **Colorado, Kansas, South Carolina**
  - Benjamini–Hochberg: **Colorado, Kansas, Mississippi, South Carolina, Tennessee, West Virginia**
  - Benjamini–Yekutieli: **Colorado, Kansas**
- **Partial pooling (RQ2):** Baseline asthma prevalence varies meaningfully by state after shrinkage:
  - Lowest posterior medians: **Texas, South Dakota, Florida, Nebraska, Minnesota (≈ 7.5%–8.1%)**
  - Highest posterior medians: **New Hampshire, West Virginia, Vermont, Rhode Island, Maine (≈ 11.3%–11.8%)**
  - **Spread (Maine − Texas): ~4.3 percentage points**
- **Smoking effect (RQ2):** The posterior for the global smoking coefficient **includes 0** (uncertain direction).
  - Main model: β median ≈ **−0.094**, 95% HDI **[−0.84, 0.72]**
  - Sensitivity analysis with a more data-informed prior yields the same qualitative conclusion (credible interval still includes 0).

> Interpretation note: this is **ecological (state-year) analysis** and does not imply individual-level causality.

## Methods
### RQ1 — State-level trend detection (Frequentist)
- Fit **OLS per state**: asthma_prevalence ~ year
- Use two-sided p-values for the **year slope**
- Correct for multiple testing across **51 hypotheses** using:
  - **Bonferroni (FWER)**
  - **Benjamini–Hochberg (FDR, independent tests)**
  - **Benjamini–Yekutieli (FDR, dependent tests)**
- Power estimated per state via **Monte Carlo simulation** (small n ≈ 11 yearly observations).

### RQ2 — Bayesian hierarchical model (Partial Pooling, PyMC)
Model on the logit scale:
- State baselines (α_s) with hierarchical prior (partial pooling)
- Year effects (δ_t)
- Global smoking effect (β)
- Posterior inference via MCMC (PyMC), with convergence checks and tuning (e.g., target_accept adjustments to avoid divergences)

## Data
The raw dataset is not stored in this repo (large file).

- Source (Kaggle): **https://www.kaggle.com/datasets/kadiryildirim12/u-s-chronic-disease-indicators-cdi-2023-release**
- Download the CSV from the above source and place the CSV in the repo root (same folder as the notebook)
- Notes: Florida (2021) and New Jersey (2019) are missing in the dataset.

## How to Run
1. Download the dataset and save it as `U.S._Chronic_Disease_Indicators__CDI___2023_Release.csv`
2. Open and run: `cdc_asthma_smoking_bayesian_hierarchical.ipynb`
