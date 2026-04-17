# World Bank GDP Dataset
## Source: https://data.worldbank.org/indicator/NY.GDP.MKTP.CD

### Original files:
- `API_NY.GDP.MKTP.CD_DS2_en_csv_v2_133326.csv` — raw World Bank CSV (two header lines then a table; years are columns).
- `Metadata_Indicator_API_NY.GDP.MKTP.CD_DS2_en_csv_v2_133326.csv` — indicator definition and source.
- `Metadata_Country_API_NY.GDP.MKTP.CD_DS2_en_csv_v2_133326.csv` — country lookup with `Country Code`, `Region`, `IncomeGroup`, `SpecialNotes`, `TableName`.

### Files I created for you:
- `gdp_by_region.csv`, `gdp_by_country.csv`, `gdp_by_income_group.csv` — tidy, ready-to-use CSVs (columns like `Region,Year,GDP ($USD)`).
- `gdp.ipynb` — example notebook that loads and reshapes the data.

Key DataFrames (what they contain & when to use)
- `df` (raw): columns include `Country Name`, `Country Code`, `Indicator Name`, `Indicator Code`, and one column per year (1960–2025). Use only to inspect raw layout.
- `gdp_by_entry` (long/tidy): columns `Country Name`, `Country Code`, `Year`, `GDP ($USD)`. Use for Tableau, Seaborn, and Plotly Express.
- `gdp_meta`: `gdp_by_entry` merged with country metadata. Adds `Region`, `IncomeGroup`, `Country` (from `TableName`). Use for grouping and joins.
- `by_region`: tidy grouped table with `Region`, `Year`, `GDP ($USD)` (sum per region-year). Ready for Tableau and long-format plotting.
- `by_region_pivot`: wide pivot (index `Year`, columns = regions). Convenient for quick multi-series plotting in pandas/Matplotlib/Plotly graph_objects.
- `by_income` / `by_income_pivot`: same as region but grouped by income group.
- `top_countries_latest`: top-N countries by GDP for the most recent year present in the data.

## Plotting tips
- Matplotlib / pandas / Plotly graph_objects: use `by_region_pivot` (wide) for quick multiple-series plots. 
- Seaborn / Plotly Express / Tableau: use `by_region` (long/tidy)

## Important notes
- Values are nominal (current US$).
- Some rows are aggregates (codes like `WLD`, `EUU`) — filter these out when you only want country-level data.
- Check `SpecialNotes` in `Metadata_Country...csv` for country-specific caveats (rebasing, fiscal years, exchange-rate notes).

## Data dictionary (key columns)
- `Country Name` (string): official country or territory name as provided by the World Bank.
- `Country Code` (string): ISO3 country code.
- `Indicator Name` (string): indicator description; here `GDP (current US$)`.
- `Indicator Code` (string): `NY.GDP.MKTP.CD`.
- `Year` (int): calendar year when data was collected (used in long/tidy tables).
- `GDP ($USD)` (float): GDP in current U.S. dollars. Missing where data is not reported.
- `Region` (string): region label from `Metadata_Country...csv` (used to group by region).
- `IncomeGroup` / `Income Group` (string): income classification from metadata.
- `TableName` / `Country` (string): human-readable country name from metadata when present.

Precomputed CSVs and their columns:
- `gdp_by_region.csv`: `Region,Year,GDP ($USD)` (long/tidy).
- `gdp_by_country.csv`: `Country Name,Country Code,Year,GDP ($USD)` (country-level tidy).
- `gdp_by_income_group.csv`: `Income Group,Year,GDP ($USD)` (long/tidy by income group).
