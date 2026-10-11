# Kazakhstan NPL macro analysis

This project examines how Brent oil prices and USD/KZT relate to Kazakhstan's
banking-sector NPL90+ share. The analysis covers data preparation, time-series
diagnostics, autoregressive modelling, chronological forecast evaluation and a
four-quarter stress scenario.

## Data

The [dataset](data/raw/kazakhstan_npl_oil_fx_quarterly.csv) contains 66 quarterly
observations, **2010Q1–2026Q2**.

- **NPL90+:** National Bank of Kazakhstan [banking financial indicators](https://nationalbank.kz/en/news/banks-performance), with related [loan statistics](https://nationalbank.kz/en/news/loans-to-economy-from-second-tier-banks/rubrics/1840).
- **Brent:** FRED [POILBREUSDM](https://fred.stlouisfed.org/data/POILBREUSDM).
- **USD/KZT:** National Bank of Kazakhstan [official exchange rates](https://nationalbank.kz/en/news/oficialnye-kursy).

NPL is the quarter-end overdue-loan share (%); Brent and FX are quarterly
averages in USD/barrel and KZT/USD. See [source notes](docs/data_sources.md) for
assembly and verification caveats.

## Approach

- Checked data quality and plotted the three series with historical event markers.
- Examined transformations, ADF/KPSS stationarity tests, decomposition, ACF/PACF and correlations.
- Compared NPL-only ARIMA candidates and selected an ARX(2) specification with oil lag 2 and FX lag 4, checking VIF and residual dependence.
- Used **48 training quarters (2010Q1–2021Q4), 10 validation quarters (2022Q1–2024Q2) and 8 final-test quarters (2024Q3–2026Q2)**.
- Compared fixed ARX, expanding-window adaptive ARX, fixed AR(2) and persistence using rolling one-quarter-ahead forecasts.
- Projected NPL recursively under unchanged macro levels and under oil at USD 45 with USD/KZT 30% higher.

The main model's coefficients are estimated only on pre-2022 data. Adaptive ARX
is supplementary: it refits using outcomes already observed at each forecast
origin, including earlier validation and test quarters.

## Results

Final-test errors are measured in NPL percentage points.

| Model | MAE | RMSE |
|---|---:|---:|
| Persistence | **0.1713** | **0.1983** |
| AR(2) | 0.3787 | 0.4033 |
| Adaptive ARX | 0.5209 | 0.5849 |
| Fixed ARX | 0.6001 | 0.6796 |

From 2026Q2, the fixed-ARX scenario projects **2027Q2 NPL of 2.87% under stress
versus 7.09% under the baseline**, starting from 4.12%. The stress path holds
Brent at USD 45 and USD/KZT at 618.80, 30% above its starting level.

## Notebooks

Run in numerical order:

1. [Data preparation and exploration](notebooks/01_data_preparation_eda.ipynb) — quality checks and historical charts.
2. [Time-series diagnostics](notebooks/02_time_series_diagnostics.ipynb) — transformations, stationarity, dependence and correlations.
3. [NPL-only model comparison](notebooks/03_model_fitting_validation.ipynb) — split exports and ARIMA candidates.
4. [ARX specification selection](notebooks/04_arx_specification_selection.ipynb) — macro lags, coefficients and residual diagnostics.
5. [Forecast validation](notebooks/05_forecast_validation.ipynb) — fixed and adaptive ARX validation.
6. [Final testing and stress scenario](notebooks/06_final_testing_stress_scenario.ipynb) — benchmarks, unified forecast chart, scenario and conclusion.

## Repository structure

```text
dbk-task/
├── data/
│   ├── raw/kazakhstan_npl_oil_fx_quarterly.csv
│   └── processed/
│       ├── train.csv
│       └── test.csv
├── notebooks/              # six sequential analysis notebooks
├── docs/data_sources.md    # sources and data caveats
├── diagnostics/            # model definitions and evaluation results
├── README.md
└── requirements.txt
```

`test.csv` contains the full post-2021 evaluation pool; Notebook 5 divides it
into the validation and final-test periods.

## Technical notes

The analysis uses Python, pandas, NumPy, statsmodels, matplotlib and seaborn.

```bash
python -m pip install -r requirements.txt
jupyter lab
```

Run the notebooks from the `notebooks/` directory in numerical order. Plots are
displayed inline only.

## Conclusion

Lower oil prices would normally weaken Kazakhstan's export income and economic
activity, while tenge depreciation can increase repayment burdens for unhedged
borrowers with foreign-currency debt, putting upward pressure on defaults.
However, NPL90+ is the outstanding bad-loan share rather than a rate of new
defaults, so write-offs, transfers and growth in total lending can reduce it
even when borrowers face greater financial stress.
Historical trends and lagged correlations therefore do not establish the causal
response of credit quality to oil or exchange-rate shocks.

In our eight-quarter final test, persistence outperforms all fitted models;
adaptive ARX improves on fixed ARX but provides no advantage over the benchmarks
and remains supplementary because it learns from post-2021 outcomes.
The counterintuitive stress projection reflects the positive fitted oil
coefficient, while the new FX shock only enters beyond the one-year horizon,
so the projected NPL decline must not be interpreted as an economic benefit.
The small sample, major historical shifts, oil-shock extrapolation and lack of
uncertainty intervals limit confidence in the estimated relationships.
The work demonstrates a transparent forecasting workflow, but does not establish
a reliable causal estimate of how much NPL would rise under the proposed stress.
