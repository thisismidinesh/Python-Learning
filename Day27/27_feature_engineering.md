# Feature Engineering in Python

## Introduction

Feature engineering is the process of transforming raw data into features that better represent the underlying problem to the predictive models, resulting in improved model accuracy. It involves creating new features, selecting important features, and transforming existing features to make them more suitable for machine learning algorithms.

## Key Concepts

### Types of Feature Engineering
- **Feature Creation**: Creating new features from existing ones
- **Feature Selection**: Selecting the most important features
- **Feature Transformation**: Transforming features to better suit the model
- **Feature Scaling**: Normalizing or standardizing features
- **Feature Encoding**: Converting categorical features to numerical

### Feature Selection Methods
- **Filter Methods**: Statistical tests (correlation, chi-square, mutual information)
- **Wrapper Methods**: Recursive feature elimination, forward/backward selection
- **Embedded Methods**: Feature selection during model training (LASSO, tree-based)

### Feature Scaling Techniques
- **Standardization (Z-score)**: Mean=0, Std=1
- **Min-Max Scaling**: Scale to [0,1] range
- **Robust Scaling**: Uses median and IQR, less sensitive to outliers
- **MaxAbs Scaling**: Scale by maximum absolute value

### Categorical Encoding
- **Label Encoding**: Ordinal categories
- **One-Hot Encoding**: Nominal categories
- **Ordinal Encoding**: Ordered categories
- **Target Encoding**: Using target statistics
- **Frequency Encoding**: Using category frequencies

## Examples

### Data Preparation and Exploration
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import make_classification, load_iris, fetch_california_housing
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import (StandardScaler, MinMaxScaler, RobustScaler, 
                                 MaxAbsScaler, LabelEncoder, OneHotEncoder, 
                                 OrdinalEncoder)
from sklearn.feature_selection import (SelectKBest, SelectPercentile, 
                                     RFE, SelectFromModel, mutual_info_regression,
                                     chi2, f_regression)
from sklearn.linear_model import LinearRegression, LogisticRegression, Lasso
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, classification_report
import warnings
warnings.filterwarnings('ignore')

# Load datasets
print("Loading datasets...")
housing = fetch_california_housing()
iris = load_iris()

# Create sample datasets
np.random.seed(42)

# Regression dataset
X_reg, y_reg = housing.data, housing.target
feature_names_reg = housing.feature_names
df_reg = pd.DataFrame(X_reg, columns=feature_names_reg)
df_reg['target'] = y_reg

# Classification dataset
X_clf, y_clf = iris.data, iris.target
feature_names_clf = iris.feature_names
df_clf = pd.DataFrame(X_clf, columns=feature_names_clf)
df_clf['target'] = y_clf

print("Regression dataset shape:", df_reg.shape)
print("Classification dataset shape:", df_clf.shape)
print()

# Create mixed-type dataset for encoding examples
np.random.seed(42)
n_samples = 1000

data = {
    'age': np.random.randint(18, 80, n_samples),
    'income': np.random.normal(50000, 15000, n_samples),
    'education': np.random.choice(['High School', 'Bachelor', 'Master', 'PhD'], n_samples),
    'city': np.random.choice(['New York', 'Los Angeles', 'Chicago', 'Houston', 'Phoenix'], n_samples),
    'experience': np.random.randint(0, 40, n_samples),
    'department': np.random.choice(['Engineering', 'Sales', 'Marketing', 'HR', 'Finance'], n_samples),
    'performance_score': np.random.uniform(1, 10, n_samples)
}

df_mixed = pd.DataFrame(data)
df_mixed['income'] = df_mixed['income'].clip(lower=0)  # Ensure non-negative income

print("Mixed-type dataset:")
print(df_mixed.head())
print()
print("Data types:")
print(df_mixed.dtypes)
print()
```

### Feature Scaling
```python
# Prepare data for scaling
X_reg_train, X_reg_test, y_reg_train, y_reg_test = train_test_split(
    X_reg, y_reg, test_size=0.2, random_state=42)

print("Original feature statistics:")
print(pd.DataFrame(X_reg_train, columns=feature_names_reg).describe())
print()

# Different scaling methods
scalers = {
    'Standard Scaler': StandardScaler(),
    'Min-Max Scaler': MinMaxScaler(),
    'Robust Scaler': RobustScaler(),
    'MaxAbs Scaler': MaxAbsScaler()
}

scaled_data = {}
scaling_stats = {}

