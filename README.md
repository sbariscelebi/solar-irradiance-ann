# Solar Irradiance Prediction with ANN and Keras Tuner

This repository contains the complete implementation of a deep learning pipeline for **hourly solar irradiance (GHI) prediction** using an Artificial Neural Network (ANN) with Keras Tuner-based hyperparameter optimization. The code covers all steps from data preprocessing and feature engineering to model training, evaluation, and visualization, and is ready for reproducible research and publication-level workflows.

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Features and Preprocessing](#features-and-preprocessing)
- [Model Architecture & Hyperparameter Optimization](#model-architecture--hyperparameter-optimization)
- [Evaluation Metrics](#evaluation-metrics)
- [Visualization](#visualization)
- [How to Use](#how-to-use)
- [Requirements](#requirements)
- [References](#references)
- [License](#license)

---

## Overview

This project aims to predict **Global Horizontal Irradiance (GHI)** for a site in Batman, Turkey, using a feedforward ANN. The pipeline includes:

- Data cleaning and outlier filtering
- Imputation and scaling
- Feature engineering (temporal & meteorological variables, cyclic encoding)
- ANN model training and evaluation
- Hyperparameter optimization with Keras Tuner
- Performance visualization and error analysis

---

## Dataset

- **Source:** [Solcast API](https://solcast.com/) (Non-commercial academic license)
- **Location:** Batman, Turkey (37.83625° N, 41.360574° E)
- **Time Period:** Jan 2022 – Jan 2025
- **Resolution:** Hourly
- **Key variables:**  
  - `period_end` (timestamp)  
  - `ghi` (W/m², Global Horizontal Irradiance)  
  - `air_temperature` (°C)  
  - `cloud_opacity`, `albedo`, and other meteorological parameters

*Note: The dataset is not distributed here due to licensing. Eligible academic users can request it from [Solcast](https://solcast.com/).*

---

## Features and Preprocessing

- **Missing value imputation:** Median imputer (`SimpleImputer(strategy="median")`)
- **Scaling:** StandardScaler (mean=0, std=1)
- **Feature engineering:**
    - Temporal variables: hour, dayofweek, month, season, etc.
    - Cyclic encoding: hour_sin, hour_cos, season_sin, season_cos
    - Synthetic variable: solar_energy = ghi × 0.18 (PV efficiency)
- **Filtering:** Nighttime (ghi ≤ 0) rows are excluded.

---

## Model Architecture & Hyperparameter Optimization

- **Model:** Feedforward ANN (Keras Sequential)
- **Optimization:** Keras Tuner (RandomSearch)
    - Input layer units: 32–256
    - Hidden layers: 1–3, each with 16–128 units
    - Learning rate: [1e-2, 1e-3, 1e-4]
- **Validation:** TimeSeriesSplit (5-fold), preventing look-ahead bias
- **Early stopping:** patience=5

---

## Evaluation Metrics

- **MAE** (Mean Absolute Error)
- **RMSE** (Root Mean Squared Error)
- **R²** (Coefficient of Determination)
- **MAPE** (Mean Absolute Percentage Error)
- **MdAE** (Median Absolute Error)

All results are saved as `.xlsx` for reproducibility.

---

## Visualization

The pipeline generates the following plots for interpretability and reporting:
- Hourly & monthly average GHI profiles
- GHI vs. temperature scatter plots
- Feature correlation heatmaps
- ANN training history (loss & MAE vs. epoch)
- Actual vs. predicted time series
- Scatter plots and error distributions (histogram, time series)
