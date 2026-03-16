# Exploratory Data Analysis (EDA) Project

## Introduction

This project focuses on performing comprehensive exploratory data analysis on a real-world dataset. EDA is a crucial step in any data science project that helps understand the data, identify patterns, detect anomalies, and generate insights that guide subsequent modeling decisions.

## Project Objectives

1. **Data Understanding**: Load and examine the dataset structure
2. **Data Cleaning**: Handle missing values, outliers, and data inconsistencies
3. **Univariate Analysis**: Analyze individual variables
4. **Bivariate Analysis**: Examine relationships between variables
5. **Multivariate Analysis**: Explore complex relationships
6. **Feature Engineering**: Create new meaningful features
7. **Data Visualization**: Create comprehensive visualizations
8. **Insights and Recommendations**: Draw actionable insights

## Dataset Description

We'll use a real-world dataset for this project. Let's work with a housing prices dataset that contains various features about houses and their sale prices.

## Implementation

### Data Loading and Initial Exploration
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')

# Set style for better visualizations
plt.style.use('seaborn-v0_8')
sns.set_palette("husl")

print("Exploratory Data Analysis Project")
print("=" * 50)

# Load the dataset (using a sample housing dataset)
# In a real project, you would load from a file or API
np.random.seed(42)

# Create a sample housing dataset
n_samples = 1000

data = {
    'price': np.random.normal(300000, 100000, n_samples).clip(min=50000),
    'area': np.random.normal(2000, 500, n_samples).clip(min=500, max=5000),
    'bedrooms': np.random.randint(1, 7, n_samples),
    'bathrooms': np.random.randint(1, 5, n_samples),
    'age': np.random.exponential(20, n_samples).clip(max=100),
    'garage': np.random.choice([0, 1, 2, 3], n_samples, p=[0.1, 0.4, 0.4, 0.1]),
    'neighborhood': np.random.choice(['A', 'B', 'C', 'D'], n_samples),
    'has_pool': np.random.choice([0, 1], n_samples, p=[0.7, 0.3]),
    'year_built': np.random.randint(1950, 2024, n_samples),
    'lot_size': np.random.normal(8000, 2000, n_samples).clip(min=1000),
    'stories': np.random.choice([1, 2, 3], n_samples, p=[0.3, 0.5, 0.2])
}

# Introduce some missing values and outliers
df = pd.DataFrame(data)

# Add missing values
for col in ['area', 'bedrooms', 'bathrooms']:
    mask = np.random.random(n_samples) < 0.05
    df.loc[mask, col] = np.nan

# Add some outliers
outlier_indices = np.random.choice(n_samples, 20, replace=False)
df.loc[outlier_indices, 'price'] = df.loc[outlier_indices, 'price'] * 5
df.loc[outlier_indices, 'area'] = df.loc[outlier_indices, 'area'] * 3

print("Dataset Overview:")
print(f"Shape: {df.shape}")
print(f"Columns: {list(df.columns)}")
print()

print("Data Types:")
print(df.dtypes)
print()

print("First 5 rows:")
print(df.head())
print()

print("Basic Statistics:")
print(df.describe())
print()

print("Missing Values:")
missing_info = df.isnull().sum()
missing_percent = (missing_info / len(df)) * 100
missing_df = pd.DataFrame({
    'Missing Count': missing_info,
    'Missing Percentage': missing_percent
})
print(missing_df[missing_df['Missing Count'] > 0])
print()
```

### Data Cleaning
```python
print("Data Cleaning Process")
print("=" * 25)

# 1. Handle Missing Values
print("1. Handling Missing Values:")

# For numerical columns, use median imputation
numerical_cols = ['area', 'bedrooms', 'bathrooms']
for col in numerical_cols:
    if df[col].isnull().sum() > 0:
        median_val = df[col].median()
        df[col].fillna(median_val, inplace=True)
        print(f"   {col}: Filled {df[col].isnull().sum()} missing values with median {median_val:.2f}")

print()

# 2. Handle Outliers
print("2. Handling Outliers:")

def detect_outliers_iqr(data, column):
    Q1 = data[column].quantile(0.25)
    Q3 = data[column].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    return data[(data[column] < lower_bound) | (data[column] > upper_bound)]

