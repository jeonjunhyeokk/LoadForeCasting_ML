# South Korea Electricity Load Forecasting

Hourly power demand forecasting experiments using EV charging, weather and calendar features.

## Project

This project studies the contribution of exogenous variables to hourly electricity demand forecasts in South Korea. The final integrated dataset covers 2020–2023. Models explored include multiple linear regression (MLR), K-nearest neighbors (KNN), Random Forest, XGBoost and LSTM.

## Dataset

`data/DATA.csv` contains 35,040 hourly observations from 2020-01-01 through 2023-12-31.

| Field | Description |
|---|---|
| `날짜` | Timestamp |
| `전력수요량(MWh)` | Electricity demand target |
| `급속충전`, `완속충전` | Fast and slow EV charging volume |
| `기온(°C)`, `습도(%)`, `지면온도(°C)` | Weather features |
| `is_holiday`, `day_type` | Holiday and weekday/weekend indicators |

The integrated dataset is included. Its source files and their distribution terms should be checked with the original providers before separate redistribution.

## Results

### Five-fold time-series cross-validation

Mean metrics across five folds; `gap=24` hours was used between train and validation segments.

| Model | MAE | MAPE (%) | RMSE | R² |
|---|---:|---:|---:|---:|
| XGBoost | 531.04 | 0.81 | 776.07 | 0.99 |
| Random Forest | 655.56 | 1.00 | 929.42 | 0.99 |
| MLR | 848.87 | 1.33 | 1099.12 | 0.99 |
| KNN | 1398.89 | 2.18 | 1930.23 | 0.96 |
| LSTM | 63724.17 | 98.98 | 64406.32 | -46.68 |

### Monthly rolling evaluation

Each month was evaluated against the following month. Values below are averages over the available monthly splits.

| Model | MAE | MAPE (%) | RMSE | R² |
|---|---:|---:|---:|---:|
| MLR | 815.14 | 1.29 | 1060.98 | 0.98 |
| Random Forest | 1453.07 | 2.23 | 2139.86 | 0.91 |
| XGBoost | 1474.14 | 2.26 | 2217.41 | 0.91 |
| KNN | 2684.16 | 4.20 | 3687.20 | 0.76 |
| LSTM | 63712.81 | 99.94 | 64189.70 | -72.65 |

Results depend on each experiment's data preparation, split and model settings. LSTM performed poorly in the saved experiments and is reported for completeness.

## Repository contents

```
data/       Final integrated dataset
figures/    Selected correlation and model figures
notebooks/  Dataset preparation notes and model evaluation
results/    Cross-validation and monthly rolling summaries
```

`Dataset.ipynb` documents the data integration process, but its original demand, EV and weather input files are not included. `Weather.ipynb` uses `data/DATA.csv`; its saved cell outputs were cleared. Running it requires Python packages for pandas, scikit-learn, XGBoost, TensorFlow and plotting.

## References

- Panama short-term load forecasting dataset used in early exploratory notebooks: [Kaggle](https://www.kaggle.com/datasets/ernestojaguilar/shortterm-electricity-load-forecasting-panama)
- Background study: [Information, 12(2), 50 (2021)](https://www.mdpi.com/2078-2489/12/2/50)
