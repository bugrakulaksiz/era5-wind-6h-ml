# Model Card — Six-Hour-Ahead ERA5 Wind Speed

**Task:** Estimate 10 m wind speed (m/s) at issue time +6 hours for a location near Darmstadt, Germany.

**Scope:** Educational retrospective regression, **not** an operational weather-warning or production forecast system.

**Algorithm:** scikit-learn `HistGradientBoostingRegressor`; `max_iter=100`, `learning_rate=0.05`, `max_leaf_nodes=15`.

**Input data:** Hourly ERA5 reanalysis (2022–2024) accessed via Open-Meteo: wind speed at 10 m, temperature at 2 m, relative humidity at 2 m, and mean sea-level pressure. Past/current values at lags 0, 1, 3, 6, 12 and 24 hours plus cyclical calendar features (28 predictors).

**Evaluation protocol:** Train on 2022 (8,730), validate and select configuration on 2023 (8,754), retrain on 2022–2023, and test on 2024 (8,778).

| 2024 test | MAE (m/s) | RMSE (m/s) |
|---|---:|---:|
| Persistence | 0.831849 | 1.124159 |
| Gradient boosting | 0.738684 | 0.977797 |

**Result:** 11.20% lower MAE and 13.02% lower RMSE than persistence.

**Limitations:** One grid point, one held-out year, historical reanalysis rather than operationally available inputs, no verification using independent observed station data and no comparison with professional forecast models. Some wind peaks appear underestimated in a first-week visualization; formal extreme-event evaluation is pending.

**Safety:** Not suitable for severe weather warnings, transportation safety decisions, or emergency operations. Treat any `.joblib` model as executable untrusted content unless its provenance is verified.
