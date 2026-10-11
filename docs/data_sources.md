# Data sources

The [assembled dataset](../data/raw/kazakhstan_npl_oil_fx_quarterly.csv) contains
66 consecutive quarters, **2010Q1–2026Q2**, with columns
`quarter,npl,brent,usdkzt`.

## Sources and timing

- **NPL90+:** National Bank of Kazakhstan [banking financial-indicator reports](https://nationalbank.kz/en/news/banks-performance). The related [loan-statistics archive](https://nationalbank.kz/en/news/loans-to-economy-from-second-tier-banks/rubrics/1840) provides lending background, not the NPL90+ shares used here. NPL is the sector-wide quarter-end share of loan balances with principal and/or interest overdue more than 90 days, expressed as a percentage of the total loan portfolio.
- **Brent:** FRED [Global price of Brent Crude, POILBREUSDM](https://fred.stlouisfed.org/data/POILBREUSDM), in nominal USD per barrel. Quarterly values are arithmetic averages of the three monthly observations.
- **USD/KZT:** National Bank of Kazakhstan [official exchange rates on average for the period](https://nationalbank.kz/en/news/oficialnye-kursy). Values are the published quarterly averages in KZT per USD; an increase means tenge depreciation.

## Assembly and caveats

NPL shares were extracted from downloaded sector reports, using the aggregate
rather than individual banks or borrower subgroups. Reporting dates of 1 April,
1 July, 1 October and 1 January map to the preceding quarter-end. The three series
were aligned chronologically, checked for gaps and duplicates, and stored to two
decimals without interpolation or CPI adjustment.

Brent observations through 2025Q2 were checked against FRED; the supplied
2025Q3–2026Q2 extension was not independently re-fetched. Historical reporting
revisions, an unresolved 2011Q4 NPL vintage discrepancy and the change in NBK's
FX averaging convention in 2021 remain comparability caveats.

The notebooks use the included CSV snapshot; they do not download or re-extract
the original reports. NPL is an outstanding-loan stock ratio, not a rate of new
defaults, and genuine historical jumps have not been removed.
