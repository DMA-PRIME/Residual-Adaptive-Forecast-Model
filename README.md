# FLU Forecasting With Traditional Models

This repository contains notebook-based influenza forecasting experiments using traditional feature engineering and machine learning workflows.

## Project Contents

- `Traditional_FLU_Model_Original.ipynb` - baseline flu forecasting workflow.
- `4_25_2026_T+1_flu_forecast_using_traditional_modelling.ipynb` - one-step-ahead forecast notebook.
- `4_25_2026_T+2_flu_forecast_using_traditional_modelling.ipynb` - two-step-ahead forecast notebook.
- `4_25_2026_T+3_flu_forecast_using_traditional_modelling.ipynb` - three-step-ahead forecast notebook.
- `4_25_2026_T+4_flu_forecast_using_traditional_modelling.ipynb` - four-step-ahead forecast notebook.

## Data

The raw data files are not included in this repository. To run the notebooks, place the required CSV files in a local `data/` directory at the project root.

## Workflow

The notebooks follow the same general structure:

1. Import libraries and configure the data directory.
2. Load and prepare flu-related datasets.
3. Create engineered forecasting features.
4. Select features using mutual information and related diagnostics.
5. Train and evaluate traditional forecasting models for different forecast horizons.

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
