# Kazakhstan NPL macro analysis

Quarterly analysis of how Brent oil prices and USD/KZT relate to the share of
loans overdue more than 90 days in Kazakhstan's banking sector.

The dataset and Notebook 1 are ready. Notebook 2 onward is described below as a
plan, not completed analysis. The existing Notebook 2 is a working draft; no
forecasting model, final backtest or stress-scenario result is presented yet.

## Dataset

The [input CSV](data/raw/kazakhstan_npl_oil_fx_quarterly.csv) contains **66 complete
quarters, 2010Q1–2026Q2**, with four columns:

| Column | Definition | Timing / unit |
| --- | --- | --- |
| `quarter` | Calendar quarter | `YYYYQn` |
| `npl` | Sector-wide share of loans overdue more than 90 days | Quarter-end, percent |
| `brent` | Nominal Brent crude oil price | Quarterly average, USD per barrel |
| `usdkzt` | Official USD/KZT exchange rate | Quarterly average, KZT per USD |

All numeric values are stored to two decimal places. A value of `24.96` in `npl`
means 24.96%, not a fraction. Higher USD/KZT means a weaker tenge. NPL is a stock
of outstanding overdue loans, not loans newly entering default over a 90-day window.
Oil and FX are nominal; no CPI adjustment is applied.

## Data sourcing and assembly

The input was assembled from downloaded official Excel/PDF reports and FRED
tables before notebook analysis. Notebook 1 reads the resulting CSV; it does not
download or parse the original sources. The original reports are not bundled
with this repository, and an end-to-end extraction script is not included.
Detailed filenames, source cells, report pages and historical revisions are
recorded in [data_sources.md](docs/data_sources.md).

### 1. Banking-sector NPL 90+

