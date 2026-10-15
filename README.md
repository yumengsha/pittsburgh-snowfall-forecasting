# Pittsburgh Snow Removal Forecasting Project

This repository contains the codebase and data pipeline for the machine learning project aimed at forecasting daily snow removal service requests in Pittsburgh using historical 311 data, weather conditions, and traffic density. The goal is to assist the Department of Public Works in proactively deploying snow response resources.

## Project Structure

```text
.
├── notebooks/
│   ├── Dataset Creation.ipynb         # Cleaning, merging, and feature engineering from 311, weather, and traffic data
│   └── Modeling.ipynb                 # Model training, evaluation, and visualization
├── data/
│   ├── raw/                           # Raw weather and traffic CSVs
│   └── processed/                     # Feature-engineered dataset, data dictionary, preliminary EDA
├── report/
│   └── Final report.pdf               # Final course report
└── README.md                          # This file
```

## Code Overview

- `Dataset Creation.ipynb`: Filters 311 records to snow/ice removal requests, merges them with daily weather and neighborhood traffic/area data, and aggregates to the neighborhood-day level. Builds lag and rolling features per neighborhood using only past values (`groupby('neighborhood')` + `shift(1)` before rolling), and evaluates simple baselines (Lag-1d, Rolling Mean 3d, Lag-1y Same Week). Output: `data/processed/snow_removal_feature_engineered.csv`.
- `Modeling.ipynb`: Explores SARIMAX, Prophet, and LightGBM. The final LightGBM model (last cells) rebuilds the features in the same leakage-safe way and is evaluated on a chronological holdout against naive baselines, using MAE, RMSE, MAPE, and R².

## Data Notes

- **Unit of observation:** one row per neighborhood-day (91 neighborhoods, 2015-04 to 2025-01, 9,802 rows).
- **Rows exist only for neighborhood-days with at least one request.** Days with zero requests are not in the table, so the model predicts *how many* requests a neighborhood gets on a day it receives any, not whether it gets one. For the same reason, `lag_1d` and `rolling_mean_3d` refer to the neighborhood's previous observed request-days, not strictly the previous calendar days.
- **Leakage controls:** request-history features (`lag_1d`, `rolling_mean_3d`) are computed within each neighborhood and shifted by one row, so the same-day target never enters a feature. Same-day weather (`SNOW`, `TAVG`, `TMAX`, `rolling_sum_snow_3d`) is treated as available from the forecast.

## Results

Chronological holdout: train on 2015–2022 (7,773 rows), test on 2023 onward (2,029 rows).

| Model | MAE | RMSE |
|---|---|---|
| LightGBM | **2.05** | **4.37** |
| Lag-1 baseline (previous request-day) | 2.28 | 4.78 |
| Train-mean baseline | 2.92 | 4.81 |

LightGBM reduces MAE by about 10% vs. the lag-1 baseline and 30% vs. the train mean. Holdout R² is 0.15: request counts are heavy-tailed, and the big storm days are hard to predict from weather and recent history alone.

Features: `SNOW`, `TAVG`, `TMAX`, `lag_1d`, `rolling_mean_3d`, `rolling_sum_snow_3d`, `month`, `is_snow_season`.

## Required Packages

The following Python packages are required to run the notebooks:

- pandas
- numpy
- lightgbm
- matplotlib
- seaborn
- altair
- scikit-learn
- statsmodels
- prophet
- openpyxl
