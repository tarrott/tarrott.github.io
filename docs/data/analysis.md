# Data Analysis

---
## NumPy


---
## Pandas

### Notes
Import pandas as pd
`df = pd.read_csv(“file.csv”)`

Outputs: (rows, columns)
`df.shape`

Outputs: first/last 5 rows & header
`df.head(5) or df.tail`

Check for null values
`df.isnull().values.any()`

Data fram correlation function
`df.corr()`

Delete column from data frame
`del df(‘column_name’)`

Mold data (change True/False values to 1 or 0)
`column_map = { True: 1, False: 0 }`
`df[‘column_name’] = df[‘column_name’].map(column_map)`

Creating column ratios (True/False ratio)
`num_true = len(df.loc[df[‘column_name’] == True])`
`num_false = len(df.loc[df[‘column_name’] == False])`

Count:
`len(dataframe)`

Count group by distinct:
`df[COLUMN.value_counts()`

Count null values
`df['COLUMN'].isna().sum()`
`len(df) - df['COLUMN'].count()`

Count distict values, use nunique:
`df['hID'].nunique()`

Count only non-null values, use count:
`df['hID'].count()`

Count total values including null values, use size attribute:
`df['hID'].size`

- https://davidhamann.de/2017/06/26/pandas-select-elements-by-string/
- https://proview-demo.nonprod.caqh.org/DirectAssure/api/POPracticeLocation/MatchReport/6053_20190606_144049


---
## Visualization
