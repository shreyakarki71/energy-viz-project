# World Bank Life Expectancy at Birth Dataset

## Source: https://data.worldbank.org/indicator/SP.DYN.LE00.IN

### Original files
- `API_SP.DYN.LE00.IN_DS2_en_csv_v2_128.csv` — raw World Bank CSV (includes two header lines: data source and last-updated lines, then the table header).
- `Metadata_Country_API_SP.DYN.LE00.IN_DS2_en_csv_v2_128.csv` — country lookup with `Country Code`, `Region`, `IncomeGroup`, `SpecialNotes`, `TableName`.

### Files I created for you
- `life_expectancy_by_region.csv`, `life_expectancy_by_country.csv`, `life_expectancy_by_income_group.csv` — convenience, precomputed CSVs (long/tidy format) produced from the main file.
- `life_expectancy.ipynb` — example notebook that reads and reshapes the data.

## Key DataFrames (what they contain & when to use)
- `df` (raw): the original table with `Country Name`, `Country Code`, `Indicator Name`, `Indicator Code`, and year columns (e.g., 1960–2024). Use to inspect raw layout.
- `life_expectancy` (long/tidy): melted version with columns `Country Name`, `Country Code`, `Year`, `Life expectancy (years)`. Use for Tableau, Seaborn, and Plotly Express.
- `life_expectancy_meta`: `life_expectancy` merged with country metadata. The notebook renames `IncomeGroup` to `Income Group` and adds `Country` (from `TableName`). Use this for grouped analyses and joins.
- `by_region`: long/tidy grouped table with `Region`, `Year`, `Life expectancy (years)` (the notebook creates this via `groupby` and currently uses `.sum()`—for averages across countries use `.mean()` or a population-weighted mean depending on the analysis).
- `by_region_pivot`: wide pivot (index `Year`, columns = regions). Convenient for quick multi-series plotting with pandas/Matplotlib/Plotly graph_objects.
- `by_income` / `by_income_pivot`: same as region but grouped by income group (the notebook computes mean for income groups).
- `by_country`: country-level tidy dataset (the notebook drops rows without `Region` to remove aggregates) saved for per-country work.

## Plotting tips
- Matplotlib / pandas / Plotly graph_objects: use `by_region_pivot` (wide) to plot multiple regions quickly.
- Seaborn / Plotly Express / Tableau: use `by_region` (long/tidy) for faceting and color-by-category workflows.

## Notes & caveats
- Values are life expectancy in years; missing values are common for early years and small territories.
- Some rows are regional/income-group aggregates (check codes like `WLD` or `EUU`); filter out rows where `Region` is missing when you want country-only data.
- Consult `SpecialNotes` in `Metadata_Country...csv` for country-specific caveats and reporting changes.

## Data dictionary (key columns)
- `Country Name` (string): official country or territory name.
- `Country Code` (string): ISO3 country code.
- `Indicator Name` (string): indicator description; here `Life expectancy at birth, total (years)`.
- `Indicator Code` (string): `SP.DYN.LE00.IN`.
- `Year` (int): calendar year (used in long/tidy tables).
- `Life expectancy (years)` (float): life expectancy at birth in years. Missing where not reported.
- `Region` (string): region label from `Metadata_Country...csv`.
- `IncomeGroup` / `Income Group` (string): income classification from metadata.

Precomputed CSVs and their columns:
- `life_expectancy_by_region.csv`: `Region,Year,Life expectancy (years)` (long/tidy).
- `life_expectancy_by_country.csv`: `Country Name,Country Code,Year,Life expectancy (years)` (country-level tidy).
- `life_expectancy_by_income_group.csv`: `Income Group,Year,Life expectancy (years)` (long/tidy by income group).
