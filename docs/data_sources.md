# Quarterly dataset and source provenance

[`kazakhstan_npl_oil_fx_quarterly.csv`](../data/raw/kazakhstan_npl_oil_fx_quarterly.csv) contains 66 consecutive calendar quarters, 2010Q1–2026Q2. All three variables are complete. NPL is a quarter-end stock; Brent and FX are quarterly averages. Nothing has been interpolated.

The CSV is the assembled source-data snapshot for this analysis, before notebook transformations. The original PDF and Excel files remain in their existing locations outside this repository. Only this four-column CSV is retained in `data/raw/`.

## Columns and timing

| Column | Definition | Precision |
| --- | --- | --- |
| `quarter` | Calendar quarter represented by the observation | YYYYQn |
| `npl` | Loans with principal and/or interest overdue more than 90 days, as a percentage of the sector's total loan portfolio at quarter-end | Stored to 2 decimals in percentage units; 24.96 means 24.96%. Since 2026-10-09, 2013Q4–2025Q2 use the supplied financial-indicator Excel share cells. Earlier source observations are rounded to two decimals. |
| `brent` | Quarterly period-average Brent benchmark price in nominal USD per barrel | 2 decimals |
| `usdkzt` | NBK's published quarterly average official exchange rate, KZT per USD; a higher value means a weaker tenge | 2 decimals |

## 2025Q3–2026Q2 extension and simplified schema (2026-10-09)

The active dataset now contains only `quarter,npl,brent,usdkzt`, with all numeric
fields stored to two decimals. CPI and real-value datasets, the comparison CSV,
and the previous archive CSV were removed from the project at the user's request.
Historical source choices and comparison results remain documented below.

Checked every sheet in the two supplied NPL files (23 sheets total). Used the
**Total:** row and **over 90 days share of total loans**, identified from the headers,
not Stage 3 loans or a bank-level mean. Source fractions are multiplied by 100.
Published shares agree with the 90+ amount / total-loans ratio within source rounding,
and bank-row balances sum to sector totals within monetary rounding.
Blank bank-level amount cells are excluded from the sum, not substituted for the
published sector share. Source files are unchanged.

| Quarter | NPL source file | Sheet | Share cell | NPL (%) | FX source file / sheet | Quarterly FX cell | USD/KZT | Brent (USD/barrel) |
| --- | --- | --- | --- | ---: | --- | --- | ---: | ---: |
| 2025Q3 | `Information about owned capital liabilities and assets-6.xls` | `01.10.2025` | K32 | 3.52 | `2025 eng.xlsx / 2025` | O15 | 536.05 | 68.14 |
| 2025Q4 | `Information about owned capital liabilities and assets.xlsx` | `01.01.2026 CD` | K32 | 3.63 | `2025 eng.xlsx / 2025` | S15 | 524.76 | 63.16 |
| 2026Q1 | `Information about owned capital liabilities and assets.xlsx` | `01.04.2026` | K32 | 3.97 | `2026 eng.xlsx / 2025` | G15 | 497.73 | 77.80 |
| 2026Q2 | `Information about owned capital liabilities and assets.xlsx` | `01.07.2026` | K32 | 4.12 | `2026 eng.xlsx / 2025` | K15 | 476.00 | 97.05 |

The 2026 NPL workbook's internal headings establish the reporting year and date.
Its January ordinary and CD variants agree on NPL (3.63%); CD is selected consistently
with earlier year-ends. January/April/July/October reports describe the preceding
quarter-end. The 2026 FX workbook has a tab named `2025`, but its internal title
is **Official exchange rates, 2026**, so its G15/K15 cells map to 2026Q1/Q2.

FX is the published **quarterly average**, not the final month's average or
the last daily rate. The 2025Q1/Q2 cells still match the existing 510.17/513.77.
The 2025Q3 source value is approximately 536.05046875 and rounds to 536.05.

Brent uses the 12 full-precision monthly observations supplied in the current request,
July 2025–June 2026. For each quarter, calculate the arithmetic mean of all three months
**before** rounding to two decimals. The supplied July 2026 price is excluded.
These newly supplied oil observations were used as provided, not independently
re-fetched or claimed to have been checked against a newer FRED download.
Earlier Brent values are unchanged apart from the requested rounding.

There are 66 consecutive complete quarters, 2010Q1–2026Q2. No gaps were interpolated.
Notebook 1 uses the new names and date range. The assignment's training cutoff is
unchanged: 48 pre-2022 observations and 18 final-test observations before lags.

