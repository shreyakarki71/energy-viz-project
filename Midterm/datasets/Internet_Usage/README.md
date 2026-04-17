# World Bank Internet Usage Dataset

## Source: https://data.worldbank.org/indicator/IT.NET.USER.ZS

### Original files:
- `API_IT.NET.USER.ZS_DS2_en_csv_v2_21.csv` — raw World Bank CSV (includes two header lines: data source and last-updated lines, then the table header).
- `Metadata_Country_API_IT.NET.USER.ZS_DS2_en_csv_v2_21.csv` — country lookup with `Country Code`, `Region`, `IncomeGroup`, `SpecialNotes`, `TableName`.

### Files I created for you
- `internet_usage_by_region.csv`, `internet_usage_by_country.csv`, `internet_usage_by_income_group.csv` — convenience, precomputed CSVs (long/tidy format) produced from the main file.
- `internet_usage.ipynb` — example notebook that reads and reshapes the data.

## Key DataFrames (what they contain & when to use)
- `df` (raw): original table with `Country Name`, `Country Code`, `Indicator Name`, `Indicator Code`, and year columns (e.g., 1990–2024). Use to inspect raw layout.
- `internet_usage` (long/tidy): melted version with columns `Country Name`, `Country Code`, `Year`, `Internet Usage (% of population)`. Use for Tableau, Seaborn, and Plotly Express.
- `internet_usage_meta`: `internet_usage` merged with country metadata (adds `Region` and `Income Group`). Use for grouped analyses and joins.
- `by_region`: long/tidy grouped table with `Region`, `Year`, `Internet Usage (% of population)` (the notebook computes the mean across countries by region).
- `by_region_pivot`: wide pivot (index `Year`, columns = regions). Convenient for quick multi-series plotting with pandas/Matplotlib/Plotly.
- `by_income` / `by_income_pivot`: same as region but grouped by income group.
- `by_country`: country-level tidy dataset (the notebook drops rows without `Region` to remove aggregates) saved for per-country work.

## Plotting tips
- Matplotlib / pandas / Plotly graph_objects: use `by_region_pivot` (wide) to plot multiple regions quickly.
- Seaborn / Plotly Express / Tableau: use `by_region` (long/tidy) for faceting and color-by-category workflows.

## Notes & caveats
- Values are percentages of the population using the internet. Missing values are common for early years and small territories.
- Country-level series are percentages — do not sum percentages across countries. Use means or population-weighted measures when aggregating.
- Some rows are regional/income-group aggregates (check codes like `WLD` or `EUU`); filter out rows where `Region` is missing when you want country-only data.
- Consult `SpecialNotes` in the country metadata for country-specific caveats and reporting changes.

## Data dictionary (key columns)
- `Country Name` (string): official country or territory name.
- `Country Code` (string): ISO3 country code.
- `Indicator Name` (string): indicator description; here `Internet users (percent of population)`.
- `Indicator Code` (string): `IT.NET.USER.ZS`.
- `Year` (int): calendar year (used in long/tidy tables).
- `Internet Usage (% of population)` (float): percent of the population using the internet. Missing where not reported.
- `Region` (string): region label from `Metadata_Country...csv`.
- `IncomeGroup` / `Income Group` (string): income classification from metadata.

Precomputed CSVs and their columns:
- `internet_usage_by_region.csv`: `Region,Year,Internet Usage (% of population)` (long/tidy).
- `internet_usage_by_country.csv`: `Country Name,Country Code,Year,Internet Usage (% of population)` (country-level tidy).
- `internet_usage_by_income_group.csv`: `Income Group,Year,Internet Usage (% of population)` (long/tidy by income group).