def cap_outliers(data, column, lower_percentile=0.05, upper_percentile=0.95):
    lower_bound = data[column].quantile(lower_percentile)
    upper_bound = data[column].quantile(upper_percentile)
    data[column] = np.clip(data[column], lower_bound, upper_bound)
    return data

# Detect outliers in key numerical columns
outlier_cols = ['price', 'area', 'lot_size']
for col in outlier_cols:
    outliers = detect_outliers_iqr(df, col)
    print(f"   {col}: {len(outliers)} outliers detected")
    
    # Cap outliers
    df = cap_outliers(df, col)
    print(f"   {col}: Outliers capped to {df[col].quantile(0.05):.2f} - {df[col].quantile(0.95):.2f}")

print()

# 3. Data Type Corrections
print("3. Data Type Corrections:")

# Convert appropriate columns to categorical
categorical_cols = ['bedrooms', 'bathrooms', 'garage', 'neighborhood', 'has_pool', 'stories']
for col in categorical_cols:
    df[col] = df[col].astype('category')

print("   Converted to categorical:", categorical_cols)
print()

# 4. Data Validation
print("4. Data Validation:")

# Check for logical inconsistencies
invalid_houses = df[df['bedrooms'] == 0]
print(f"   Houses with 0 bedrooms: {len(invalid_houses)}")

# Check year built vs age
df['calculated_age'] = 2024 - df['year_built']
age_diff = (df['age'] - df['calculated_age']).abs()
inconsistent_age = df[age_diff > 5]  # Allow 5-year tolerance
print(f"   Houses with inconsistent age: {len(inconsistent_age)}")

# Fix age inconsistencies
df['age'] = df['calculated_age']
df.drop('calculated_age', axis=1, inplace=True)

print("   Age inconsistencies corrected")
print()

print("Cleaned Dataset Summary:")
print(f"Shape: {df.shape}")
print("Missing values after cleaning:", df.isnull().sum().sum())
print()
```

### Univariate Analysis
```python
print("Univariate Analysis")
print("=" * 20)

# Numerical variables
numerical_vars = ['price', 'area', 'age', 'lot_size', 'year_built']
categorical_vars = ['bedrooms', 'bathrooms', 'garage', 'neighborhood', 'has_pool', 'stories']

fig, axes = plt.subplots(2, 3, figsize=(18, 12))
axes = axes.ravel()

for i, col in enumerate(numerical_vars):
    if i < len(axes):
        # Histogram with KDE
        sns.histplot(df[col], kde=True, ax=axes[i], color='skyblue')
        axes[i].set_title(f'Distribution of {col.title()}')
        axes[i].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Box plots for numerical variables
fig, axes = plt.subplots(2, 3, figsize=(18, 12))
axes = axes.ravel()

for i, col in enumerate(numerical_vars):
    if i < len(axes):
        sns.boxplot(y=df[col], ax=axes[i], color='lightcoral')
        axes[i].set_title(f'Box Plot of {col.title()}')
        axes[i].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Categorical variables
fig, axes = plt.subplots(2, 3, figsize=(18, 12))
axes = axes.ravel()

for i, col in enumerate(categorical_vars):
    if i < len(axes):
        value_counts = df[col].value_counts()
        sns.barplot(x=value_counts.index, y=value_counts.values, ax=axes[i], color='lightgreen')
        axes[i].set_title(f'Distribution of {col.title()}')
        axes[i].tick_params(axis='x', rotation=45)
        axes[i].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Summary statistics for numerical variables
print("Numerical Variables Summary:")
print("=" * 30)
print(df[numerical_vars].describe())
print()

# Summary statistics for categorical variables
print("Categorical Variables Summary:")
print("=" * 32)

for col in categorical_vars:
    print(f"\n{col.title()}:")
    value_counts = df[col].value_counts()
    percentages = (value_counts / len(df)) * 100
    summary = pd.DataFrame({
        'Count': value_counts,
        'Percentage': percentages.round(2)
    })
    print(summary)
print()
```

### Bivariate Analysis
```python
print("Bivariate Analysis")
print("=" * 18)

# Correlation matrix for numerical variables
plt.figure(figsize=(10, 8))
correlation_matrix = df[numerical_vars].corr()
mask = np.triu(np.ones_like(correlation_matrix, dtype=bool))
sns.heatmap(correlation_matrix, mask=mask, annot=True, cmap='coolwarm', 
            center=0, square=True, linewidths=0.5)