for name, scaler in scalers.items():
    # Fit and transform training data
    X_train_scaled = scaler.fit_transform(X_reg_train)
    X_test_scaled = scaler.transform(X_reg_test)
    
    scaled_data[name] = (X_train_scaled, X_test_scaled)
    
    # Calculate statistics
    df_scaled = pd.DataFrame(X_train_scaled, columns=feature_names_reg)
    stats = df_scaled.describe()
    scaling_stats[name] = stats
    
    print(f"{name} Statistics:")
    print(f"Mean: {df_scaled.mean().round(6).tolist()}")
    print(f"Std:  {df_scaled.std().round(6).tolist()}")
    print(f"Min:  {df_scaled.min().round(6).tolist()}")
    print(f"Max:  {df_scaled.max().round(6).tolist()}")
    print()

# Visualize scaling effects
fig, axes = plt.subplots(2, 2, figsize=(15, 10))
axes = axes.ravel()

for i, (name, (X_train_scaled, _)) in enumerate(scaled_data.items()):
    df_scaled = pd.DataFrame(X_train_scaled, columns=feature_names_reg)
    
    # Plot boxplots for each feature
    df_scaled.boxplot(ax=axes[i], rot=45)
    axes[i].set_title(f'{name} - Feature Distributions')
    axes[i].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Compare scaling impact on model performance
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score

scaling_performance = {}

for name, (X_train_scaled, X_test_scaled) in scaled_data.items():
    lr = LinearRegression()
    lr.fit(X_train_scaled, y_reg_train)
    pred = lr.predict(X_test_scaled)
    r2 = r2_score(y_reg_test, pred)
    scaling_performance[name] = r2

print("Model Performance with Different Scaling:")
for name, r2 in scaling_performance.items():
    print(f"{name}: R² = {r2:.4f}")
print()
```

### Categorical Encoding
```python
# Prepare data for encoding
X_mixed = df_mixed.drop('performance_score', axis=1)
y_mixed = df_mixed['performance_score']

# Identify categorical and numerical columns
categorical_cols = X_mixed.select_dtypes(include=['object']).columns.tolist()
numerical_cols = X_mixed.select_dtypes(include=[np.number]).columns.tolist()

print("Categorical columns:", categorical_cols)
print("Numerical columns:", numerical_cols)
print()

# Label Encoding (for ordinal categories)
ordinal_categories = ['education']  # Assuming education has order
label_encoders = {}

X_encoded = X_mixed.copy()

for col in ordinal_categories:
    le = LabelEncoder()
    X_encoded[col] = le.fit_transform(X_encoded[col])
    label_encoders[col] = le
    
    print(f"Label Encoding for {col}:")
    for i, category in enumerate(le.classes_):
        print(f"  {category} -> {i}")
    print()

# One-Hot Encoding (for nominal categories)
nominal_categories = ['city', 'department']
X_encoded = pd.get_dummies(X_encoded, columns=nominal_categories, drop_first=True)

print("After One-Hot Encoding:")
print("New columns:", [col for col in X_encoded.columns if any(cat in col for cat in nominal_categories)])
print("Shape:", X_encoded.shape)
print()

# Frequency Encoding
frequency_encoders = {}

for col in categorical_cols:
    if col not in ordinal_categories:  # Skip already encoded columns
        freq_map = X_mixed[col].value_counts(normalize=True)
        X_encoded[f'{col}_freq'] = X_mixed[col].map(freq_map)
        frequency_encoders[col] = freq_map
        
        print(f"Frequency Encoding for {col}:")
        for category, freq in freq_map.items():
            print(f"  {category}: {freq:.3f}")
        print()

# Target Encoding (Mean Encoding)
target_encoders = {}

for col in categorical_cols:
    if col not in ordinal_categories:
        target_means = df_mixed.groupby(col)['performance_score'].mean()
        X_encoded[f'{col}_target'] = X_mixed[col].map(target_means)
        target_encoders[col] = target_means
        
        print(f"Target Encoding for {col}:")
        for category, mean in target_means.items():
            print(f"  {category}: {mean:.3f}")
        print()

print("Final encoded dataset shape:", X_encoded.shape)
print("Columns:", X_encoded.columns.tolist())
print()

# Compare encoding methods
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import cross_val_score

# Split data
X_train_enc, X_test_enc, y_train_enc, y_test_enc = train_test_split(
    X_encoded, y_mixed, test_size=0.2, random_state=42)

# Train model
rf_enc = RandomForestRegressor(n_estimators=100, random_state=42)
rf_enc.fit(X_train_enc, y_train_enc)
pred_enc = rf_enc.predict(X_test_enc)

print("Model Performance with Encoded Features:")
print(f"R² Score: {r2_score(y_test_enc, pred_enc):.4f}")
print(f"RMSE: {np.sqrt(mean_squared_error(y_test_enc, pred_enc)):.4f}")
print()

