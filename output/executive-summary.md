# Executive summary

## Context and objective

Retail demand in the Guayas province is highly dynamic, shaped by promotions, seasonality, holidays, and local behavior patterns.  
The objective of this project was to forecast daily grocery sales for all products and stores in Guayas for the first quarter of 2014, using historical data and a combination of statistical and machine-learning models.

---

## Understanding the data

Multiple datasets were combined to capture different aspects of customer behavior:

- sales data describes what was sold and when
- store and item metadata explain where and what is sold
- transaction counts proxy customer footfall
- holidays capture shifts in shopping behavior
- oil prices provide a macro-economic context

Exploratory analysis revealed patterns consistent with the Pichincha example:
- a clear upward trend over time
- strong weekly seasonality
- sharp drops to zero sales around the New Year, likely reflecting store closures

December consistently stands out as the strongest sales month, confirming holiday-driven demand.

One notable difference in Guayas is visible in the day-type analysis: average sales on standard work days are lower than on bridge days, transferred holidays, and additional holidays.  
Because the analysis focused on the three largest product families, a perishable vs. non-perishable comparison was not possible in this setup.

---

## Modeling approach

Three forecasting approaches were tested:

### SARIMAX
A SARIMAX model was applied to a single store–item pair as a proof of concept.  
At this granularity, demand is extremely sparse, with many zero-sales days and occasional spikes.

- RMSE: 1.53  
- MSE:  2.34  
- MAE:  0.98  
- R²:   0.16  

The model captures the overall level but struggles with sharp jumps, which is typical for classical statistical models on irregular retail series.  
The key outcome is validation of the end-to-end forecasting workflow rather than predictive accuracy.

An aggregated SARIMAX version was also tested to model total daily demand:

- RMSE: 1054.01  
- MSE:  1,110,933.65  
- MAE:  817.76  
- R²:   0.01  

Aggregation smooths the series but does not eliminate volatility caused by promotions and product-mix shifts. The model follows the general rhythm but cannot capture combined spikes across stores.

---

### XGBoost
XGBoost serves as the primary forecasting model, using lag features, calendar variables, and promotion indicators.  
Hyperparameter tuning via randomized search further improved generalization.

- RMSE: 1.33  
- MSE:  1.78  
- MAE:  0.10  
- R²:   0.75  

The model closely tracks weekly patterns and overall demand levels, even during sharper peaks.  
The strong R² indicates that XGBoost explains a large share of the variability in daily demand.

---

### Prophet
Prophet was included as a simple, interpretable baseline on aggregated daily sales.

- RMSE: 904.31  
- MSE:  817,772.78  
- MAE:  673.12  
- R²:   0.27  

Prophet captures trend and weekly seasonality well but struggles with sudden demand spikes.  
The relatively wide confidence intervals reflect uncertainty inherent in retail demand.

---

## Model comparison

| Model            | RMSE     | MSE          | MAE    | R²   |
|------------------|----------|--------------|--------|------|
| SARIMAX          | 1.53     | 2.34         | 0.98   | 0.16 |
| SARIMAX (agg.)   | 1054.01  | 1,110,933.65 | 817.76 | 0.01 |
| XGBoost          | 1.33     | 1.78         | 0.10   | 0.75 |
| Prophet          | 904.31   | 817,772.78   | 673.12 | 0.27 |

XGBoost clearly outperforms the other approaches, combining low error with strong explanatory power.  
Classical models provide useful baselines but are limited by the volatility and nonlinearity of retail demand.

---

## Conclusions and next steps

- Data preparation and feature engineering had the largest impact on model performance
- Small EDA decisions, such as sampling, significantly affected accuracy
- XGBoost emerged as the most robust and scalable forecasting approach
- Classical time-series models remain useful for diagnostics and baselines, but not for final forecasts in this setting

A natural next step would be to extend the feature set beyond the three largest product families and evaluate whether the observed gains generalize across the full assortment.