## NPL90+

### Financial-indicator workbook update (2026-10-09)

At the user's request, checked all 12 supplied `.xls` workbooks and all 154
monthly/closing-adjusted sheets. The active quarterly dataset now uses each
sheet's **Total:** row and **90+ overdue share of total loans** for 2013Q4–2025Q2,
rounded to two decimals. This section supersedes the earlier PDF-derived NPL
values for those quarters. The preceding assembly and comparison CSV were later removed from the project during the requested single-file cleanup.
The older extraction sections below are historical provenance, not a second set
of active values.

All source files remain unchanged in the user's Downloads directory.

| Code | Source filename | Internal reporting year | Sheets checked |
| --- | --- | --- | ---: |
| W1 | `Information about owned capital liabilities and assets-6.xls` | 2025 | 13 |
| W2 | `Information about owned capital liabilities and assets-5.xls` | 2024 | 13 |
| W3 | `Information about owned capital liabilities and assets-4.xls` | 2023 | 12 |
| W4 | `Information about owned capital liabilities and assets-3.xls` | 2022 | 12 |
| W5 | `Information about owned capital liabilities and assets-2.xls` | 2021 | 13 |
| W6 | `Information about owned capital liabilities and assets.xls` | 2020 | 13 |
| W7 | `Information about owned capital liabilities and assets39.xls` | 2019 | 13 |
| W8 | `Information about owned capital liabilities and assets27.xls` | 2018 | 13 |
| W9 | `Information about owned capital liabilities and assets13.xls` | 2017 | 13 |
| W10 | `Information about owned capital liabilities and assets2016.xls` | 2016 | 13 |
| W11 | `Information about owned capital liabilities and assets2015.xls` | 2015 | 13 |
| W12 | `Information about owned capital liabilities and assets2014eng.xls` | 2014 | 13 |

The 90+ amount/share columns move across formats: G/H in the 2014–2016 files,
H/I in the 2017–2022 files, and J/K in the 2023–2025 files. Columns were identified
from their headers, not assumed from position. In the newer layout, the separate
**Stage 3 loans and ACCI** columns are not NPL90+ and were excluded. The broader
over-7-day/over-30-day delinquency shares and individual-bank rows were also excluded.
The relevant footnote defines 90+ as overdue principal and/or interest.

For every sheet, checked the published share against the 90+ amount divided by
Total loans (D), and reconciled total loans and 90+ balances with the sum of bank
rows. Tolerances allow published source rounding: 0.005 percentage points for
share/balance agreement and bank-row rounding at the reported monetary precision.
Amounts are in thousand KZT; shares are fractions in the source, multiplied by 100
for the dataset. Published share cells were used directly, not reconstructed from
rounded monetary balances or averaged across banks.

Report dates come from the **internal heading**. In W12, the tab named
`01.01.2019` actually has a January 1, **2014** heading; it is an unadjusted
2013Q4 observation, not a 2018Q4 observation. Its share is about 31.37%, whereas
`01.01.2014 ЗО` gives about 31.15%. The closing-adjusted sheet is selected.
No source tabs were renamed or edited.

For January year-ends, use the CD/ЗО/closing-data sheet where available, consistent
with the year-end PDF convention. Where no separate adjusted sheet is supplied,
use the available January sheet and record that fact. January represents Q4 of
the preceding year; April/July/October represent Q1/Q2/Q3. Ordinary and adjusted
January variants were compared, rather than mixed or treated as separate quarters.

The initial comparison checked all 58 quarter-date records, including unselected January variants and 2025Q3. It recorded source cells, balances, raw shares and prior values. The comparison CSV and CPI-enriched output were subsequently removed during cleanup. The table below preserves the 47 selected NPL replacements.