# Feature importance
feature_importance = pd.DataFrame({
    'feature': X_encoded.columns,
    'importance': rf_enc.feature_importances_
}).sort_values('importance', ascending=False)

print("Top 10 Feature Importances:")
print(feature_importance.head(10))
```

### Feature Creation
```python
# Create new features from existing ones
df_engineered = df_mixed.copy()

# Age-based features
df_engineered['age_group'] = pd.cut(df_engineered['age'], 
                                   bins=[0, 25, 35, 50, 65, 100], 
                                   labels=['Young', 'Young Adult', 'Adult', 'Middle Age', 'Senior'])

df_engineered['age_squared'] = df_engineered['age'] ** 2
df_engineered['age_log'] = np.log1p(df_engineered['age'])

# Income-based features
df_engineered['income_log'] = np.log1p(df_engineered['income'])
df_engineered['income_per_age'] = df_engineered['income'] / df_engineered['age']
df_engineered['income_category'] = pd.qcut(df_engineered['income'], 
                                          q=4, labels=['Low', 'Medium', 'High', 'Very High'])

# Experience-based features
df_engineered['exp_per_age'] = df_engineered['experience'] / df_engineered['age']
df_engineered['exp_squared'] = df_engineered['experience'] ** 2

# Interaction features
df_engineered['income_exp_interaction'] = df_engineered['income'] * df_engineered['experience']
df_engineered['age_exp_ratio'] = df_engineered['age'] / (df_engineered['experience'] + 1)  # +1 to avoid division by zero

# Education level mapping
education_mapping = {
    'High School': 1,
    'Bachelor': 2,
    'Master': 3,
    'PhD': 4
}
df_engineered['education_level'] = df_engineered['education'].map(education_mapping)

# Department performance (simulated)
dept_performance = df_mixed.groupby('department')['performance_score'].mean()
df_engineered['dept_avg_performance'] = df_engineered['department'].map(dept_performance)

print("New features created:")
new_features = [col for col in df_engineered.columns if col not in df_mixed.columns]
print(new_features)
print()

print("Sample of engineered features:")
print(df_engineered[new_features].head())
print()

# Correlation analysis of new features with target
correlations = df_engineered.corr()['performance_score'].sort_values(ascending=False)
print("Feature correlations with target:")
print(correlations)
print()

# Visualize feature relationships
fig, axes = plt.subplots(2, 3, figsize=(15, 10))

# Age vs performance by education
sns.boxplot(data=df_engineered, x='education', y='performance_score', ax=axes[0,0])
axes[0,0].set_title('Performance by Education')
axes[0,0].tick_params(axis='x', rotation=45)

# Income vs performance
sns.scatterplot(data=df_engineered, x='income', y='performance_score', ax=axes[0,1])
axes[0,1].set_title('Income vs Performance')

# Experience vs performance
sns.scatterplot(data=df_engineered, x='experience', y='performance_score', ax=axes[0,2])
axes[0,2].set_title('Experience vs Performance')

# Age group vs performance
sns.boxplot(data=df_engineered, x='age_group', y='performance_score', ax=axes[1,0])
axes[1,0].set_title('Performance by Age Group')
axes[1,0].tick_params(axis='x', rotation=45)

# Income category vs performance
sns.boxplot(data=df_engineered, x='income_category', y='performance_score', ax=axes[1,1])
axes[1,1].set_title('Performance by Income Category')

# Department vs performance
sns.boxplot(data=df_engineered, x='department', y='performance_score', ax=axes[1,2])
axes[1,2].set_title('Performance by Department')
axes[1,2].tick_params(axis='x', rotation=45)

plt.tight_layout()
plt.show()
```

### Feature Selection
```python
# Prepare data for feature selection
X_engineered = df_engineered.drop(['performance_score', 'education', 'city', 'department'], axis=1)
y_engineered = df_engineered['performance_score']

# Handle categorical features created during engineering
categorical_features = ['age_group', 'income_category']
X_engineered = pd.get_dummies(X_engineered, columns=categorical_features, drop_first=True)

# Fill any missing values
X_engineered = X_engineered.fillna(X_engineered.mean())

print("Feature selection dataset shape:", X_engineered.shape)
print("Features:", X_engineered.columns.tolist())
print()

# Split data
X_train_fs, X_test_fs, y_train_fs, y_test_fs = train_test_split(
    X_engineered, y_engineered, test_size=0.2, random_state=42)

# 1. Filter Methods
print("1. Filter Methods:")
print("-" * 30)

