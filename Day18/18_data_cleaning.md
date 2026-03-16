# Data Cleaning with Pandas

## Introduction

Data cleaning is a crucial step in the data analysis process. Real-world data is often messy, containing missing values, duplicates, inconsistent formatting, and outliers. Pandas provides powerful tools for cleaning and preprocessing data to prepare it for analysis.

## Key Concepts

### Common Data Issues
- **Missing values**: NaN, None, empty strings
- **Duplicates**: Repeated rows or entries
- **Inconsistent data**: Different formats, typos
- **Outliers**: Extreme values that don't fit the pattern
- **Incorrect data types**: Numbers stored as strings

### Data Cleaning Steps
1. **Explore data**: Understand structure and identify issues
2. **Handle missing values**: Remove or impute
3. **Remove duplicates**: Identify and eliminate duplicates
4. **Fix data types**: Convert to appropriate types
5. **Handle outliers**: Detect and treat outliers
6. **Standardize formats**: Ensure consistency

### Pandas Methods for Cleaning
- `isnull()`, `notnull()`: Detect missing values
- `dropna()`: Remove missing values
- `fillna()`: Fill missing values
- `drop_duplicates()`: Remove duplicates
- `astype()`: Change data types

## Examples

### Exploring Data
```python
import pandas as pd
import numpy as np

# Create sample messy dataset
data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva', 'Alice'],
    'Age': [25, np.nan, 35, 28, 32, 25],
    'Salary': [50000, 60000, np.nan, 55000, 65000, 50000],
    'Department': ['HR', 'IT', 'Finance', 'IT', 'HR', 'HR'],
    'Join_Date': ['2020-01-15', '2020-03-20', '2019-11-10', '2021-05-05', '2018-12-01', '2020-01-15']
}

df = pd.DataFrame(data)
print("Original DataFrame:")
print(df)
print()

# Basic info
print("DataFrame info:")
print(df.info())
print()

# Summary statistics
print("Summary statistics:")
print(df.describe())
print()

# Check for missing values
print("Missing values per column:")
print(df.isnull().sum())
print()

# Check for duplicates
print("Duplicate rows:")
print(df[df.duplicated()])
print()
```

### Handling Missing Values
```python
# Detect missing values
print("Missing values:")
print(df.isnull())
print()

# Drop rows with any missing values
df_dropna = df.dropna()
print("After dropping rows with NaN:")
print(df_dropna)
print()

# Fill missing values with mean (for numeric columns)
df_filled = df.copy()
df_filled['Age'] = df_filled['Age'].fillna(df_filled['Age'].mean())
df_filled['Salary'] = df_filled['Salary'].fillna(df_filled['Salary'].median())
print("After filling missing values:")
print(df_filled)
print()

# Forward fill and backward fill
df_ffill = df.fillna(method='ffill')
print("Forward fill:")
print(df_ffill)
print()

df_bfill = df.fillna(method='bfill')
print("Backward fill:")
print(df_bfill)
print()
```

### Removing Duplicates
```python
# Check for duplicates
print("Number of duplicate rows:", df.duplicated().sum())
print()

# Remove duplicates (keeps first occurrence)
df_no_duplicates = df.drop_duplicates()
print("After removing duplicates:")
print(df_no_duplicates)
print()

# Remove duplicates based on specific columns
df_no_dup_names = df.drop_duplicates(subset=['Name'])
print("Unique names only:")
print(df_no_dup_names)
print()
```

### Data Type Conversion
```python
# Check current data types
print("Current data types:")
print(df.dtypes)
print()

# Convert to appropriate types
df_clean = df.copy()
df_clean['Age'] = df_clean['Age'].astype('Int64')  # Nullable integer
df_clean['Salary'] = df_clean['Salary'].astype('Int64')
df_clean['Join_Date'] = pd.to_datetime(df_clean['Join_Date'])
print("After type conversion:")
print(df_clean.dtypes)
print()
print(df_clean)
print()
```

