# Retail sales forecasting

This project explores demand forecasting for the Corporación Favorita grocery dataset, focusing on stores in the Guayas province.  
The goal is to model daily product demand and compare different forecasting approaches, ranging from classical time-series models to modern machine-learning methods.

The project follows an end-to-end workflow: data preparation, exploratory analysis, feature engineering, model training, evaluation, and comparison.

---

## Project objective

Customer demand in Guayas changes quickly and is influenced by promotions, seasonality, holidays, and broader economic signals.  
The objective is to forecast daily sales for every product in every store for the period **January to March 2014**, providing a reliable basis for operational planning.

---

## Data sources

The analysis combines multiple datasets from Corporación Favorita:

- daily sales (`train.csv`)
- store metadata (location, cluster)
- item metadata (family, perishability)
- transactions (proxy for footfall)
- holiday and event calendar
- oil prices (macro-economic signal)

To keep the analysis tractable and precise, the project focuses on the **three largest product families** in Guayas. Row sampling was avoided, as it significantly reduced forecasting accuracy during exploration.

---

## Repository structure

```
notebooks/
├── data_prep.ipynb        # Data preparation, feature engineering, and EDA
├── model_arima.ipynb      # SARIMAX model on a single store–item series
├── model_xgboost.ipynb    # Global XGBoost model with lag and calendar features
├── model_prophet.ipynb    # Prophet baseline on aggregated daily demand
output/
├── executive_summary.md   # High-level findings and model comparison
README.md
```

Each notebook can be opened and run directly in Google Colab using the embedded links.

⸻

Modeling approaches

Three modeling strategies are explored:
	•	SARIMAX
Applied to a single store–item time series as a proof of concept for classical statistical forecasting with exogenous variables.
	•	XGBoost
A global machine-learning model trained across many store–item series using lag features and calendar signals. Hyperparameters are tuned via time-series-aware cross-validation.
	•	Prophet
A baseline model applied to aggregated daily demand, providing an interpretable comparison with built-in trend and seasonality.

⸻

Evaluation

Models are evaluated on a held-out forecast window (January–March 2014) using:
	•	RMSE
	•	MSE
	•	MAE
	•	R²

MAPE is intentionally avoided, as retail demand often includes zero or near-zero sales, which leads to unstable percentage errors.

⸻

Key takeaway

While classical time-series models provide useful baselines, XGBoost clearly outperforms the alternatives, achieving the lowest error metrics and the highest explained variance.
This highlights the importance of feature engineering and nonlinear modeling when forecasting volatile retail demand.
