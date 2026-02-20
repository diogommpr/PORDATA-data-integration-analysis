# PORDATA Data Integration and Analysis

## 1. Overview
This project was developed to practice data cleaning, integration, and exploratory data 
analysis using public datasets from PORDATA, Base de Dados de Portugal Contemporâneo. 

The main goal was to apply the data wrangling techniques learned from "Python for Data Analysis, 
Data Wrangling with pandas, NumPy & Jupyter"*, by *Wes McKinney. To that end, I performed the following steps: 
downloading multiple datasets, cleaning and harmonizing them, merging them into a single unified dataset, and 
conducting exploratory analyses to extract insights at the municipal level.


## 2. Data Sources
The datasets were obtained from publicly available PORDATA databases (https://www.pordata.pt/municipios). 

The following datasets were used: 
- Resident population by sex and age group 
- Crude mortality rate 
- Infant mortality rate 
- Crude birth rate 
- Unemployment by age group 
- Average monthly income 
- Number of pharmacies
- Crimes by category 
- Crude nuptiality rate
- Crude divorce rate

> **Note:** Due to GitHub's file size limitations, the resident population dataset (population_by_age_sex.csv)
> contains only data from 2006 onwards. Since this dataset is used as the basis for merging the consolidated datasets,
> all of them are limited to the period 2006–2024. To obtain consolidated datasets with all available years, download
> the full resident population dataset and replace the file currently provided in the /raw folder before running the pipeline.

## 3. Generated Datasets
-----------------
As part of this project, four final datasets were created from the cleaned and merged raw data:

1. **Merged dataset (all years, one row per municipality)**: contains a single row for each municipality, aggregating all variables.
2. **Merged detailed dataset (all years, multiple rows per crime type)**: contains multiple rows per municipality, separated by type of crime.
3. **2024-specific dataset (one row per municipality)**: subset for the year 2024, with one row per municipality.
4. **2024-specific detailed dataset (multiple rows per crime type)**: subset for the year 2024, with multiple rows per municipality depending on the crime type.

These datasets are saved in the `data/processed/` folder. The statistical analyses were run on the **2024-specific dataset (one row per municipality)**.

## 4. Methodology
The project followed these main steps:

### 4.1. Download of Raw Datasets
- CSV datasets from PORDATA

### 4.2 Import Libraries
- `pandas`, `re`, `unidecode`
- Standard Python data science libraries
  
### 4.3 Load Data
- Each CSV from PORDATA is loaded into a separate DataFrame

### 4.4 Data Cleaning
- Standardize column names: Portuguese to English, removal of leading/trailing spaces and accents, etc.)
- Drop unnecessary columns
- Rename columns to descriptive names: e.g., “Valor” to “Population”, “Deaths_per_1000”, etc.)
- Normalize categorical values 
- Filter only Municipality-level rows: “Level” = “Municipality”
- Handle datasets with multiple breakdowns by keeping total values only:
  - Population: totals across age and sex.
  - Unemployment: total unemployed population.
- Crime data were treated in two ways:
  - An aggregated version with total crimes per municipality and year.
  - A detailed version retaining multiple rows per crime type.

### 4.5 Missing Data Imputation
- Some datasets lack values for 2024
- Values from 2023 were carried to 2024

### 4.6 Merge Datasets
- Main merge (“merged_df”): one row per Municipality per Year
- Detailed merge (“merged_detailed_df”: keeps multiple rows per “Crime_type”
- Merge keys: “Year” and “Municipality”
- Check for missing values

### 4.7 Export Cleaned/Merged Data
- CSVs saved to “data/processed”:
    - “merged_dataset.csv” → standard consolidated dataset
    - “merged_dataset_detailed.csv” → detailed dataset (multiple rows per crime type)
    - “municipal_data_2024.csv” → 2024 only, one row per municipality
    - “municipal_data_2024_detailed.csv” → 2024 only, detailed crime rows

### 4.8 **Exploratory data analysis**:
   - Top/bottom municipalities by key variables.
   - Normalized rates (per 1,000 inhabitants, etc.).
   - Salary quartile analysis and ANOVA tests.
   - Correlation analysis between variables.

## 5. Key Design Choices
- Municipality-level analysis was chosen to ensure comparability across datasets and years.
- When datasets included multiple breakdowns (e.g., age, sex, crime category), total values were retained to simplify integration.
- Crime data were preserved in both aggregated and detailed formats to support different analytical perspectives.
- Analyses were performed on the 2024 simplified dataset (one row per municipality) to use the most up-to-date data. 

## 6. Key Findings 
- Significant variation exists across municipalities in demographic, economic, and social indicators.
- Normalizing variables by population size was essential to avoid misleading comparisons between municipalities.
- Statistically significant differences were observed across income quartiles for all analyzed variables; the highest-income quartile exhibited lower mortality rates and higher birth and divorce rates compared to other quartiles.
- Moderate correlations were observed between
  - Mortality rate and birth rate (r = -0.54)
  - Mortality rate and divorce rate (r = -0.42)
  - Mortality rate and number of pharmacies per 10,000 inhabitants (r = 0.56)
  - Average income and birth rate (r = 0.40)

## 7. Tools and Technologies
- Python
- pandas
- matplotlib / seaborn
- sciPy
- Jupyter Notebook

## 8. Repository Structure
```
data/
  raw/         # Original CSVs downloaded from PORDATA
  processed/   # Cleaned and merged datasets generated by 01_data_cleaning.ipynb
notebooks/
  01_data_cleaning.ipynb   # Data cleaning, merging, and CSV generation
  02_analysis.ipynb        # Exploratory data analysis and visualizations
README.md
```

## 9. How to Reproduce
1. Clone the repository.
2. Open `01_data_cleaning.ipynb` and run all cells:
   - Cleaned datasets will be saved in `data/processed/`.
3. Open `02_analysis.ipynb` and run all cells:
   - Analyses and plots will be generated from the processed CSVs.
4. All paths are **relative**, so the notebooks can be run without changing absolute paths.
