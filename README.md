# Beyond GDP: Human Development and Capability Analysis

Is GDP alone enough to explain how developed and how happy a country is, or do
health, education, freedom, social support and governance matter just as much?

This project brings together four global data sources for the year 2023, builds a
single country-level dataset, and studies the relationship between income and other
dimensions of human development. The framing is inspired by Amartya Sen's capability
approach, which argues that development should be judged by what people are actually
able to do and be, not by income alone.

## Research question

Is GDP alone sufficient to explain human development and well-being, or do
education, health, freedom, social support and governance play an equally important
role?

## Data sources

All data is for the 2023 reference year.

- UNDP Human Development Report 2025 — HDI, life expectancy, expected and mean years
  of schooling, GNI per capita
- World Happiness Report — happiness score and factor contributions (social support,
  freedom, generosity, perceptions of corruption)
- World Bank — GDP per capita, health expenditure, unemployment, inflation, population
- Transparency International — Corruption Perceptions Index (CPI) 2025

The raw files are not included in this repository because of their size and the
data providers' terms. Each source is freely available from the links above.

## Project structure

```
beyond-gdp-capability-analysis/
├── data/
│   └── processed/
│       ├── master_dataset.csv        cleaned, merged dataset (130 countries)
│       └── capability_index.csv      the experimental capability index
├── notebooks/
│   ├── 01_data_understanding.ipynb   load, clean and merge the four sources
│   ├── 02_sql_analysis.ipynb         SQLite database and SQL analysis
│   ├── 03_eda.ipynb                  exploratory charts
│   ├── 04_statistics.ipynb           correlation, regression, hypothesis test
│   ├── 05_clustering.ipynb           KMeans clustering of countries
│   ├── 06_outliers.ipynb             outlier detection with z-scores
│   └── 07_capability_index.ipynb     building and testing the capability index
├── requirements.txt
└── README.md
```

## Methodology

1. Data cleaning and merging. The four sources use different formats and country
   names. Names were standardised to ISO3 codes, regional aggregates were removed,
   and the sources were merged with an inner join. Ten countries with missing values
   (mostly inflation in crisis economies) were dropped, leaving 130 countries with
   complete data.
2. SQL analysis. The data was loaded into a SQLite database split into four tables
   and queried with joins, grouping, window functions and a view.
3. Exploratory data analysis. Histograms, scatter plots, a correlation heatmap, a
   boxplot and bar charts to see the patterns.
4. Statistics. Pearson and Spearman correlation, a multiple linear regression
   explaining HDI, and a t-test comparing happiness across development levels.
5. Clustering. KMeans grouped countries into three development clusters, with the
   number of clusters chosen using the elbow method and silhouette score.
6. Outlier detection. Z-scores flagged countries that are extreme on any measure.
7. Capability index. An experimental index built from education, health, freedom and
   social support (income deliberately excluded), then compared against GDP, HDI and
   happiness.

## Key findings

- Income and development are linked, but the link is not a straight line. GDP and HDI
  have a Pearson correlation of 0.71, but a Spearman correlation of 0.98. Richer
  countries are almost always higher on HDI, but the extra HDI gained per extra
  dollar shrinks sharply as countries get rich.
- In a regression explaining HDI from factors outside its own formula, GDP was not a
  significant predictor once happiness and governance were included, while happiness
  and the corruption score were significant.
- High-HDI countries (average happiness 6.46) are significantly happier than
  lower-HDI countries (average happiness 4.88), confirmed by a t-test.
- Clustering split countries into high, middle and low development groups. Income
  separated the groups the most (about 36 times difference), while happiness differed
  far less.
- The capability index (built without income) correlates 0.94 with HDI and 0.86 with
  happiness, but only 0.67 with GDP. Measuring development through people's
  capabilities looks much more like HDI and well-being than like raw income.

## Limitations

- The analysis is cross-sectional (one year, 2023), so it shows associations, not
  causes, and no trends over time.
- The happiness factor columns are "explained by" contributions from the report's
  model, not raw survey values.
- Some predictors overlap, which makes individual regression coefficients harder to
  interpret.
- The capability index is an experimental framework to illustrate Sen's idea, not a
  replacement for HDI.

## How to run

```
pip install -r requirements.txt
jupyter notebook
```

Open the notebooks in order (01 to 07). Notebooks 02 onward use the cleaned dataset
in data/processed.

## Tools used

Python (pandas, numpy, matplotlib, seaborn, scipy, statsmodels, scikit-learn),
SQL (SQLite), Jupyter Notebook.
## Author

Ashutosh Jayant

- GitHub: https://github.com/jayantashutosh-rgb
- LinkedIn: https://www.linkedin.com/in/ashutosh-jayant-954a54a0/