# Correlation-based selection
correlations = X_train_fs.corrwith(y_train_fs).abs().sort_values(ascending=False)
print("Top 10 features by correlation with target:")
print(correlations.head(10))
print()

# Mutual Information
mi_scores = mutual_info_regression(X_train_fs, y_train_fs)
mi_df = pd.DataFrame({'feature': X_train_fs.columns, 'mi_score': mi_scores})
mi_df = mi_df.sort_values('mi_score', ascending=False)
print("Top 10 features by mutual information:")
print(mi_df.head(10))
print()

# SelectKBest with f_regression
kbest = SelectKBest(score_func=f_regression, k=10)
X_train_kbest = kbest.fit_transform(X_train_fs, y_train_fs)
selected_features_kbest = X_train_fs.columns[kbest.get_support()].tolist()

print("SelectKBest (f_regression) selected features:")
print(selected_features_kbest)
print()

# 2. Wrapper Methods
print("2. Wrapper Methods:")
print("-" * 30)

# Recursive Feature Elimination (RFE)
lr = LinearRegression()
rfe = RFE(estimator=lr, n_features_to_select=10)
X_train_rfe = rfe.fit_transform(X_train_fs, y_train_fs)
selected_features_rfe = X_train_fs.columns[rfe.get_support()].tolist()

print("RFE selected features:")
print(selected_features_rfe)
print()

# 3. Embedded Methods
print("3. Embedded Methods:")
print("-" * 30)

# LASSO feature selection
lasso = Lasso(alpha=0.01, random_state=42)
lasso.fit(X_train_fs, y_train_fs)
lasso_coef = pd.DataFrame({'feature': X_train_fs.columns, 'coef': lasso.coef_})
lasso_selected = lasso_coef[lasso_coef['coef'] != 0]['feature'].tolist()

print("LASSO selected features (non-zero coefficients):")
print(lasso_selected)
print()

# Random Forest feature importance
rf = RandomForestRegressor(n_estimators=100, random_state=42)
rf.fit(X_train_fs, y_train_fs)
rf_importance = pd.DataFrame({'feature': X_train_fs.columns, 'importance': rf.feature_importances_})
rf_importance = rf_importance.sort_values('importance', ascending=False)

print("Top 10 features by Random Forest importance:")
print(rf_importance.head(10))
print()

# SelectFromModel with Random Forest
sfm = SelectFromModel(rf, threshold='median')
X_train_sfm = sfm.fit_transform(X_train_fs, y_train_fs)
selected_features_sfm = X_train_fs.columns[sfm.get_support()].tolist()

print("SelectFromModel (Random Forest) selected features:")
print(selected_features_sfm)
print()

# Compare feature selection methods
selection_methods = {
    'SelectKBest': selected_features_kbest,
    'RFE': selected_features_rfe,
    'LASSO': lasso_selected,
    'Random Forest': selected_features_sfm
}

# Find common features across methods
all_selected = []
for method, features in selection_methods.items():
    all_selected.extend(features)

feature_counts = pd.Series(all_selected).value_counts()
common_features = feature_counts[feature_counts >= 2].index.tolist()

print("Features selected by multiple methods:")
print(common_features)
print()

# Evaluate models with different feature sets
from sklearn.metrics import r2_score

feature_sets = {
    'All Features': X_train_fs.columns.tolist(),
    'SelectKBest': selected_features_kbest,
    'RFE': selected_features_rfe,
    'LASSO': lasso_selected,
    'Random Forest': selected_features_sfm,
    'Common Features': common_features
}

performance_results = {}

for name, features in feature_sets.items():
    if len(features) == 0:
        continue
        
    X_train_subset = X_train_fs[features]
    X_test_subset = X_test_fs[features]
    
    rf_temp = RandomForestRegressor(n_estimators=100, random_state=42)
    rf_temp.fit(X_train_subset, y_train_fs)
    pred_temp = rf_temp.predict(X_test_subset)
    
    r2 = r2_score(y_test_fs, pred_temp)
    rmse = np.sqrt(mean_squared_error(y_test_fs, pred_temp))
    
    performance_results[name] = {
        'r2': r2,
        'rmse': rmse,
        'n_features': len(features)
    }

print("Model Performance with Different Feature Sets:")
print("=" * 60)
print(f"{'Method':<15} {'Features':<10} {'R²':<10} {'RMSE':<10}")
print("-" * 60)

for name, results in performance_results.items():
    print(f"{name:<15} {results['n_features']:<10} {results['r2']:<10.4f} {results['rmse']:<10.4f}")

print()

# Visualize feature selection performance
methods = list(performance_results.keys())
r2_scores = [performance_results[m]['r2'] for m in methods]
n_features = [performance_results[m]['n_features'] for m in methods]

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

