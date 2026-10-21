R code for the manuscript *Importance Versus Unique Predictive Contribution of Correlated Clinical Domains: A Gallstone Classification Example* (Tosun & Karaman).

The code compares coefficient-based domain importance with the unique predictive contribution of a domain, U_j = LogLoss(model without domain j) − LogLoss(full model). It uses ridge logistic regression (primary) and XGBoost (sensitivity), out-of-bag bootstrap, and a Monte Carlo simulation with known truth.

## Data

Download the UCI Gallstone dataset (CC BY 4.0) and save it as `gallstone.xlsx`:
https://archive.ics.uci.edu/dataset/1150/gallstone-1

319 patients, 38 features, outcome column `Gallstone Status`.

## How to run

1. Install R and these packages: `readxl`, `glmnet`, `xgboost`, `ggplot2`, `tidyr` (the script installs missing ones).
2. Open the R script and edit `base_dir` at the top to your own folder. Put `gallstone.xlsx` in that folder.
3. At the top of the script there are a few settings that you can set to `TRUE` or `FALSE`. They turn the long analyses on or off (see the table below). Then run the whole script.

| Setting | Meaning |
|---|---|
| `RUN_OLD_HEAVY` | `TRUE` runs the ridge (B = 500) and XGBoost (B = 200) bootstraps; `FALSE` reads their saved CSV files |
| `RUN_XGB_TUNED` | `TRUE` runs the tuned XGBoost bootstrap (long, resumable) |
| `MAKE_FIGURES` | `TRUE` redraws the figures |
| `RUN_SIM` | `TRUE` runs the simulation (long, parallel, resumable) |
| `R_sim`, `B_sim` | simulation replications per cell (50) and bootstrap iterations per replication (20); use `R_sim <- 3` for a quick test |

For a first full run, set `RUN_OLD_HEAVY <- TRUE`. A seed is fixed (`set.seed(2026)` and the seeds inside each step), so results can be reproduced.

## Output files

| File | Content |
|---|---|
| `high_correlation_pairs.csv`, `vif_table.csv`, `domain_dimensionality.csv` | redundancy among variables (Section 3.1) |
| `ridge_bootstrap_Uj.csv`, `xgb_bootstrap_Uj.csv`, `xgb_tuned_summary.csv` | U_j with 95% intervals and P(U_j > 0) |
| `importance_vs_Uj.csv`, `rank_gap_table.csv`, `meanabs_rank_gap.csv` | importance, ranks and rank gaps (Table 1) |
| `domain_size_confound_check.csv` | correlation of importance and U_j with domain size |
| `sim_replicates.csv`, `sim_truth_Uj.csv`, `sim_summary.csv` | simulation results (Table 2) |
| `figures/` | figures as PNG and PDF |

## Contact

Alper Umut Tosun, Department of Statistics, Yıldız Technical University (umut.tosun@std.yildiz.edu.tr)
