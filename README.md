# ETL Midterm Project
## Project Overview
This project demonstrates a complete ETL (Extract, Transform, Load) using Python and Pandas. It processes raw and incremental sales data, applies transformation techniques such as cleaning, enrichment, and categorization, and saves the results for analysis.

## Data Source
The project uses two CSV files:

* raw_data.csv: Contains sales records from 2023 to 2024

* incremental_data.csv: Contains new sales records to be merged, from 2025

These files were generated using Mockaroo with custom schemas to simulate realistic business data.

## ETL Phases
## i. etl_extract.ipynb
### Loading the Data
I loaded both raw_data.csv and incremental_data.csv from the data/ directory using pandas.

### Previewing the Data
.head() is used then to inspect the first few rows of each dataset and understand their structure.

### Inspecting Metadata
.info() and .describe() help identify column types, missing values, and statistical summaries such as mean, min among others .

### Data Quality Checks that were performed:
* Missing values were counted per column

* Duplicate rows were identified

* Column types were validated

### They were then both merged,

Merged output: data/merged_data.csv

## ii. etl_transform.ipynb
A series of transformations to both the full and incremental datasets were done to prepare them for analysis.

### a. Cleaning
### Duplicate Removal
- Removed duplicate rows using drop_duplicates():
```
df.drop_duplicates(inplace=True)
```

### Missing Value Imputation
- Numerical columns: Filled missing values with the column median:
```
df[col] = df[col].fillna(df[col].median())
```

### b. Enrichment
Dropped columns with:
- Only null values
- Low variance (only one unique value)
```
drop_cols = [col for col in df.columns if df[col].nunique() <= 1 or df[col].isnull().all()]
df = df.drop(columns=drop_cols)
```

### c. Structural Formatting
Date Conversion
- Converted all columns containing "date" in their name to datetime format using error coercion:
```
df[col] = pd.to_datetime(df[col], errors='coerce')
```
### Saving Transformed Data Transformed outputs are saved to the transformed/ folder:
transformed/transformed_full.csv
transformed/transformed_incremental.csv

## TOOLS USED
VS Code
Pandas
Jupyter Notebook
## HOW TO RUN THE PROJECT
- Clone or download the project folder to your local machine.

- Open the project in Jupyter Notebook or JupyterLab.

- Place raw_data.csv and incremental_data.csv inside the data/ folder.

- Open etl_extract.ipynb and run all cells to clean and merge the datasets.

- Open etl_transform.ipynb and run all cells to apply transformations and save outputs

### Transformed files will appear in the transformed/ folder

## Load & Verification
## etl_loaded.ipynb
For the Load phase of my ETL pipeline, I chose to use SQLite3 to store the transformed datasets. I loaded both transformed_full.csv and transformed_incremental.csv into a SQLite database file named full_data.db
A snippet of the code
```
pd.read_sql('SELECT * FROM full_data LIMIT 5', conn)
```
Issues & Solutions:
Initially, I considered saving the data in Parquet format as well, but ran into an ImportError due to missing dependencies (pyarrow or fastparquet). I resolved this by installing pyarrow via pip:
``` 
pip install pyarrow
```

## FOLDER STRUCTURE
```
et_exam_Nelisa_347/
├── data/
│   ├── raw_data.csv
│   ├── incremental_data.csv
│   ├── raw_data_clean.csv
│   ├── incremental_data_clean.csv
│   └── merged_data.csv
├── transformed/
│   ├── transformed_full.csv
│   └── transformed_incremental.csv
├── loaded/
│   ├── full_data.db
│   ├── full_data.parquet
│   ├── incremental_data.parquet
├── etl_extract.ipynb
├── etl_transform.ipynb
├── etl_load.ipynb
├── README.md
└── .gitignore