| Quarter | Workbook | Selected sheet | Share cell | Previous NPL (%) | Updated NPL (%) |
| --- | --- | --- | --- | ---: | ---: |
| 2013Q4 | W12 | `01.01.2014 ЗО` | H48 | 31.200000 | 31.15 |
| 2014Q1 | W12 | `01.04.2014` | H48 | 32.900000 | 32.89 |
| 2014Q2 | W12 | `01.07.2014` | H48 | 32.200000 | 32.23 |
| 2014Q3 | W12 | `01.10.2014` | H48 | 29.700000 | 29.72 |
| 2014Q4 | W11 | `01.01.2015 ЗО` | H48 | 23.500000 | 23.55 |
| 2015Q1 | W11 | `01.04.2015` | H48 | 23.400000 | 23.42 |
| 2015Q2 | W11 | `01.07.2015` | H47 | 9.980000 | 9.98 |
| 2015Q3 | W11 | `01.10.2015` | H45 | 9.200000 | 9.17 |
| 2015Q4 | W10 | `01.01.2016 ЗО` | H45 | 8.000000 | 7.95 |
| 2016Q1 | W10 | `01.04.2016` | H45 | 8.400000 | 8.36 |
| 2016Q2 | W10 | `01.07.2016` | H45 | 7.900000 | 7.90 |
| 2016Q3 | W10 | `01.10.2016` | H44 | 7.900000 | 7.86 |
| 2016Q4 | W9 | `01.01.2017 CD` | I44 | 6.700000 | 6.72 |
| 2017Q1 | W9 | `01.04.2017` | I43 | 7.700000 | 7.68 |
| 2017Q2 | W9 | `01.07.2017` | I43 | 10.700000 | 10.71 |
| 2017Q3 | W9 | `01.10.2017` | I43 | 12.700000 | 12.75 |
| 2017Q4 | W8 | `01.01.2018 (CD)` | I42 | 9.300000 | 9.31 |
| 2018Q1 | W8 | `01.04.2018` | I42 | 10.000000 | 10.01 |
| 2018Q2 | W8 | `01.07.2018` | I42 | 8.800000 | 8.75 |
| 2018Q3 | W8 | `01.10.2018` | I38 | 8.500000 | 8.52 |
| 2018Q4 | W7 | `01.01.2019  (CD)` | I38 | 7.400000 | 7.38 |
| 2019Q1 | W7 | `01.04.2019` | I38 | 8.600000 | 8.61 |
| 2019Q2 | W7 | `01.07.2019` | I38 | 9.400000 | 9.38 |
| 2019Q3 | W7 | `01.10.2019` | I38 | 9.300000 | 9.34 |
| 2019Q4 | W6 | `01.01.2020 CD` | I36 | 8.100000 | 8.14 |
| 2020Q1 | W6 | `01.04.2020` | I36 | 8.900000 | 8.94 |
| 2020Q2 | W6 | `01.07.2020` | I36 | 9.000000 | 8.97 |
| 2020Q3 | W6 | `01.10.2020` | I35 | 8.400000 | 8.35 |
| 2020Q4 | W5 | `01.01.2021 (CD)` | I35 | 6.900000 | 6.85 |
| 2021Q1 | W5 | `01.04.2021` | I34 | 7.100000 | 7.10 |
| 2021Q2 | W5 | `01.07.2021` | I32 | 4.800000 | 4.77 |
| 2021Q3 | W5 | `01.10.2021` | I31 | 4.300000 | 4.29 |
| 2021Q4 | W4 | `01.01.2022` | I31 | 3.300000 | 3.31 |
| 2022Q1 | W4 | `01.04.2022` | I31 | 3.600000 | 3.57 |
| 2022Q2 | W4 | `01.07.2022` | I31 | 3.600000 | 3.61 |
| 2022Q3 | W4 | `01.10.2022` | I30 | 3.600000 | 3.60 |
| 2022Q4 | W3 | `01.01.2023 CD` | K30 | 3.400000 | 3.36 |
| 2023Q1 | W3 | `01.04.2023` | K30 | 3.440000 | 3.44 |
| 2023Q2 | W3 | `01.07.2023` | K30 | 3.290000 | 3.29 |
| 2023Q3 | W3 | `01.10.2023` | K30 | 3.270000 | 3.27 |
| 2023Q4 | W2 | `01.01.2024 CD` | K30 | 2.890000 | 2.89 |
| 2024Q1 | W2 | `01.04.2024` | K30 | 3.070000 | 3.07 |
| 2024Q2 | W2 | `01.07.2024` | K30 | 3.090000 | 3.09 |
| 2024Q3 | W2 | `01.10.2024` | K30 | 3.220000 | 3.22 |
| 2024Q4 | W1 | `01.01.2025 CD` | K30 | 3.050000 | 3.05 |
| 2025Q1 | W1 | `01.04.2025` | K30 | 3.360000 | 3.36 |
| 2025Q2 | W1 | `01.07.2025` | K31 | 3.370000 | 3.37 |