### Handling Outliers
```python
# Create dataset with outliers
data_outliers = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'Salary': [50000, 60000, 2000000, 55000, 65000]  # 2000000 is outlier
}

df_outliers = pd.DataFrame(data_outliers)
print("Data with potential outliers:")
print(df_outliers)
print()

# Detect outliers using IQR method
Q1 = df_outliers['Salary'].quantile(0.25)
Q3 = df_outliers['Salary'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers = df_outliers[(df_outliers['Salary'] < lower_bound) | (df_outliers['Salary'] > upper_bound)]
print("Outliers detected:")
print(outliers)
print()

# Remove outliers
df_no_outliers = df_outliers[(df_outliers['Salary'] >= lower_bound) & (df_outliers['Salary'] <= upper_bound)]
print("After removing outliers:")
print(df_no_outliers)
print()
```

### String Cleaning
```python
# Create messy string data
string_data = {
    'Name': ['  Alice  ', 'BOB', ' charlie ', 'David', 'EVA'],
    'Email': ['alice@email.com', 'bob@GMAIL.COM', 'charlie@test.com', 'david@company.org', 'eva@MAIL.COM']
}

df_strings = pd.DataFrame(string_data)
print("Messy string data:")
print(df_strings)
print()

# Clean strings
df_strings['Name'] = df_strings['Name'].str.strip().str.title()
df_strings['Email'] = df_strings['Email'].str.lower()
print("Cleaned strings:")
print(df_strings)
print()
```

### Data Validation
```python
# Create validation rules
def validate_age(age):
    return 18 <= age <= 65

def validate_salary(salary):
    return 30000 <= salary <= 150000

# Apply validation
df_validation = df_filled.copy()
df_validation['Valid_Age'] = df_validation['Age'].apply(validate_age)
df_validation['Valid_Salary'] = df_validation['Salary'].apply(validate_salary)
print("Data validation:")
print(df_validation[['Name', 'Age', 'Valid_Age', 'Salary', 'Valid_Salary']])
print()

# Filter valid records
valid_records = df_validation[df_validation['Valid_Age'] & df_validation['Valid_Salary']]
print("Valid records only:")
print(valid_records)
print()
```

### Comprehensive Cleaning Pipeline
```python
def clean_dataset(df):
    """Comprehensive data cleaning function"""
    df_clean = df.copy()
    
    # 1. Handle missing values
    numeric_columns = df_clean.select_dtypes(include=[np.number]).columns
    for col in numeric_columns:
        df_clean[col] = df_clean[col].fillna(df_clean[col].median())
    
    # 2. Remove duplicates
    df_clean = df_clean.drop_duplicates()
    
    # 3. Convert data types
    if 'Join_Date' in df_clean.columns:
        df_clean['Join_Date'] = pd.to_datetime(df_clean['Join_Date'], errors='coerce')
    
    # 4. Clean strings
    string_columns = df_clean.select_dtypes(include=['object']).columns
    for col in string_columns:
        df_clean[col] = df_clean[col].str.strip().str.title()
    
    return df_clean

# Apply cleaning pipeline
df_cleaned = clean_dataset(df)
print("Final cleaned dataset:")
print(df_cleaned)
print()

print("Cleaning summary:")
print(f"Original shape: {df.shape}")
print(f"Cleaned shape: {df_cleaned.shape}")
print(f"Missing values remaining: {df_cleaned.isnull().sum().sum()}")
```

## Tasks

Create a Python file named `data_cleaning.py` in this folder and implement the following:

1. Load a dataset with various data quality issues
2. Implement missing value handling strategies
3. Remove duplicate entries
4. Convert data types appropriately
5. Detect and handle outliers
6. Create a comprehensive data cleaning pipeline

## Additional Resources

- Pandas data cleaning documentation
- Data preprocessing best practices
- Handling missing data strategies