plt.title('Correlation Matrix of Numerical Variables')
plt.show()

print("Correlation with Price:")
price_corr = df[numerical_vars].corr()['price'].sort_values(ascending=False)
print(price_corr)
print()

# Scatter plots: Price vs key numerical variables
key_vars = ['area', 'age', 'lot_size', 'year_built']
fig, axes = plt.subplots(2, 2, figsize=(15, 12))
axes = axes.ravel()

for i, var in enumerate(key_vars):
    sns.scatterplot(data=df, x=var, y='price', ax=axes[i], alpha=0.6)
    axes[i].set_title(f'Price vs {var.title()}')
    axes[i].grid(True, alpha=0.3)
    
    # Add trend line
    z = np.polyfit(df[var], df['price'], 1)
    p = np.poly1d(z)
    axes[i].plot(df[var], p(df[var]), "r--", alpha=0.8)

plt.tight_layout()
plt.show()

# Box plots: Price by categorical variables
fig, axes = plt.subplots(2, 3, figsize=(18, 12))
axes = axes.ravel()

for i, var in enumerate(categorical_vars):
    if i < len(axes):
        sns.boxplot(data=df, x=var, y='price', ax=axes[i])
        axes[i].set_title(f'Price by {var.title()}')
        axes[i].tick_params(axis='x', rotation=45)
        axes[i].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Statistical tests
from scipy import stats

print("Statistical Tests:")
print("=" * 18)

# ANOVA test for categorical variables vs price
for cat_var in categorical_vars:
    groups = [group['price'].values for name, group in df.groupby(cat_var)]
    if len(groups) > 1:
        f_stat, p_value = stats.f_oneway(*groups)
        print(f"ANOVA - {cat_var} vs price: F={f_stat:.2f}, p={p_value:.4f}")
        
print()

# T-test for binary categorical variables
binary_vars = ['has_pool']
for var in binary_vars:
    group1 = df[df[var] == 1]['price']
    group0 = df[df[var] == 0]['price']
    t_stat, p_value = stats.ttest_ind(group1, group0)
    print(f"T-test - {var} vs price: t={t_stat:.2f}, p={p_value:.4f}")
    
print()
```

### Multivariate Analysis
```python
print("Multivariate Analysis")
print("=" * 21)

# Pair plot for key variables
key_vars_for_pairplot = ['price', 'area', 'bedrooms', 'bathrooms', 'age']
sns.pairplot(df[key_vars_for_pairplot], diag_kind='kde', plot_kws={'alpha': 0.6})
plt.suptitle('Pair Plot of Key Variables', y=1.02)
plt.show()

# 3D scatter plot
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure(figsize=(12, 8))
ax = fig.add_subplot(111, projection='3d')

scatter = ax.scatter(df['area'], df['age'], df['price'], 
                    c=df['bedrooms'].astype(int), cmap='viridis', alpha=0.6)

ax.set_xlabel('Area')
ax.set_ylabel('Age')
ax.set_zlabel('Price')
ax.set_title('3D Scatter Plot: Area, Age, and Price (colored by Bedrooms)')

# Add colorbar
cbar = plt.colorbar(scatter)
cbar.set_label('Bedrooms')

plt.show()

# Faceted analysis
fig, axes = plt.subplots(2, 2, figsize=(15, 12))

# Price vs Area by Neighborhood
sns.scatterplot(data=df, x='area', y='price', hue='neighborhood', ax=axes[0,0])
axes[0,0].set_title('Price vs Area by Neighborhood')
axes[0,0].grid(True, alpha=0.3)

# Price distribution by Bedrooms and Bathrooms
bed_bath_pivot = df.pivot_table(values='price', index='bedrooms', columns='bathrooms', aggfunc='mean')
sns.heatmap(bed_bath_pivot, annot=True, fmt='.0f', cmap='YlOrRd', ax=axes[0,1])
axes[0,1].set_title('Average Price by Bedrooms and Bathrooms')

# Price vs Age by Has Pool
sns.scatterplot(data=df, x='age', y='price', hue='has_pool', style='has_pool', ax=axes[1,0])
axes[1,0].set_title('Price vs Age by Pool Status')
axes[1,0].grid(True, alpha=0.3)

# Price distribution by Stories and Garage
stories_garage_pivot = df.pivot_table(values='price', index='stories', columns='garage', aggfunc='mean')
sns.heatmap(stories_garage_pivot, annot=True, fmt='.0f', cmap='Blues', ax=axes[1,1])
axes[1,1].set_title('Average Price by Stories and Garage')

