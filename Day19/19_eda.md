# Exploratory Data Analysis (EDA)

## Introduction

Exploratory Data Analysis (EDA) is the process of analyzing and visualizing data to understand its main characteristics, uncover patterns, and identify relationships between variables. EDA helps data scientists understand the data before applying more complex analytical techniques or machine learning models.

## Key Concepts

### EDA Goals
- **Understand data structure**: Types, shapes, distributions
- **Identify patterns**: Trends, correlations, anomalies
- **Detect issues**: Missing values, outliers, inconsistencies
- **Generate hypotheses**: Form questions for further analysis
- **Guide preprocessing**: Inform data cleaning and feature engineering

### EDA Techniques
- **Univariate analysis**: Single variable analysis
- **Bivariate analysis**: Two variable relationships
- **Multivariate analysis**: Multiple variable relationships
- **Statistical summaries**: Mean, median, mode, variance
- **Data visualization**: Charts, plots, graphs

### Common Visualizations
- Histograms: Distribution of continuous variables
- Box plots: Quartiles and outliers
- Scatter plots: Relationships between variables
- Bar charts: Categorical data frequencies
- Heatmaps: Correlation matrices

## Examples

### Data Loading and Initial Exploration
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load sample dataset (using seaborn's built-in dataset)
df = sns.load_dataset('iris')
print("Dataset shape:", df.shape)
print("First 5 rows:")
print(df.head())
print()

# Basic information
print("Data types:")
print(df.dtypes)
print()

print("Summary statistics:")
print(df.describe())
print()

# Check for missing values
print("Missing values:")
print(df.isnull().sum())
print()
```

### Univariate Analysis
```python
# Numerical variables
numerical_cols = ['sepal_length', 'sepal_width', 'petal_length', 'petal_width']

print("Numerical columns analysis:")
for col in numerical_cols:
    print(f"\n{col.upper()}:")
    print(f"  Mean: {df[col].mean():.2f}")
    print(f"  Median: {df[col].median():.2f}")
    print(f"  Std: {df[col].std():.2f}")
    print(f"  Min: {df[col].min():.2f}")
    print(f"  Max: {df[col].max():.2f}")
    print(f"  Skewness: {df[col].skew():.2f}")
print()

# Categorical variables
print("Species distribution:")
print(df['species'].value_counts())
print()

print("Species percentages:")
print(df['species'].value_counts(normalize=True) * 100)
print()
```

### Data Visualization - Univariate
```python
# Set up the plotting style
plt.style.use('seaborn-v0_8')
sns.set_palette("husl")

# Create subplots for numerical variables
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
fig.suptitle('Distribution of Numerical Variables', fontsize=16)

# Histograms
axes[0, 0].hist(df['sepal_length'], bins=20, alpha=0.7, color='skyblue')
axes[0, 0].set_title('Sepal Length Distribution')
axes[0, 0].set_xlabel('Sepal Length')
axes[0, 0].set_ylabel('Frequency')

axes[0, 1].hist(df['sepal_width'], bins=20, alpha=0.7, color='lightgreen')
axes[0, 1].set_title('Sepal Width Distribution')
axes[0, 1].set_xlabel('Sepal Width')
axes[0, 1].set_ylabel('Frequency')

axes[1, 0].hist(df['petal_length'], bins=20, alpha=0.7, color='salmon')
axes[1, 0].set_title('Petal Length Distribution')
axes[1, 0].set_xlabel('Petal Length')
axes[1, 0].set_ylabel('Frequency')

axes[1, 1].hist(df['petal_width'], bins=20, alpha=0.7, color='gold')
axes[1, 1].set_title('Petal Width Distribution')
axes[1, 1].set_xlabel('Petal Width')
axes[1, 1].set_ylabel('Frequency')

plt.tight_layout()
plt.show()
```

### Box Plots for Outlier Detection
```python
# Box plots for all numerical variables
plt.figure(figsize=(12, 8))
df.boxplot(column=numerical_cols)
plt.title('Box Plots of Numerical Variables')
plt.ylabel('Values')
plt.xticks(rotation=45)
plt.show()

# Individual box plots by species
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
fig.suptitle('Box Plots by Species', fontsize=16)

for i, col in enumerate(numerical_cols):
    ax = axes[i//2, i%2]
    df.boxplot(column=col, by='species', ax=ax)
    ax.set_title(f'{col.replace("_", " ").title()} by Species')
    ax.set_xlabel('Species')

plt.tight_layout()
plt.show()
```

### Bivariate Analysis
```python
# Correlation matrix
correlation_matrix = df[numerical_cols].corr()
print("Correlation Matrix:")
print(correlation_matrix)
print()

# Scatter plots
fig, axes = plt.subplots(2, 3, figsize=(15, 10))
fig.suptitle('Scatter Plots of Variable Relationships', fontsize=16)

scatter_pairs = [('sepal_length', 'sepal_width'),
                 ('sepal_length', 'petal_length'),
                 ('sepal_length', 'petal_width'),
                 ('sepal_width', 'petal_length'),
                 ('sepal_width', 'petal_width'),
                 ('petal_length', 'petal_width')]

for i, (x, y) in enumerate(scatter_pairs):
    ax = axes[i//3, i%3]
    ax.scatter(df[x], df[y], alpha=0.6)
    ax.set_xlabel(x.replace('_', ' ').title())
    ax.set_ylabel(y.replace('_', ' ').title())
    ax.set_title(f'{x.replace("_", " ").title()} vs {y.replace("_", " ").title()}')

plt.tight_layout()
plt.show()
```

### Correlation Heatmap
```python
# Correlation heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', center=0,
            square=True, linewidths=0.5)
plt.title('Correlation Heatmap')
plt.show()
```

### Multivariate Analysis
```python
# Pair plot
plt.figure(figsize=(12, 10))
sns.pairplot(df, hue='species', diag_kind='kde', markers=['o', 's', 'D'])
plt.suptitle('Pair Plot of Iris Dataset', y=1.02)
plt.show()

# Violin plots
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
fig.suptitle('Violin Plots by Species', fontsize=16)

for i, col in enumerate(numerical_cols):
    ax = axes[i//2, i%2]
    sns.violinplot(x='species', y=col, data=df, ax=ax)
    ax.set_title(f'{col.replace("_", " ").title()} Distribution by Species')

plt.tight_layout()
plt.show()
```

### Categorical Analysis
```python
# Bar plot for species distribution
plt.figure(figsize=(8, 6))
species_counts = df['species'].value_counts()
species_counts.plot(kind='bar', color=['skyblue', 'lightgreen', 'salmon'])
plt.title('Species Distribution')
plt.xlabel('Species')
plt.ylabel('Count')
plt.xticks(rotation=0)
plt.show()

# Pie chart
plt.figure(figsize=(8, 6))
plt.pie(species_counts, labels=species_counts.index, autopct='%1.1f%%',
        colors=['skyblue', 'lightgreen', 'salmon'])
plt.title('Species Distribution (Pie Chart)')
plt.show()
```

### Statistical Tests
```python
from scipy import stats

# Test for normality
print("Normality tests (Shapiro-Wilk):")
for col in numerical_cols:
    stat, p_value = stats.shapiro(df[col])
    print(f"{col}: p-value = {p_value:.4f} {'(Normal)' if p_value > 0.05 else '(Not Normal)'}")

print()

# ANOVA test (differences between species)
print("ANOVA test results:")
for col in numerical_cols:
    groups = [df[df['species'] == species][col] for species in df['species'].unique()]
    f_stat, p_value = stats.f_oneway(*groups)
    print(f"{col}: F-stat = {f_stat:.2f}, p-value = {p_value:.4f}")
```

### Outlier Detection
```python
# Z-score method
from scipy import stats

print("Outlier detection using Z-score:")
for col in numerical_cols:
    z_scores = np.abs(stats.zscore(df[col]))
    outliers = df[z_scores > 3]
    print(f"{col}: {len(outliers)} outliers detected")

print()

# IQR method
print("Outlier detection using IQR:")
for col in numerical_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    outliers = df[(df[col] < lower_bound) | (df[col] > upper_bound)]
    print(f"{col}: {len(outliers)} outliers detected")
```

## Tasks

Create a Python file named `eda_analysis.py` in this folder and implement the following:

1. Load and explore a dataset (use any available dataset)
2. Perform univariate analysis with appropriate visualizations
3. Conduct bivariate analysis to identify relationships
4. Create correlation analysis and heatmaps
5. Implement outlier detection methods
6. Generate a comprehensive EDA report

## Additional Resources

- Exploratory Data Analysis with Python
- Data visualization best practices
- Statistical analysis for data science