ax1.bar(methods, r2_scores, color='skyblue', alpha=0.7)
ax1.set_ylabel('R² Score')
ax1.set_title('Model Performance by Feature Selection Method')
ax1.tick_params(axis='x', rotation=45)
ax1.grid(True, alpha=0.3)

for i, v in enumerate(r2_scores):
    ax1.text(i, v + 0.001, f'{v:.3f}', ha='center', va='bottom')

ax2.bar(methods, n_features, color='lightcoral', alpha=0.7)
ax2.set_ylabel('Number of Features')
ax2.set_title('Number of Features Selected')
ax2.tick_params(axis='x', rotation=45)
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Advanced Feature Engineering Techniques
```python
# Polynomial features
from sklearn.preprocessing import PolynomialFeatures

# Create polynomial features
poly = PolynomialFeatures(degree=2, interaction_only=False, include_bias=False)
X_poly = poly.fit_transform(X_reg)

print("Polynomial Features:")
print(f"Original features: {X_reg.shape[1]}")
print(f"Polynomial features: {X_poly.shape[1]}")
print("New feature names:")
poly_feature_names = poly.get_feature_names_out(feature_names_reg)
print(poly_feature_names[:20])  # Show first 20
print()

# Binning continuous features
from sklearn.preprocessing import KBinsDiscretizer

# Bin age feature
age_bins = KBinsDiscretizer(n_bins=5, encode='ordinal', strategy='quantile')
df_engineered['age_binned'] = age_bins.fit_transform(df_engineered[['age']])

print("Age Binning:")
print("Bin edges:", age_bins.bin_edges_[0])
print("Value counts:")
print(df_engineered['age_binned'].value_counts().sort_index())
print()

# Handle outliers
def handle_outliers(df, columns, method='iqr', factor=1.5):
    """Handle outliers using IQR or Z-score method"""
    df_clean = df.copy()
    
    for col in columns:
        if method == 'iqr':
            Q1 = df_clean[col].quantile(0.25)
            Q3 = df_clean[col].quantile(0.75)
            IQR = Q3 - Q1
            lower_bound = Q1 - factor * IQR
            upper_bound = Q3 + factor * IQR
            
            # Cap outliers
            df_clean[col] = np.clip(df_clean[col], lower_bound, upper_bound)
            
        elif method == 'zscore':
            z_scores = np.abs((df_clean[col] - df_clean[col].mean()) / df_clean[col].std())
            df_clean[col] = np.where(z_scores > 3, df_clean[col].median(), df_clean[col])
    
    return df_clean

# Handle outliers in numerical columns
numerical_cols = ['age', 'income', 'experience', 'performance_score']
df_no_outliers = handle_outliers(df_engineered, numerical_cols, method='iqr')

print("Outlier Handling:")
print("Original statistics:")
print(df_engineered[numerical_cols].describe())
print()
print("After outlier handling:")
print(df_no_outliers[numerical_cols].describe())
print()

# Feature scaling after engineering
from sklearn.preprocessing import PowerTransformer

# Power transformation for skewed features
pt = PowerTransformer(method='yeo-johnson')
income_transformed = pt.fit_transform(df_engineered[['income']])
df_engineered['income_transformed'] = income_transformed

print("Power Transformation:")
print("Original income statistics:")
print(df_engineered['income'].describe())
print()
print("Transformed income statistics:")
print(df_engineered['income_transformed'].describe())
print()

# Visualize transformation effect
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

ax1.hist(df_engineered['income'], bins=30, alpha=0.7, color='blue')
ax1.set_title('Original Income Distribution')
ax1.set_xlabel('Income')
ax1.set_ylabel('Frequency')
ax1.grid(True, alpha=0.3)

ax2.hist(df_engineered['income_transformed'], bins=30, alpha=0.7, color='green')
ax2.set_title('Transformed Income Distribution')
ax2.set_xlabel('Transformed Income')
ax2.set_ylabel('Frequency')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

## Tasks

Create a Python file named `feature_engineering.py` in this folder and implement the following:

1. Implement different feature scaling techniques (Standard, Min-Max, Robust, MaxAbs)
2. Apply categorical encoding methods (Label, One-Hot, Target, Frequency encoding)
3. Create new features from existing ones (polynomial, interaction, binning)
4. Implement feature selection methods (filter, wrapper, embedded)
5. Handle outliers and skewed distributions
6. Compare model performance with different feature engineering approaches

## Additional Resources

- Scikit-learn preprocessing documentation
- Feature engineering best practices
- Domain-specific feature engineering techniques