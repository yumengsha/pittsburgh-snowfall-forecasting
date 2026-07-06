Pittsburgh Snow Removal Forecasting Project
===========================================

This repository contains the codebase and data pipeline for the machine learning project aimed at forecasting daily snow removal service requests in Pittsburgh using historical 311 data, weather conditions, and traffic density. The goal is to assist the Department of Public Works in proactively deploying snow response resources.

Project Structure
-----------------
.
├── notebooks/
│   ├── Clean and Feature.ipynb        # Feature engineering from 311, weather, and traffic data
│   └── Analysis copy.ipynb            # Model training, evaluation, and visualization
├── data/
│   ├── raw/                           # Raw CSVs including 311, traffic, and weather
│   └── processed/                     # Feature-engineered data and merged datasets
├── report/
│   ├── Final_Report.pdf               # Final course report
│   └── slides.pdf                     # Presentation slides
└── README.txt                         # This file

Code Overview
-------------
- Clean and Feature.ipynb: Loads raw datasets, cleans and merges 311 request records, weather data, and traffic features. Produces a feature-engineered dataset at the neighborhood-day level.
- Analysis copy.ipynb: Trains a LightGBM regression model, evaluates performance using MAE, RMSE, and MAPE, compares with baseline models (Lag-1, Lag-7, YoY), and includes SHAP analysis for model interpretability.

Required Packages
-----------------
The following Python packages are required to run the notebooks:

- pandas
- numpy
- lightgbm
- matplotlib
- seaborn
- sklearn (scikit-learn)
- shap
- datetime
- joblib
- openpyxl