All 47 covered quarters were refreshed from the workbooks. At two decimals,
33 differ numerically from the previous PDF-sourced values and 14 are unchanged;
the maximum change is 0.05 percentage points. Most differences reflect the
previous PDFs' one-decimal presentation. For example, 2022Q1/Q2/Q3 are now
3.57%/3.61%/3.60%, rather than three identical 3.6% entries. This provides more
detail, but does not eliminate the later period's low variability.

NPL values for 2010Q1–2013Q3 retain their previous sources and are only rounded
to two decimals as requested; these new files do not resolve the separate
2011Q4 original-versus-later-vintage question. Brent and FX were unchanged in the initial NPL revision. The subsequent extension above adds 2025Q3–2026Q2 and rounds all numeric fields to two decimals.
Notebook 1's saved tables and figures were refreshed against the revised CSV.

Source: National Bank of Kazakhstan, banking-sector regulatory reporting, workbook `Information on the late payment loans2010англ.xls` supplied by the user. Official [2010 archive](https://nationalbank.kz/en/news/banks-performance/rubrics/1943).

Use the TOTAL row and column G (share of loans overdue more than 90 days), multiplied by 100. Check against column F (90+ loan balance) divided by column C (total loan balance). The separate columns S-T headed "Non-performing loans" use a classification/provision-based definition and are not the requested NPL90+ measure.

| Quarter | Reporting date / sheet | Cell | Unrounded share |
| --- | --- | --- | --- |
| 2010Q1 | 01.04.2010 | G47 | 0.24956364882754672 |
| 2010Q2 | 01.07.2010 | G47 | 0.25251847201758881 |
| 2010Q3 | 01.10.2010 | G46 | 0.25833659850209523 |

The workbook's 01.01.2010 observation represents 2009Q4 and is excluded. The source notes revisions to earlier reporting; subsequent annual files must be checked for comparable numerator and denominator definitions.

Additional user-supplied NBK workbooks: `Banks overdue nonperfoming and writtenoff loans2011.xls`, `Займы с просрочкой платежей2012.xls`, and `Information on the late payment loans2013eng.xls`. The following observations use column G on the TOTAL row, checked against F/C, and are multiplied by 100 and rounded to six decimals in the CSV.

| Quarter | Workbook year | Reporting date / sheet | Cell | Unrounded share |
| --- | --- | --- | --- | --- |
| 2010Q4 | 2011 | 01.01.2011 | G47 | 0.23752859256949369 |
| 2011Q1 | 2011 | 01.04.2011 | G47 | 0.25269660842336161 |
| 2011Q2 | 2011 | 01.07.2011 | G47 | 0.26267151695902119 |
| 2011Q3 | 2011 | 01.10.2011 | G47 | 0.29456812873608668 |
| 2011Q4 | 2012 | 01.01.2012 | G46 | 0.30601771325914912 |
| 2012Q1 | 2012 | 01.04.2012 | G46 | 0.31903962041632167 |
| 2012Q2 | 2012 | 01.07.2012 | G46 | 0.30897383298657166 |
| 2012Q3 | 2012 | 01.10.2012 | G47 | 0.30936040210956672 |
| 2012Q4 | 2013 | 01.01.2013 | G46 | 0.29792204271556816 |

January observations are assigned to Q4 of the preceding year. Sheets marked CD (closing turnovers) are not used. The 2013 workbook contains only 01.01.2013 and its CD version, so it provides 2012Q4, not observations for the 2013 quarters. These three workbooks retain the same overdue-90-days numerator and total-loan-portfolio denominator labels; this check does not establish comparability with later reporting formats.

### 2013 quarterly PDF reports

Source: user-supplied NBK Committee for Financial Market Control and Supervision reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`.

Use the sector-wide loan-quality table, row `Займы с просроченной задолженностью свыше 90 дней` (shortened to `Займы с просрочкой свыше 90 дней` in the April report), and the current reporting date's `в % к итогу` column. Do not use borrower subgroups, provisions, the growth column, or the separate classification-based `Неработающие займы` measure.

| Quarter | Report date in filename | Page / table | Published NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2013Q1 | 01-04-2013 | Page 6, Table 5 | 30.2% | 3,556.0 | 11,782.9 |
| 2013Q2 | 01-07-2013 | Page 6, Table 4 | 30.0% | 3,672.9 | 12,255.8 |
| 2013Q3 | 01-10-2013 | Page 6, Table 4 | 29.6% | 3,756.2 | 12,673.3 |
| 2013Q4 | 01-01-2014 | Page 8, Table 4 | 31.2% | 4,158.2 | 13,348.2 |

The CSV retains the published percentages, not extra precision reconstructed from rounded loan balances. Trailing zeros are storage formatting only. The January 2014 report explicitly includes closing turnovers (`с учетом заключительных оборотов`) and supplies the published year-end 2013 figure. Earlier Excel observations are retained unchanged; report vintages and closing-turnover adjustments should be considered when assessing historical comparability. The July, October, and January reports also reproduce the preceding 2013 quarterly percentages in their historical tables/charts, agreeing with the observations above.

### 2014 quarterly PDF reports

Source: user-supplied NBK reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. In each report, use page 8, Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column.

| Quarter | Report date in filename | Page / table | Published NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2014Q1 | 01-04-2014 | Page 8, Table 4 | 32.9% | 4,770.3 | 14,503.5 |
| 2014Q2 | 01-07-2014 | Page 8, Table 4 | 32.2% | 4,685.5 | 14,535.7 |
| 2014Q3 | 01-10-2014 | Page 8, Table 4 | 29.7% | 4,294.8 | 14,452.0 |
| 2014Q4 | 01-01-2015 | Page 8, Table 4 | 23.5% | 3,340.2 | 14,184.4 |

The CSV retains the published one-decimal percentages. Ratios calculated independently from the displayed loan balances round to the same percentages. The January 2015 report includes closing turnovers (`с учетом заключительных оборотов`) and supplies 2014Q4, not 2015Q1. Its historical chart also confirms the first three quarterly percentages. The fall from 29.7% in 2014Q3 to 23.5% in 2014Q4 is present in the source, not a transcription error; no cause is inferred from that movement alone.

### 2015 quarterly PDF reports

Source: user-supplied NBK reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. In each report, use page 8, Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column.

| Quarter | Report date in filename | Page / table | Published table NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2015Q1 | 01-04-2015 | Page 8, Table 4 | 23.4% | 3,309.8 | 14,133.2 |
| 2015Q2 | 01-07-2015 | Page 8, Table 4 | 9.98% | 1,247.6 | 12,503.9 |
| 2015Q3 | 01-10-2015 | Page 8, Table 4 | 9.2% | 1,315.7 | 14,350.1 |
| 2015Q4 | 01-01-2016 | Page 8, Table 4 | 8.0% | 1,236.9 | 15,553.7 |

The CSV retains the table's published percentages and precision, consistent with the preceding PDF extractions. Some historical chart labels provide two decimals (23.42% for 2015Q1 and 9.17% for 2015Q3); these agree with the table after rounding and have not replaced the table values. Ratios calculated independently from the displayed balances match the table's percentages at its published precision. The January 2016 report includes closing turnovers and supplies 2015Q4, not 2016Q1.

The July 2015 report's page 4, footnote 2, links the reduction in the sector's loan portfolio to transfers of assets and liabilities between Kazkommertsbank and BTA Bank, termination of BTA Bank's license, and BTA's exit from the banking system. The NPL90+ share falls sharply from 23.4% in 2015Q1 to 9.98% in 2015Q2. Flag 2015Q2 as a potential structural break for modelling; the observed reduction must not be interpreted solely as a response to oil or exchange rates. The footnote concerns the loan-portfolio reduction and does not quantify each factor's contribution to the NPL-ratio change.

### 2016-2017 quarterly PDF reports

Source: eight user-supplied NBK reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. In every report, use page 8, Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column.

| Quarter | Report date in filename | Page / table | Published table NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2016Q1 | 01-04-2016 | Page 8, Table 4 | 8.4% | 1,305.0 | 15,619.3 |
| 2016Q2 | 01-07-2016 | Page 8, Table 4 | 7.9% | 1,209.4 | 15,315.5 |
| 2016Q3 | 01-10-2016 | Page 8, Table 4 | 7.9% | 1,217.4 | 15,490.2 |
| 2016Q4 | 01-01-2017 | Page 8, Table 4 | 6.7% | 1,042.1 | 15,510.8 |
| 2017Q1 | 01-04-2017 | Page 8, Table 4 | 7.7% | 1,171.7 | 15,248.1 |
| 2017Q2 | 01-07-2017 | Page 8, Table 4 | 10.7% | 1,663.0 | 15,533.3 |
| 2017Q3 | 01-10-2017 | Page 8, Table 4 | 12.7% | 1,772.3 | 13,902.9 |
| 2017Q4 | 01-01-2018 | Page 8, Table 4 | 9.3% | 1,265.2 | 13,590.5 |

The CSV retains the published one-decimal percentages. Independent ratios of displayed 90+ balances to total loans round to the same values. Both January reports include closing turnovers and supply Q4 of the preceding year. Their historical charts agree with the corresponding year's first three quarterly observations. Provision amounts and coverage ratios are not NPL90+ shares and are excluded. All earlier NPL observations and the oil and exchange-rate columns are unchanged.

### 2018-2019 quarterly PDF reports

Source: eight user-supplied NBK reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. Use Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column. The table moves from page 8 in the 2018 observations to page 7 in the 2019 observations.

| Quarter | Report date in filename | Page / table | Published table NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2018Q1 | 01-04-2018 | Page 8, Table 4 | 10.0% | 1,331.6 | 13,306.3 |
| 2018Q2 | 01-07-2018 | Page 8, Table 4 | 8.8% | 1,179.9 | 13,482.1 |
| 2018Q3 | 01-10-2018 | Page 8, Table 4 | 8.5% | 1,123.7 | 13,194.1 |
| 2018Q4 | 01-01-2019 | Page 8, Table 4 | 7.4% | 1,016.3 | 13,762.7 |
| 2019Q1 | 01-04-2019 | Page 7, Table 4 | 8.6% | 1,122.8 | 13,044.8 |
| 2019Q2 | 01-07-2019 | Page 7, Table 4 | 9.4% | 1,280.2 | 13,646.9 |
| 2019Q3 | 01-10-2019 | Page 7, Table 4 | 9.3% | 1,331.3 | 14,256.6 |
| 2019Q4 | 01-01-2020 | Page 7, Table 4 | 8.1% | 1,198.8 | 14,742.0 |

The CSV retains the published one-decimal percentages. Independent ratios of the displayed 90+ balances to total loans round to the same values. January reports supply Q4 of the preceding year; the January 2019 report explicitly includes closing turnovers. For the 2019 reports, Table 3 also shows gross carrying amounts and amounts net of provisions: neither is the denominator used in Table 4. Retain Table 4's total principal loan balance (for example, KZT 14,742.0 billion at 01.01.2020, not gross carrying amount KZT 15,307.4 billion). All earlier NPL observations and the oil and exchange-rate columns are unchanged.

### 2020-2021 quarterly PDF reports

Source: eight user-supplied reports of the Agency of the Republic of Kazakhstan for Regulation and Development of the Financial Market (ARRFR), `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. Use Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column. Table 4 is on page 7, except the April 2021 report, where its header and total are on page 7 and its 90+ row continues on page 8.

| Quarter | Report date in filename | Page / table | Published table NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2020Q1 | 01-04-2020 | Page 7, Table 4 | 8.9% | 1,365.1 | 15,261.0 |
| 2020Q2 | 01-07-2020 | Page 7, Table 4 | 9.0% | 1,349.3 | 14,992.5 |
| 2020Q3 | 01-10-2020 | Page 7, Table 4 | 8.4% | 1,278.2 | 15,306.3 |
| 2020Q4 | 01-01-2021 | Page 7, Table 4 | 6.9% | 1,082.1 | 15,792.1 |
| 2021Q1 | 01-04-2021 | Pages 7-8, Table 4 | 7.1% | 1,120.8 | 15,792.7 |
| 2021Q2 | 01-07-2021 | Page 7, Table 4 | 4.8% | 800.1 | 16,764.4 |
| 2021Q3 | 01-10-2021 | Page 7, Table 4 | 4.3% | 775.1 | 18,084.8 |
| 2021Q4 | 01-01-2022 | Page 7, Table 4 | 3.3% | 668.8 | 20,200.4 |

The CSV retains the published one-decimal percentages. Independent ratios of displayed 90+ balances to Table 4's total principal loan balances round to the same values. Gross carrying amounts, amounts net of provisions, and borrower-specific tables are not used. January reports supply Q4 of the preceding year. The April 2021 report's Graph 2 labels the historical 01.01.2021 observation as 6.80%, whereas Table 4 and the original January 2021 report give 6.9%; the dataset retains the table value, also supported by the displayed balances. The fall from 7.1% in 2021Q1 to 4.8% in 2021Q2 is present in the source, not a transcription error; no cause is inferred from it alone. All earlier NPL observations and the oil and exchange-rate columns are unchanged.

### 2022-2023 quarterly PDF reports

Source: eight user-supplied ARRFR reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. In each report, use page 7, Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column.

| Quarter | Report date in filename | Page / table | Published table NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2022Q1 | 01-04-2022 | Page 7, Table 4 | 3.6% | 729.2 | 20,447.8 |
| 2022Q2 | 01-07-2022 | Page 7, Table 4 | 3.6% | 769.7 | 21,305.6 |
| 2022Q3 | 01-10-2022 | Page 7, Table 4 | 3.6% | 801.8 | 22,300.5 |
| 2022Q4 | 01-01-2023 | Page 7, Table 4 | 3.4% | 814.6 | 24,254.7 |
| 2023Q1 | 01-04-2023 | Page 7, Table 4 | 3.44% | 847.4 | 24,621.8 |
| 2023Q2 | 01-07-2023 | Page 7, Table 4 | 3.29% | 852.7 | 25,893.3 |
| 2023Q3 | 01-10-2023 | Page 7, Table 4 | 3.27% | 905.6 | 27,694.0 |
| 2023Q4 | 01-01-2024 | Page 7, Table 4 | 2.89% | 863.8 | 29,853.7 |

The CSV retains each table's published precision: one decimal for 2022 and two decimals for 2023. Graph 2 rounds 2023 observations to one decimal; its labels have not replaced the more precise table values. Independent ratios of displayed 90+ balances to Table 4's total principal loan balances round to the published percentages. Gross carrying amounts, amounts net of provisions, growth rates, provision coverage, and borrower subgroups are excluded. January reports supply Q4 of the preceding year. All earlier NPL observations and the oil and exchange-rate columns are unchanged.

### 2024-2025 quarterly PDF reports

Source: six user-supplied ARRFR reports, `Текущее состояние банковского сектора Республики Казахстан`, with filenames `tekuschee-sostoyanie-bankovskogo-sektora-rk-po-sostoyaniyu-na-<report-date>.pdf`. In each report, use page 7, Table 4, the sector-wide `Займы с просроченной задолженностью свыше 90 дней` row and the current reporting date's `в % к итогу` column.

| Quarter | Report date in filename | Page / table | Published table NPL90+ | 90+ loans, KZT billion | Total loans, KZT billion |
| --- | --- | --- | --- | --- | --- |
| 2024Q1 | 01-04-2024 | Page 7, Table 4 | 3.07% | 928.7 | 30,282.9 |
| 2024Q2 | 01-07-2024 | Page 7, Table 4 | 3.09% | 987.7 | 31,967.1 |
| 2024Q3 | 01-10-2024 | Page 7, Table 4 | 3.22% | 1,074.4 | 33,359.9 |
| 2024Q4 | 01-01-2025 | Page 7, Table 4 | 3.05% | 1,094.1 | 35,835.1 |
| 2025Q1 | 01-04-2025 | Page 7, Table 4 | 3.36% | 1,232.6 | 36,681.5 |
| 2025Q2 | 01-07-2025 | Page 7, Table 4 | 3.37% | 1,319.4 | 39,112.4 |

The CSV retains the tables' published two-decimal percentages. Independent ratios of displayed 90+ balances to Table 4's total principal loan balances round to the same values. Gross carrying amounts, amounts net of provisions, growth rates, provision coverage, and borrower subgroups are excluded. The January 2025 report supplies 2024Q4; the April and July 2025 reports supply 2025Q1 and 2025Q2 respectively. Brent and FX for these two quarters were subsequently verified and added as documented below. All earlier NPL observations are unchanged. The later Excel extension above supplies 2025Q3/Q4 and 2026Q1/Q2.

## Brent

Source: IMF Primary Commodity Prices, distributed through FRED, [Global price of Brent Crude, POILBREUSDQ](https://fred.stlouisfed.org/series/POILBREUSDQ). [Full quarterly table](https://fred.stlouisfed.org/data/POILBREUSDQ). Checked on 2026-10-07.

The existing local `POILBREUSDM.csv` contains quarterly observations despite the monthly series identifier in its filename. Its 2010-2024 values were checked against FRED's native quarterly POILBREUSDQ table and agree to the CSV's six-decimal precision. FRED labels quarter observations by their first day: 2010-01-01 maps to 2010Q1, not to the preceding quarter.

On 2026-10-08, all 184 user-pasted monthly observations from 2010-04-01 through 2025-07-01 were compared with [FRED's POILBREUSDM monthly table](https://fred.stlouisfed.org/data/POILBREUSDM) and matched. The arithmetic mean of each quarter's three monthly observations also matched the native quarterly series, including all 60 existing 2010-2024 CSV values to six decimals. January-March 2010, absent from the pasted excerpt, are present in FRED and were included in that quarterly check. Added the two requested quarterly observations from POILBREUSDQ: 2025Q1 = 75.04278053830230 and 2025Q2 = 66.95555555555557 USD/barrel, stored as 75.042781 and 66.955556. No incomplete quarter was averaged, and 2025Q3-Q4 Brent cells have not been filled in this update.

The thesis file's `nominal_price` column differs from this Brent series; its underlying benchmark has not been established. It was not used. For example, 2024Q4 is 74.000940 USD/barrel in the verified Brent series, versus 69.66 in the thesis merged file. No deflator has been applied.

## USD/KZT

Source: National Bank of Kazakhstan, [official exchange rates on average for the period](https://nationalbank.kz/en/news/oficialnye-kursy), using the user's downloaded annual source files in the thesis `USDKZT` folder.

For 2010-2022, the USD row was extracted from each annual PDF, using the four explicitly published quarterly columns, rather than recomputing an average from rounded monthly values. Sources are `2010en.pdf` through `2016en.pdf`, `2017kz.pdf`, `2018en.pdf`, `2019 eng11.pdf`, `2020en.pdf`, `2021 eng.pdf`, and `2022 eng.pdf`. For 2023 and 2024, sources are `2023 eng.xlsx` and `2024 eng.xlsx`, sheet names matching the year, USD row 15, quarterly cells G15/K15/O15/S15. Values are rounded to two decimals; the underlying 2023Q3 source value is 455.105714285714. The cell references were rechecked on 2026-10-08 and corrected in this documentation; the previously stored quarterly FX values were already correct.

All 60 quarterly exchange-rate observations were compared with these NBK files. Three discrepancies in the thesis merged CSV were corrected:

| Quarter | Thesis CSV | NBK source / new CSV |
| --- | --- | --- |
| 2014Q2 | 186.66 | 182.66 |
| 2017Q4 | 333.41 | 334.41 |
| 2020Q1 | 389.59 | 389.56 |

The 2021 PDF states that, starting in 2021Q1, quarterly average rates use an arithmetic average weighted by working days. This is an aggregation-method change that should be documented in the analysis; this initial dataset retains the published NBK quarterly figures on both sides of the change.

### 2024 consistency check and 2025 extension

On 2026-10-08, checked the user-supplied Downloads files `2024 eng.xlsx` and `2025 eng.xlsx`. Quarterly headings are in G3/K3/O3/S3, consistent with the original thesis-folder 2023 and 2024 workbooks, also rechecked. Use the USD row (C15 = USD, B15 = 1 US DOLLAR), and quarterly cells G15/K15/O15/S15, not the March/June/September/December monthly columns F15/J15/N15/R15. Both Downloads files' B46 note states that period-average rates are arithmetic averages over the working days of the period.

| Quarter | Downloads workbook / sheet | Cell | Source quarterly USD/KZT | CSV USD/KZT |
| --- | --- | --- | --- | --- |
| 2024Q1 | 2024 eng.xlsx / 2024 | G15 | 450.36 | 450.36 |
| 2024Q2 | 2024 eng.xlsx / 2024 | K15 | 447.70 | 447.70 |
| 2024Q3 | 2024 eng.xlsx / 2024 | O15 | 477.65 | 477.65 |
| 2024Q4 | 2024 eng.xlsx / 2024 | S15 | 499.87 | 499.87 |
| 2025Q1 | 2025 eng.xlsx / 2025 | G15 | 510.17 | 510.17 |
| 2025Q2 | 2025 eng.xlsx / 2025 | K15 | 513.77 | 513.77 |
| 2025Q3 | 2025 eng.xlsx / 2025 | O15 | 536.0504687499999 | 536.05 |
| 2025Q4 | 2025 eng.xlsx / 2025 | S15 | 524.76 | 524.76 |

All four 2024 observations match the existing CSV exactly. Retained 2025Q1-Q2 quarterly FX observations, rounded to two decimals. The incomplete 2025Q3-Q4 rows were initially appended and then removed at the user's request; the current extension reinstates them with complete NPL and Brent observations. The retained 2025Q1/Q2 FX observations are unchanged.
