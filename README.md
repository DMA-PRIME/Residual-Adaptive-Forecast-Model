# Residual Adaptive Forecast Model

This repository contains a notebook-based influenza admissions forecasting pipeline built around classical+novel time-series feature engineering, mutual-information feature ranking, multicollinearity screening, Bayesian Optimization, and XGBoost regression. The project compares one-step through four-step ahead forecasting of CDC influenza admissions using merged weekly surveillance and hospital-system indicators from CDC, MUSC, and PRISMA-style data sources.

## Repository Description

Technical influenza forecasting experiments for T+1, T+2, T+3, and T+4 weekly flu admissions using merged CDC/MUSC/PRISMA data, feature selection, lag/rolling/seasonal features, VIF diagnostics, Bayesian Hyperparameter Optimization, and XGBoost regression.

## Project Contents

- `Traditional_FLU_Model_Original.ipynb` - baseline flu forecasting workflow.
- `4_25_2026_T+1_flu_forecast_using_traditional_modelling.ipynb` - one-step-ahead forecast notebook.
- `4_25_2026_T+2_flu_forecast_using_traditional_modelling.ipynb` - two-step-ahead forecast notebook.
- `4_25_2026_T+3_flu_forecast_using_traditional_modelling.ipynb` - three-step-ahead forecast notebook.
- `4_25_2026_T+4_flu_forecast_using_traditional_modelling.ipynb` - four-step-ahead forecast notebook.

## Data Inputs

The raw data files are not included in this repository. Each notebook expects the following local files under a project-root `data/` directory:

| File | Role in Pipeline |
| --- | --- |
| `data/CDC.csv` | Weekly CDC respiratory hospitalization and admissions indicators. |
| `data/MUSC.csv` | Weekly MUSC encounter, diagnosis, testing, admission, outpatient, ED, and vaccination indicators. |
| `data/PRISMA.csv` | Weekly PRISMA encounter, diagnosis, testing, admission, outpatient, ED, and vaccination indicators. |

## Feature Selection

The T+1, T+2, T+3, and T+4 forecasting notebooks use different selected feature sets, so each horizon-specific model is trained with a different number of raw and engineered features.

Feature selection is performed in stages:

- **MI score:** Candidate predictors are first ranked using mutual information against the shifted target, computed on the training portion of the series.
- **VIF score:** The selected feature set is then screened with variance inflation factor diagnostics to reduce multicollinearity among predictors.
- **Fixed features:** Some clinically important features are force-included no matter what their VIF scores are.

## Feature Engineering

The notebooks engineer time-series features from the selected exogenous variables and observed target history. Because each horizon uses a different selected feature set, the feature-engineering stage produces a different number of final model features for each model.

## Modeling

The primary estimator is `xgboost.XGBRegressor`. Bayesian optimization through Optuna is used to search for the best hyperparameter combination for each model. 

## Evaluation Metrics

The notebooks report:
- Percent agreement (PA), defined as the mean of `min(actual, predicted) / max(actual, predicted)`.
  
## Complete Pipeline at a glance:

1. Load `CDC.csv`, `MUSC.csv`, and `PRISMA.csv` from `data/`.
2. Normalize source-specific column names and merge the three datasets on weekly date.
3. Clean numeric fields by standardizing missing tokens, removing commas, converting percent strings to fractions, coercing numeric values, and preserving `Week` as a datetime field.
4. Sort observations chronologically and construct horizon-specific shifted targets.
5. Rank candidate features with `mutual_info_regression` using only the training portion of the series.
6. Retain the top mutual-information features, force-include key clinical indicators, and reduce multicollinearity using VIF diagnostics.
7. Engineer time-series features from selected exogenous variables and observed target history.
8. Split data chronologically into train, embargo gap, and hold-out test partitions.
9. Tune `XGBRegressor` with Optuna using time-series cross-validation.
10. Evaluate final hold-out predictions with percent agreement and use previous history of prediction to make better forecasting.

## Main Python Libraries

The notebooks use common data science and forecasting libraries, including:

- pandas
- numpy
- scikit-learn
- xgboost
- optuna
- matplotlib

## Running The Notebooks

1. Clone this repository.
2. Open the project folder in Jupyter Notebook, JupyterLab, VS Code, or Google Colab.
3. Make sure the `data/` folder is present in the project root.
4. Run the notebooks from top to bottom.

If a required package is missing, the notebooks include setup code for installing selected dependencies such as `optuna` and `xgboost`.

## Notes

This project is organized for experimentation and comparison across forecast horizons. Results may vary depending on package versions, data updates, and model tuning settings.
