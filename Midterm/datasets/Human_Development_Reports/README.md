# Human Development Reports — Composite Indices

## Source: United Nations Development Programme (HDR) composite indices https://hdr.undp.org/data-center/documentation-and-downloads

### Original files:
- `HDR25_Composite_indices_complete_time_series.csv` — main wide table with short-name column headers and year-suffixed fields.
- `HDR25_Composite_indices_metadata.xlsx` (sheet: `codebook`) — metadata mapping short names to full names, `Time series`, and `Short name`/`Full name` fields.


### Files I created for you:
- `human_development.csv` — long/tidy country-level output produced by the notebook (years in a `Year` column).
- `human_development_aggregates.csv` — aggregates / regional rows extracted from the time series.
- `human_development_reports.ipynb` — notebook that performs the load, metadata-driven renaming, melting to long format, and writes the CSVs.

## Key DataFrames (what they contain & when to use)
- `df` (raw): original wide table read from `HDR25_Composite_indices_complete_time_series.csv`. Contains columns like `iso3` plus short-name-year columns.
- `meta`: metadata read from `HDR25_Composite_indices_metadata.xlsx` (`codebook` sheet). Use to map short names to full descriptive names and to detect which fields contain time-series ranges.
- `df_renamed`: a version of `df` with short-name columns replaced by full names that include the year (keeps the year suffix so columns can be melted reliably).
- `df_long`: long/tidy dataframe produced by melting/wide_to_long with columns `iso3`, `Year`, and one column per measured field. Use for plotting and Tableau.
- `aggregates`: rows extracted for aggregates (rows where `iso3` contains a dot or other aggregate marker); useful for regional summaries or non-country series.

## Plotting & analysis tips
- Use `df_long` for faceting or grouping in Seaborn/Plotly Express and for Tableau imports.
- Use `aggregates` when you need region-level or composite summaries; filter `df_long` to remove aggregates for pure country-level analyses.

## Notes & caveats
- The metadata sheet indicates which short-name fields represent time series via the `Time series` column (e.g., ranges like `1990-2020`). The notebook detects those and melts them into the `Year` column.
- Encoding: the CSV is read with `latin-1` in the notebook to avoid encoding issues.

## Data dictionary (key columns)
- `iso3` (string): ISO3 country code used to identify countries and aggregates.
- `Year` (int): calendar year exposed by the melting step.
- measured fields (various numeric columns): the notebook renames short-name-year columns into `Full name: YYYY` and then melts them; after `wide_to_long` the measured fields become separate columns whose names are the values from the metadata `Full name` field.

### All measured fields (from the HDR codebook `Full name`)
- ISO3
- HDR Country Name
- Human Development Groups
- UNDP Developeing Regions
- HDI
- HDI Rank
- Human Development Index (value)
- Life Expectancy at Birth (years)
- Expected Years of Schooling (years)
- Mean Years of Schooling (years)
- Gross National Income Per Capita (2021 PPP$)
- GDI
- GDI Group
- Gender Development Index (value)
- HDI female (value)
- Life Expectancy at Birth, female (years)
- Expected Years of Schooling, female (years)
- Mean Years of Schooling, female (years)
- Gross National Income Per Capita, female (2021 PPP$)
- HDI male (value)
- Life Expectancy at Birth, male (years)
- Expected Years of Schooling, male (years)
- Mean Years of Schooling, male (years)
- Gross National Income Per Capita, male (2021 PPP$)
- IHDI
- Inequality-adjusted Human Development Index (value)
- Coefficient of human inequality
- Overall loss (%)
- Inequality in life expectancy
- Inequality in eduation
- Inequality in income
- GII
- GII Rank
- Gender Inequality Index (value)
- Maternal Mortality Ratio (deaths per 100,000 live births)
- Adolescent Birth Rate (births per 1,000 women ages 15-19)
- Population with at least some secondary education, female (% ages 25 and older)
- Population with at least some secondary education, male (% ages 25 and older)
- Share of seats in parliament, female (% held by women)
- Share of seats in parliament, male (% held by men)
- Labour force participation rate, female (% ages 15 and older)
- Labour force participation rate, male (% ages 15 and older)
- PHDI
- Difference from HDI rank
- Planetary pressures–adjusted Human Development Index (value)
- Difference from HDI value (%)
- Carbon dioxide emissions per capita (production) (tonnes)
- Material footprint per capita (tonnes)
- Additional Indicator
- Population, total (millions)

Metadata (`HDR25_Composite_indices_metadata.xlsx`, sheet `codebook`) columns of interest:
- `Short name` (string): short identifier used as the column stub in the wide CSV.
- `Full name` (string): human-readable description used in `df_renamed` and in final `df_long` column labels.
- `Time series` (string): indicates whether the field contains a range of years (e.g., `1990-2020`) and is used by the notebook to detect which fields to melt.

Precomputed CSVs and their columns:
- `human_development.csv`: `iso3,Year,<measured fields derived from Full name>` (country-level tidy).
- `human_development_aggregates.csv`: same columns, but rows correspond to aggregates/regions.

## Glossary (acronyms)
- `HDI`: Human Development Index — composite index measuring average achievement in key dimensions of human development: health, education and standard of living.
- `GDI`: Gender Development Index — compares female and male HDI values to measure gender gaps in human development.
- `IHDI`: Inequality-adjusted Human Development Index — HDI adjusted for inequality in distribution of each dimension across the population.
- `GII`: Gender Inequality Index — reflects gender-based disadvantage in reproductive health, empowerment and labor market participation.
- `PHDI`: Planetary pressures–adjusted Human Development Index — HDI adjusted for environmental pressures (e.g., carbon and material footprints).