- Collected downloaded NBK/ARRFR banking reports and financial-indicator
  workbooks. The [NBK banking-sector archive](https://nationalbank.kz/en/news/banks-performance/rubrics/1943)
  is one of the official source locations.
- For 2010Q1–2012Q4, extracted the **TOTAL** row's overdue-more-than-90-days
  share from the early Excel workbooks. Checked it against the 90+ loan balance
  divided by the total loan portfolio.
- For 2013Q1–2013Q3, extracted the current-date percentage from the PDFs' sector-wide
  loan-quality table, using the row for loans overdue more than 90 days.
- For 2013Q4–2026Q2, used the financial-indicator workbooks' **Total** row and
  **over-90-days share of total loans**. These Excel shares replaced the initial
  PDF-derived values where available. Columns were identified from their headers
  because the layouts change across years. Source fractions were multiplied by 100.
- Used the sector aggregate, not an average of individual banks or borrower
  subgroups. Separate Stage 3 and classification-based non-performing-loan
  measures were excluded. Workbook shares were checked against amounts, and
  sector balances were reconciled with bank-row totals within source rounding.
- Mapped reporting dates to the preceding quarter: **1 April → Q1, 1 July → Q2,
  1 October → Q3, 1 January → the previous year's Q4**. Internal report headings
  were checked rather than trusting filenames or sheet names alone. For the later
  workbook series, January closing-adjusted sheets (`CD`/`ЗО`) were selected where
  available; earlier source choices are documented separately.

### 2. Brent oil prices

- Used FRED's [monthly Brent series, POILBREUSDM](https://fred.stlouisfed.org/series/POILBREUSDM),
  sourced from IMF commodity-price data, with the
  [native quarterly series, POILBREUSDQ](https://fred.stlouisfed.org/series/POILBREUSDQ)
  as a historical consistency check.
- Parsed the supplied date/value observations and assigned each month to its
  calendar quarter. Quarter-start labels identify the quarter they begin.
- Calculated the arithmetic mean of the three monthly observations for each
  complete quarter **before rounding**. The historical monthly observations through
  July 2025 and the assembled quarters through 2025Q2 were checked against FRED.
- Added 2025Q3–2026Q2 from the subsequently supplied monthly prices. This extension
  was used as provided and was **not independently re-fetched from FRED**. July 2026
  was excluded because it lies beyond the final quarter.

### 3. USD/KZT exchange rates

- Used downloaded NBK [official exchange-rate reports](https://nationalbank.kz/en/news/oficialnye-kursy).
  Extracted the USD row from annual PDFs for 2010–2022 and Excel workbooks for
  2023–2026.
- Selected the explicitly published **quarterly average** columns, not quarter-end
  daily rates or March/June/September/December monthly averages. In the recent
  workbook layout, these are cells `G15`, `K15`, `O15` and `S15`.
- Checked historical values against the source reports and corrected three
  discrepancies in the previous thesis dataset. The supplied 2024 workbook matched
  the assembled 2024 values; the 2025 and 2026 workbooks supplied the extension.
- Retained NBK's published aggregation convention. The 2021 report records a change
  to quarterly averages weighted by working days; this is a comparability caveat,
  not a method reconstructed from rounded monthly rates.

### 4. Final alignment and checks

- Joined NPL, Brent and FX by calendar quarter and sorted chronologically.
- Checked the schema, date range, missing values, duplicate quarters, numeric
  finiteness and sensible value ranges. No missing quarters were interpolated.
- Rounded final source values to two decimals and retained one four-column CSV
  in `data/raw/`. Genuine jumps were not smoothed or removed as outliers.

The sources do not guarantee a uniform reporting regime throughout the sample.
In particular, the 2015Q2 NPL decline coincides with banking-sector restructuring,
and the 2011Q4 original-versus-later-vintage discrepancy remains unresolved.
Formatting an older published percentage to two decimals does not add precision.

## Notebook 1 — completed

[Data preparation and exploration](notebooks/01_data_preparation_eda.ipynb):

- Loaded the CSV and displayed its first rows and data types.
- Documented variable definitions, source timing and units.
- Verified all 66 consecutive quarters, exact column names, no missing or infinite
  values, no duplicates, and valid NPL/oil/FX ranges.
- Created a working copy with quarter-end plotting dates without modifying the input.
- Displayed descriptive statistics and the chronological 48/18 training/test boundary.
- Reviewed the largest NPL changes, including the 2015Q2 decline, without deleting them.
- Plotted all three series rebased to **2010Q1 = 100**, plus separate original-unit
  panels. Annotated the oil-price decline, banking restructuring, tenge float,
  COVID-19 pandemic and Russia's invasion of Ukraine.
- Saved two figures in `reports/figures/`. All 14 code cells were executed successfully.

The full-history charts are descriptive. Their event markers are not confirmed
structural breaks or causal estimates. No model transformations, lag selection
or forecast fitting are performed in Notebook 1.

## Notebook 2 — completed

[Time-series diagnostics](notebooks/02_time_series_diagnostics.ipynb):

- Split the dataset chronologically: training through 2021Q4 and testing
  from 2022Q1. All analytical diagnostics use training data only.
- Plotted the training series and inspected ACF/PACF in levels.
- Applied exploratory STL decomposition with a four-quarter period.
- Tested levels and candidate transformations using ADF and KPSS.
- Calculated NPL changes in percentage points and oil/FX log changes ×100,
  retaining the original levels.
- Plotted the transformed series and inspected their ACF/PACF.
- Displayed contemporaneous correlations using 47 training quarters and
  lagged correlations for lags 0–4 using 43 shared training quarters.
- Saved seven figures. All 12 code cells executed successfully.

The diagnostics guide candidate model specifications; they do not establish
causal effects, confirm structural breaks or demonstrate forecast performance.
VIF, model fitting and chronological validation are reserved for Notebook 3.

## Planned analysis

### Notebook 3 — model fitting and validation

Planned file: `03_model_fitting_validation.ipynb`.

- Fit a simple oil + FX regression and a compact ARX alternative with lagged NPL.
  Use predictor timing consistent with information available at the forecast date.
- Check coefficient interpretation, stability, influential observations and residual
  dependence. Inspect residual ACF/PACF and serial-correlation tests after fitting.
- Consider regression with ARMA errors only if diagnostics and validation justify it.
- Compare candidates using expanding-window validation entirely within pre-2022
  data. Make data-derived choices using only each fold's available history.
- Select and lock the specification, then refit on the complete pre-2022 sample.

### Notebook 4 — backtesting and stress scenario

Planned file: `04_backtesting_stress_scenario.ipynb`.

- Evaluate the locked model on 2022Q1–2026Q2 and compare MAE/RMSE with persistence:
  **next quarter's NPL = this quarter's NPL**.
- Plot actual and predicted NPL and discuss performance under the later regime.
- Specify a four-quarter path with Brent at **USD 45** and USD/KZT **30% above its
  starting value**, explicitly documenting the starting quarter and shock duration.
- Compare baseline and stressed NPL one year ahead and report uncertainty and limits.
- Write a short plain-language conclusion without presenting predictive associations
  as identified causal effects.

The primary split follows the assignment: **2010Q1–2021Q4 for training (48 quarters)**
and **2022Q1–2026Q2 for the final test (18 quarters)**, before losses from lags.
The final test will not be used to choose transformations, break dates, lags or models.

## Repository layout

```text
data/raw/       Assembled four-column source snapshot
docs/           Detailed data provenance
notebooks/      Numbered analysis notebooks
reports/figures/ Saved charts
requirements.txt Python dependencies
```

## Running Notebook 1

From the repository root:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

Open `notebooks/01_data_preparation_eda.ipynb`, use `notebooks/` as the kernel's
working directory, restart the kernel and run all cells. Paths are project-relative.
The input CSV remains unchanged; running the notebook regenerates its two figures.
Original source reports are not required to run Notebook 1, but reconstructing the
CSV requires those external files and the extraction notes.