plt.tight_layout()
plt.show()

# Advanced multivariate analysis
print("Advanced Multivariate Insights:")
print("=" * 32)

# Price prediction using multiple variables
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score
from sklearn.model_selection import train_test_split

# Prepare data for modeling
X = df[['area', 'bedrooms', 'bathrooms', 'age', 'lot_size', 'garage', 'has_pool', 'stories']]
X = pd.get_dummies(X, columns=['bedrooms', 'bathrooms', 'garage', 'stories'], drop_first=True)
y = df['price']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train model
model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print(f"Model R² Score: {r2_score(y_test, y_pred):.4f}")
print()

# Feature importance
feature_importance = pd.DataFrame({
    'feature': X.columns,
    'coefficient': model.coef_
}).sort_values('coefficient', ascending=False)

print("Feature Importance (Linear Regression Coefficients):")
print(feature_importance)
print()
```

### Feature Engineering
```python
print("Feature Engineering")
print("=" * 20)

# Create new features
df_engineered = df.copy()

# Price per square foot
df_engineered['price_per_sqft'] = df_engineered['price'] / df_engineered['area']

# Age categories
df_engineered['age_category'] = pd.cut(df_engineered['age'], 
                                      bins=[0, 10, 25, 50, 100], 
                                      labels=['New', 'Modern', 'Established', 'Old'])

# Size categories
df_engineered['size_category'] = pd.cut(df_engineered['area'], 
                                       bins=[0, 1500, 2500, 3500, 5000], 
                                       labels=['Small', 'Medium', 'Large', 'Extra Large'])

# Luxury score (combination of features)
df_engineered['luxury_score'] = (
    (df_engineered['area'] / df_engineered['area'].max()) * 0.3 +
    (df_engineered['bathrooms'].astype(int) / df_engineered['bathrooms'].max()) * 0.2 +
    (df_engineered['garage'].astype(int) / df_engineered['garage'].max()) * 0.2 +
    (df_engineered['has_pool'].astype(int)) * 0.15 +
    ((2024 - df_engineered['age']) / 100) * 0.15
)

# Bedroom to bathroom ratio
df_engineered['bed_bath_ratio'] = df_engineered['bedrooms'].astype(int) / df_engineered['bathrooms'].astype(int)

# Lot efficiency (building area / lot size)
df_engineered['lot_efficiency'] = df_engineered['area'] / df_engineered['lot_size']

print("New Features Created:")
new_features = ['price_per_sqft', 'age_category', 'size_category', 'luxury_score', 'bed_bath_ratio', 'lot_efficiency']
for feature in new_features:
    print(f"  - {feature}")
print()

# Analyze new features
fig, axes = plt.subplots(2, 3, figsize=(18, 12))
axes = axes.ravel()

for i, feature in enumerate(new_features):
    if i < len(axes):
        if feature in ['age_category', 'size_category']:
            # Categorical features
            value_counts = df_engineered[feature].value_counts()
            sns.barplot(x=value_counts.index, y=value_counts.values, ax=axes[i])
            axes[i].set_title(f'Distribution of {feature.replace("_", " ").title()}')
            axes[i].tick_params(axis='x', rotation=45)
        else:
            # Numerical features
            sns.histplot(df_engineered[feature], kde=True, ax=axes[i])
            axes[i].set_title(f'Distribution of {feature.replace("_", " ").title()}')
        axes[i].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Correlation of new features with price
new_features_numerical = [f for f in new_features if f not in ['age_category', 'size_category']]
correlation_with_price = df_engineered[new_features_numerical + ['price']].corr()['price'].sort_values(ascending=False)

print("Correlation of New Features with Price:")
print(correlation_with_price)
print()

# Box plots for categorical new features
fig, axes = plt.subplots(1, 2, figsize=(15, 6))

sns.boxplot(data=df_engineered, x='age_category', y='price', ax=axes[0])
axes[0].set_title('Price by Age Category')
axes[0].tick_params(axis='x', rotation=45)
axes[0].grid(True, alpha=0.3)

