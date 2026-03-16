# Pandas Fundamentals

## Introduction

Pandas is a powerful Python library for data manipulation and analysis. It provides data structures and operations for working with structured data, making it easy to clean, transform, and analyze datasets. Pandas is built on top of NumPy and is essential for data science and analysis tasks.

## Key Concepts

### Data Structures
- **Series**: One-dimensional labeled array
- **DataFrame**: Two-dimensional labeled data structure
- **Index**: Labels for rows and columns

### Data Import/Export
- CSV files: `pd.read_csv()`, `df.to_csv()`
- Excel files: `pd.read_excel()`, `df.to_excel()`
- JSON: `pd.read_json()`, `df.to_json()`
- SQL databases: `pd.read_sql()`

### Data Manipulation
- Selection: `df['column']`, `df.loc[]`, `df.iloc[]`
- Filtering: Boolean indexing
- Grouping: `df.groupby()`
- Merging: `pd.merge()`, `df.join()`

## Examples

### Creating DataFrames and Series
```python
import pandas as pd
import numpy as np

# Create Series
s = pd.Series([1, 3, 5, 6, 8])
print("Series:")
print(s)
print()

# Create DataFrame from dictionary
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David'],
    'Age': [25, 30, 35, 40],
    'City': ['New York', 'London', 'Paris', 'Tokyo']
}
df = pd.DataFrame(data)
print("DataFrame:")
print(df)
print()

# DataFrame info
print("Shape:", df.shape)
print("Columns:", df.columns.tolist())
print("Index:", df.index.tolist())
```

### Reading Data Files
```python
# Reading CSV (assuming file exists)
# df = pd.read_csv('data.csv')

# Reading with options
# df = pd.read_csv('data.csv', sep=',', header=0, index_col=0)

# Create sample data for demonstration
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'Age': [25, 30, 35, 28, 32],
    'Salary': [50000, 60000, 70000, 55000, 65000],
    'Department': ['HR', 'IT', 'Finance', 'IT', 'HR']
}
df = pd.DataFrame(data)
```

### Data Selection and Indexing
```python
# Select single column
names = df['Name']
print("Names column:")
print(names)
print()

# Select multiple columns
subset = df[['Name', 'Age']]
print("Name and Age:")
print(subset)
print()

# Select rows by position
first_row = df.iloc[0]
print("First row:")
print(first_row)
print()

# Select rows by label
# df.loc[0]  # if index is numeric

# Boolean indexing
it_employees = df[df['Department'] == 'IT']
print("IT Employees:")
print(it_employees)
print()
```

### Data Manipulation
```python
# Add new column
df['Experience'] = [2, 5, 8, 3, 6]
print("Added Experience column:")
print(df)
print()

# Modify existing column
df['Age'] = df['Age'] + 1
print("Ages increased by 1:")
print(df[['Name', 'Age']])
print()

# Drop column
df_dropped = df.drop('Experience', axis=1)
print("Dropped Experience column:")
print(df_dropped)
print()
```

### Filtering and Sorting
```python
# Filter by condition
high_salary = df[df['Salary'] > 60000]
print("High salary employees:")
print(high_salary[['Name', 'Salary']])
print()

# Multiple conditions
it_high_salary = df[(df['Department'] == 'IT') & (df['Salary'] > 55000)]
print("IT employees with high salary:")
print(it_high_salary)
print()

# Sort by column
sorted_df = df.sort_values('Salary', ascending=False)
print("Sorted by salary (descending):")
print(sorted_df[['Name', 'Salary']])
print()
```

### Grouping and Aggregation
```python
# Group by department
dept_groups = df.groupby('Department')
print("Department groups:")
for name, group in dept_groups:
    print(f"\n{name}:")
    print(group)
print()

# Aggregate functions
dept_stats = df.groupby('Department').agg({
    'Salary': ['mean', 'max', 'min'],
    'Age': 'mean'
})
print("Department statistics:")
print(dept_stats)
print()

# Simple aggregation
avg_salary_by_dept = df.groupby('Department')['Salary'].mean()
print("Average salary by department:")
print(avg_salary_by_dept)
print()
```

### Handling Missing Data
```python
# Create DataFrame with missing values
data_with_nan = {
    'A': [1, 2, np.nan, 4],
    'B': [5, np.nan, 7, 8],
    'C': [9, 10, 11, np.nan]
}
df_nan = pd.DataFrame(data_with_nan)
print("DataFrame with NaN:")
print(df_nan)
print()

# Check for missing values
print("Missing values:")
print(df_nan.isnull())
print()

# Drop rows with missing values
df_dropped_na = df_nan.dropna()
print("Dropped NaN rows:")
print(df_dropped_na)
print()

# Fill missing values
df_filled = df_nan.fillna(0)
print("Filled NaN with 0:")
print(df_filled)
print()
```

### Merging and Joining
```python
# Create two DataFrames
df1 = pd.DataFrame({
    'ID': [1, 2, 3, 4],
    'Name': ['Alice', 'Bob', 'Charlie', 'David']
})

df2 = pd.DataFrame({
    'ID': [1, 2, 3, 5],
    'Salary': [50000, 60000, 70000, 80000]
})

print("DataFrame 1:")
print(df1)
print()
print("DataFrame 2:")
print(df2)
print()

# Inner join
merged_inner = pd.merge(df1, df2, on='ID', how='inner')
print("Inner join:")
print(merged_inner)
print()

# Left join
merged_left = pd.merge(df1, df2, on='ID', how='left')
print("Left join:")
print(merged_left)
print()
```

### Data Export
```python
# Export to CSV
df.to_csv('sample_data.csv', index=False)
print("Data exported to CSV")

# Export to Excel (requires openpyxl)
# df.to_excel('sample_data.xlsx', index=False)

# Export to JSON
df.to_json('sample_data.json', orient='records')
print("Data exported to JSON")
```

## Tasks

Create a Python file named `pandas_basics.py` in this folder and implement the following:

1. Create DataFrames from different data sources (dictionaries, lists)
2. Perform data selection and filtering operations
3. Implement grouping and aggregation functions
4. Handle missing data in DataFrames
5. Merge and join multiple DataFrames

## Additional Resources

- Pandas official documentation
- Pandas cheat sheet
- Data manipulation with Pandas