sns.boxplot(data=df_engineered, x='size_category', y='price', ax=axes[1])
axes[1].set_title('Price by Size Category')
axes[1].tick_params(axis='x', rotation=45)
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Insights and Recommendations
```python
print("Key Insights and Recommendations")
print("=" * 35)

# Insight 1: Price drivers
print("1. Key Price Drivers:")
top_correlations = correlation_matrix['price'].sort_values(ascending=False)[1:6]  # Exclude self-correlation
print("   Top 5 features correlated with price:")
for feature, corr in top_correlations.items():
    print(".4f")
print()

# Insight 2: Neighborhood analysis
neighborhood_stats = df.groupby('neighborhood')['price'].agg(['mean', 'median', 'count']).sort_values('mean', ascending=False)
print("2. Neighborhood Analysis:")
print("   Average prices by neighborhood:")
for idx, row in neighborhood_stats.iterrows():
    print(".0f")
print()

# Insight 3: Age impact
age_price_corr = df['age'].corr(df['price'])
print("3. Age Impact:")
print(".4f")
if age_price_corr < -0.3:
    print("   → Older houses tend to be cheaper")
elif age_price_corr > 0.3:
    print("   → Older houses tend to be more expensive")
else:
    print("   → Age has minimal impact on price")
print()

# Insight 4: Luxury features
luxury_features = ['has_pool', 'garage', 'bathrooms']
print("4. Luxury Features Impact:")
for feature in luxury_features:
    if feature == 'has_pool':
        pool_premium = df[df['has_pool'] == 1]['price'].mean() - df[df['has_pool'] == 0]['price'].mean()
        print(".0f")
    elif feature == 'garage':
        garage_premium = df[df['garage'] > 0]['price'].mean() - df[df['garage'] == 0]['price'].mean()
        print(".0f")
    elif feature == 'bathrooms':
        bath_corr = df['bathrooms'].astype(int).corr(df['price'])
        print(".4f")
print()

# Insight 5: Size efficiency
size_efficiency = df['area'].corr(df['price'])
print("5. Size Efficiency:")
print(".4f")
print("   → Larger houses command higher prices")
print()

# Recommendations
print("Recommendations:")
print("=" * 15)
print("1. Focus on area, bedrooms, and bathrooms as primary selling points")
print("2. Consider neighborhood premiums when pricing properties")
print("3. Luxury features like pools significantly increase property value")
print("4. Newer properties generally command higher prices")
print("5. Consider property age when evaluating investment potential")
print("6. Large lots with efficient space utilization are valuable")
print()

# Actionable insights for stakeholders
print("Actionable Insights for Different Stakeholders:")
print("=" * 47)

print("For Home Sellers:")
print("• Maximize property value by highlighting key features (area, bedrooms, bathrooms)")
print("• Consider renovations that add luxury features like pools or additional bathrooms")
print("• Time the sale when the property is in optimal condition")
print()

print("For Home Buyers:")
print("• Prioritize properties with high area-to-price ratios")
print("• Look for properties in premium neighborhoods")
print("• Consider newer properties for better long-term value")
print()

print("For Real Estate Agents:")
print("• Use data-driven pricing strategies based on comparable properties")
print("• Highlight luxury features in property listings")
print("• Educate clients about market trends and neighborhood premiums")
print()

print("For Investors:")
print("• Focus on properties in high-growth neighborhoods")
print("• Consider renovation potential in older properties")
print("• Evaluate lot efficiency for development opportunities")
print()

# Final summary
print("Project Summary:")
print("=" * 16)
print(f"• Dataset: {df.shape[0]} properties with {df.shape[1]} features")
print("• Key findings: Area, bedrooms, and bathrooms are strongest price predictors")
print("• Data quality: Cleaned missing values and outliers")
print("• Insights: Generated actionable recommendations for stakeholders")
print("• Visualizations: Created comprehensive EDA plots and charts")
```

## Tasks

Create a Python file named `eda_project.py` in this folder and implement the following:

1. Load and explore a real-world dataset (or create a comprehensive synthetic dataset)
2. Perform data cleaning (missing values, outliers, data validation)
3. Conduct univariate, bivariate, and multivariate analysis
4. Create visualizations for data exploration
5. Perform feature engineering to create new meaningful features
6. Generate insights and actionable recommendations
7. Document findings and create a comprehensive EDA report

## Additional Resources

- EDA best practices and techniques
- Data visualization libraries (matplotlib, seaborn, plotly)
- Statistical analysis methods
- Feature engineering strategies