# Regression Algorithms in Python

## Introduction

Regression is a supervised machine learning technique used to predict continuous numerical values. Unlike classification which predicts categories, regression predicts quantities. Python's scikit-learn library provides comprehensive regression algorithms for different types of problems.

## Key Concepts

### Types of Regression
- **Simple Linear Regression**: One feature, one target
- **Multiple Linear Regression**: Multiple features, one target
- **Polynomial Regression**: Non-linear relationships
- **Regularized Regression**: L1/L2 regularization (Ridge, Lasso)
- **Decision Tree Regression**: Tree-based approach
- **Random Forest Regression**: Ensemble of decision trees
- **Support Vector Regression**: SVM for regression

### Evaluation Metrics
- **Mean Absolute Error (MAE)**: Average absolute errors
- **Mean Squared Error (MSE)**: Average squared errors
- **Root Mean Squared Error (RMSE)**: Square root of MSE
- **R² Score**: Proportion of variance explained
- **Adjusted R²**: R² adjusted for number of features

### Assumptions of Linear Regression
- Linearity: Linear relationship between features and target
- Independence: Observations are independent
- Homoscedasticity: Constant variance of errors
- Normality: Normal distribution of errors
- No multicollinearity: Features not highly correlated

## Examples

### Data Preparation and Exploration
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import make_regression, load_boston
from sklearn.model_selection import train_test_split, cross_val_score, GridSearchCV
from sklearn.preprocessing import StandardScaler, PolynomialFeatures
from sklearn.metrics import (mean_absolute_error, mean_squared_error, r2_score, 
                           explained_variance_score)
from sklearn.linear_model import LinearRegression, Ridge, Lasso
from sklearn.tree import DecisionTreeRegressor
from sklearn.ensemble import RandomForestRegressor
from sklearn.svm import SVR
from sklearn.neighbors import KNeighborsRegressor

# Generate synthetic regression data
np.random.seed(42)
X, y = make_regression(n_samples=1000, n_features=5, noise=0.1, random_state=42)

# Create DataFrame for easier analysis
feature_names = [f'feature_{i}' for i in range(X.shape[1])]
df = pd.DataFrame(X, columns=feature_names)
df['target'] = y

print("Dataset Information:")
print(f"Dataset shape: {df.shape}")
print(f"Features: {feature_names}")
print()

print("First 5 rows:")
print(df.head())
print()

print("Descriptive Statistics:")
print(df.describe())
print()

# Correlation analysis
correlation_matrix = df.corr()
print("Correlation with target:")
print(correlation_matrix['target'].sort_values(ascending=False))
print()

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

print("Training set shape:", X_train.shape)
print("Testing set shape:", X_test.shape)
print()

# Scale the features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Linear Regression
```python
# Train Linear Regression
lr = LinearRegression()
lr.fit(X_train_scaled, y_train)

# Make predictions
lr_pred = lr.predict(X_test_scaled)

print("Linear Regression Results:")
print(f"R² Score: {r2_score(y_test, lr_pred):.4f}")
print(f"Mean Absolute Error: {mean_absolute_error(y_test, lr_pred):.4f}")
print(f"Mean Squared Error: {mean_squared_error(y_test, lr_pred):.4f}")
print(f"Root Mean Squared Error: {np.sqrt(mean_squared_error(y_test, lr_pred)):.4f}")
print()

# Feature coefficients
print("Feature Coefficients:")
for feature, coef in zip(feature_names, lr.coef_):
    print(f"{feature}: {coef:.4f}")
print(f"Intercept: {lr.intercept_:.4f}")
print()

# Residual analysis
residuals = y_test - lr_pred
plt.figure(figsize=(12, 5))

plt.subplot(1, 3, 1)
plt.scatter(lr_pred, residuals, alpha=0.6)
plt.xlabel('Predicted Values')
plt.ylabel('Residuals')
plt.title('Residuals vs Predicted Values')
plt.axhline(y=0, color='r', linestyle='--')
plt.grid(True, alpha=0.3)

plt.subplot(1, 3, 2)
plt.hist(residuals, bins=30, alpha=0.7, color='skyblue')
plt.xlabel('Residuals')
plt.ylabel('Frequency')
plt.title('Residual Distribution')
plt.grid(True, alpha=0.3)

plt.subplot(1, 3, 3)
stats.probplot(residuals, dist="norm", plot=plt)
plt.title('Q-Q Plot of Residuals')

plt.tight_layout()
plt.show()
```

### Polynomial Regression
```python
# Create polynomial features
degrees = [1, 2, 3, 4]
poly_results = {}

for degree in degrees:
    # Create polynomial features
    poly = PolynomialFeatures(degree=degree)
    X_train_poly = poly.fit_transform(X_train_scaled)
    X_test_poly = poly.transform(X_test_scaled)
    
    # Train linear regression on polynomial features
    poly_lr = LinearRegression()
    poly_lr.fit(X_train_poly, y_train)
    
    # Make predictions
    poly_pred = poly_lr.predict(X_test_poly)
    
    # Calculate metrics
    r2 = r2_score(y_test, poly_pred)
    mse = mean_squared_error(y_test, poly_pred)
    rmse = np.sqrt(mse)
    
    poly_results[degree] = {
        'r2': r2,
        'mse': mse,
        'rmse': rmse,
        'n_features': X_train_poly.shape[1]
    }

print("Polynomial Regression Results:")
print("=" * 50)
print(f"{'Degree':<8} {'R²':<10} {'MSE':<12} {'RMSE':<12} {'Features':<10}")
print("-" * 50)

for degree, results in poly_results.items():
    print(f"{degree:<8} {results['r2']:<10.4f} {results['mse']:<12.4f} {results['rmse']:<12.4f} {results['n_features']:<10}")

print()

# Plot polynomial regression performance
degrees_list = list(poly_results.keys())
r2_scores = [poly_results[d]['r2'] for d in degrees_list]

plt.figure(figsize=(10, 6))
plt.plot(degrees_list, r2_scores, 'bo-', linewidth=2, markersize=8)
plt.xlabel('Polynomial Degree')
plt.ylabel('R² Score')
plt.title('Polynomial Regression Performance')
plt.grid(True, alpha=0.3)
plt.xticks(degrees_list)
plt.show()
```

### Regularized Regression (Ridge and Lasso)
```python
# Ridge Regression
alphas = [0.001, 0.01, 0.1, 1.0, 10.0, 100.0]
ridge_results = {}

for alpha in alphas:
    ridge = Ridge(alpha=alpha, random_state=42)
    ridge.fit(X_train_scaled, y_train)
    ridge_pred = ridge.predict(X_test_scaled)
    
    r2 = r2_score(y_test, ridge_pred)
    mse = mean_squared_error(y_test, ridge_pred)
    
    ridge_results[alpha] = {
        'r2': r2,
        'mse': mse,
        'coefficients': ridge.coef_
    }

print("Ridge Regression Results:")
print("=" * 40)
print(f"{'Alpha':<8} {'R²':<10} {'MSE':<12}")
print("-" * 40)

for alpha, results in ridge_results.items():
    print(f"{alpha:<8} {results['r2']:<10.4f} {results['mse']:<12.4f}")

print()

# Lasso Regression
lasso_results = {}

for alpha in alphas:
    lasso = Lasso(alpha=alpha, random_state=42, max_iter=10000)
    lasso.fit(X_train_scaled, y_train)
    lasso_pred = lasso.predict(X_test_scaled)
    
    r2 = r2_score(y_test, lasso_pred)
    mse = mean_squared_error(y_test, lasso_pred)
    
    lasso_results[alpha] = {
        'r2': r2,
        'mse': mse,
        'coefficients': lasso.coef_,
        'n_nonzero_coef': np.sum(lasso.coef_ != 0)
    }

print("Lasso Regression Results:")
print("=" * 45)
print(f"{'Alpha':<8} {'R²':<10} {'MSE':<12} {'Non-zero Coef':<15}")
print("-" * 45)

for alpha, results in lasso_results.items():
    print(f"{alpha:<8} {results['r2']:<10.4f} {results['mse']:<12.4f} {results['n_nonzero_coef']:<15}")

print()

# Compare coefficient shrinkage
best_ridge_alpha = max(ridge_results.keys(), key=lambda x: ridge_results[x]['r2'])
best_lasso_alpha = max(lasso_results.keys(), key=lambda x: lasso_results[x]['r2'])

plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.plot(range(len(feature_names)), ridge_results[best_ridge_alpha]['coefficients'], 'bo-', label='Ridge')
plt.plot(range(len(feature_names)), lasso_results[best_lasso_alpha]['coefficients'], 'ro-', label='Lasso')
plt.plot(range(len(feature_names)), lr.coef_, 'go-', label='Linear')
plt.xlabel('Feature Index')
plt.ylabel('Coefficient Value')
plt.title('Coefficient Comparison')
plt.legend()
plt.grid(True, alpha=0.3)
plt.xticks(range(len(feature_names)), feature_names, rotation=45)

plt.subplot(1, 2, 2)
plt.plot(alphas, [ridge_results[a]['r2'] for a in alphas], 'b-', label='Ridge R²', linewidth=2)
plt.plot(alphas, [lasso_results[a]['r2'] for a in alphas], 'r-', label='Lasso R²', linewidth=2)
plt.xscale('log')
plt.xlabel('Alpha (log scale)')
plt.ylabel('R² Score')
plt.title('Regularization Effect on R²')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Decision Tree Regression
```python
# Test different max depths
depths = range(1, 21)
dt_results = {}

for depth in depths:
    dt = DecisionTreeRegressor(max_depth=depth, random_state=42)
    dt.fit(X_train, y_train)
    dt_pred = dt.predict(X_test)
    
    r2 = r2_score(y_test, dt_pred)
    mse = mean_squared_error(y_test, dt_pred)
    
    dt_results[depth] = {
        'r2': r2,
        'mse': mse
    }

# Plot performance vs depth
depths_list = list(dt_results.keys())
r2_scores = [dt_results[d]['r2'] for d in depths_list]
mse_scores = [dt_results[d]['mse'] for d in depths_list]

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

ax1.plot(depths_list, r2_scores, 'bo-', linewidth=2)
ax1.set_xlabel('Max Depth')
ax1.set_ylabel('R² Score')
ax1.set_title('Decision Tree: R² vs Max Depth')
ax1.grid(True, alpha=0.3)

ax2.plot(depths_list, mse_scores, 'ro-', linewidth=2)
ax2.set_xlabel('Max Depth')
ax2.set_ylabel('MSE')
ax2.set_title('Decision Tree: MSE vs Max Depth')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Best depth
best_depth = max(dt_results.keys(), key=lambda x: dt_results[x]['r2'])
dt_best = DecisionTreeRegressor(max_depth=best_depth, random_state=42)
dt_best.fit(X_train, y_train)
dt_pred = dt_best.predict(X_test)

print(f"Best Decision Tree Results (max_depth={best_depth}):")
print(f"R² Score: {r2_score(y_test, dt_pred):.4f}")
print(f"Mean Squared Error: {mean_squared_error(y_test, dt_pred):.4f}")
print()

print("Feature Importances:")
for feature, importance in zip(feature_names, dt_best.feature_importances_):
    print(f"{feature}: {importance:.4f}")
```

### Random Forest Regression
```python
# Test different numbers of estimators
n_estimators_list = [10, 50, 100, 200, 500]
rf_results = {}

for n_est in n_estimators_list:
    rf = RandomForestRegressor(n_estimators=n_est, random_state=42, n_jobs=-1)
    rf.fit(X_train, y_train)
    rf_pred = rf.predict(X_test)
    
    r2 = r2_score(y_test, rf_pred)
    mse = mean_squared_error(y_test, rf_pred)
    
    rf_results[n_est] = {
        'r2': r2,
        'mse': mse
    }

# Plot performance vs number of estimators
r2_scores = [rf_results[n]['r2'] for n in n_estimators_list]
mse_scores = [rf_results[n]['mse'] for n in n_estimators_list]

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

ax1.plot(n_estimators_list, r2_scores, 'bo-', linewidth=2, markersize=8)
ax1.set_xlabel('Number of Estimators')
ax1.set_ylabel('R² Score')
ax1.set_title('Random Forest: R² vs Number of Estimators')
ax1.grid(True, alpha=0.3)

ax2.plot(n_estimators_list, mse_scores, 'ro-', linewidth=2, markersize=8)
ax2.set_xlabel('Number of Estimators')
ax2.set_ylabel('MSE')
ax2.set_title('Random Forest: MSE vs Number of Estimators')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Best Random Forest
best_n_est = max(rf_results.keys(), key=lambda x: rf_results[x]['r2'])
rf_best = RandomForestRegressor(n_estimators=best_n_est, random_state=42, n_jobs=-1)
rf_best.fit(X_train, y_train)
rf_pred = rf_best.predict(X_test)

print(f"Best Random Forest Results (n_estimators={best_n_est}):")
print(f"R² Score: {r2_score(y_test, rf_pred):.4f}")
print(f"Mean Squared Error: {mean_squared_error(y_test, rf_pred):.4f}")
print()

print("Feature Importances:")
for feature, importance in zip(feature_names, rf_best.feature_importances_):
    print(f"{feature}: {importance:.4f}")
```

### Support Vector Regression
```python
# Test different kernels
kernels = ['linear', 'poly', 'rbf', 'sigmoid']
svr_results = {}

for kernel in kernels:
    svr = SVR(kernel=kernel)
    svr.fit(X_train_scaled, y_train)
    svr_pred = svr.predict(X_test_scaled)
    
    r2 = r2_score(y_test, svr_pred)
    mse = mean_squared_error(y_test, svr_pred)
    
    svr_results[kernel] = {
        'r2': r2,
        'mse': mse
    }

print("Support Vector Regression Results:")
print("=" * 40)
print(f"{'Kernel':<10} {'R²':<10} {'MSE':<12}")
print("-" * 40)

for kernel, results in svr_results.items():
    print(f"{kernel:<10} {results['r2']:<10.4f} {results['mse']:<12.4f}")

print()

# Best SVR
best_kernel = max(svr_results.keys(), key=lambda x: svr_results[x]['r2'])
svr_best = SVR(kernel=best_kernel)
svr_best.fit(X_train_scaled, y_train)
svr_pred = svr_best.predict(X_test_scaled)

print(f"Best SVR Results (kernel={best_kernel}):")
print(f"R² Score: {r2_score(y_test, svr_pred):.4f}")
print(f"Mean Squared Error: {mean_squared_error(y_test, svr_pred):.4f}")
```

### K-Nearest Neighbors Regression
```python
# Test different k values
k_values = range(1, 21)
knn_results = {}

for k in k_values:
    knn = KNeighborsRegressor(n_neighbors=k)
    knn.fit(X_train_scaled, y_train)
    knn_pred = knn.predict(X_test_scaled)
    
    r2 = r2_score(y_test, knn_pred)
    mse = mean_squared_error(y_test, knn_pred)
    
    knn_results[k] = {
        'r2': r2,
        'mse': mse
    }

# Plot performance vs k
k_list = list(knn_results.keys())
r2_scores = [knn_results[k]['r2'] for k in k_list]
mse_scores = [knn_results[k]['mse'] for k in k_list]

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

ax1.plot(k_list, r2_scores, 'bo-', linewidth=2)
ax1.set_xlabel('Number of Neighbors (k)')
ax1.set_ylabel('R² Score')
ax1.set_title('KNN Regression: R² vs k')
ax1.grid(True, alpha=0.3)

ax2.plot(k_list, mse_scores, 'ro-', linewidth=2)
ax2.set_xlabel('Number of Neighbors (k)')
ax2.set_ylabel('MSE')
ax2.set_title('KNN Regression: MSE vs k')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Best KNN
best_k = max(knn_results.keys(), key=lambda x: knn_results[x]['r2'])
knn_best = KNeighborsRegressor(n_neighbors=best_k)
knn_best.fit(X_train_scaled, y_train)
knn_pred = knn_best.predict(X_test_scaled)

print(f"Best KNN Regression Results (k={best_k}):")
print(f"R² Score: {r2_score(y_test, knn_pred):.4f}")
print(f"Mean Squared Error: {mean_squared_error(y_test, knn_pred):.4f}")
```

### Model Comparison and Evaluation
```python
# Define all models with their best parameters
models = {
    'Linear Regression': LinearRegression(),
    'Polynomial (degree 2)': LinearRegression(),  # Will be fitted on polynomial features
    'Ridge': Ridge(alpha=best_ridge_alpha),
    'Lasso': Lasso(alpha=best_lasso_alpha),
    'Decision Tree': DecisionTreeRegressor(max_depth=best_depth, random_state=42),
    'Random Forest': RandomForestRegressor(n_estimators=best_n_est, random_state=42),
    'SVR': SVR(kernel=best_kernel),
    'KNN': KNeighborsRegressor(n_neighbors=best_k)
}

# Models that need scaling
scaled_models = ['Linear Regression', 'Ridge', 'Lasso', 'SVR', 'KNN']

# Evaluate all models
results = {}

for name, model in models.items():
    if name == 'Polynomial (degree 2)':
        # Special handling for polynomial
        poly = PolynomialFeatures(degree=2)
        X_train_poly = poly.fit_transform(X_train_scaled)
        X_test_poly = poly.transform(X_test_scaled)
        model.fit(X_train_poly, y_train)
        pred = model.predict(X_test_poly)
    elif name in scaled_models:
        model.fit(X_train_scaled, y_train)
        pred = model.predict(X_test_scaled)
    else:
        model.fit(X_train, y_train)
        pred = model.predict(X_test)
    
    # Calculate metrics
    r2 = r2_score(y_test, pred)
    mae = mean_absolute_error(y_test, pred)
    mse = mean_squared_error(y_test, pred)
    rmse = np.sqrt(mse)
    
    results[name] = {
        'r2': r2,
        'mae': mae,
        'mse': mse,
        'rmse': rmse
    }

# Display comparison
print("Regression Model Comparison:")
print("=" * 80)
print(f"{'Model':<20} {'R²':<10} {'MAE':<10} {'MSE':<12} {'RMSE':<10}")
print("-" * 80)

for name, metrics in results.items():
    print(f"{name:<20} {metrics['r2']:<10.4f} {metrics['mae']:<10.4f} {metrics['mse']:<12.4f} {metrics['rmse']:<10.4f}")

print()

# Find best model
best_model = max(results.keys(), key=lambda x: results[x]['r2'])
print(f"Best performing model: {best_model} (R² = {results[best_model]['r2']:.4f})")
print()

# Visualize model performance
model_names = list(results.keys())
r2_scores = [results[name]['r2'] for name in model_names]

plt.figure(figsize=(12, 6))
bars = plt.bar(model_names, r2_scores, color='skyblue', alpha=0.7)
plt.xlabel('Model')
plt.ylabel('R² Score')
plt.title('Model Performance Comparison (R² Score)')
plt.xticks(rotation=45, ha='right')
plt.grid(True, alpha=0.3)

# Add value labels on bars
for bar, score in zip(bars, r2_scores):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.01, 
             f'{score:.3f}', ha='center', va='bottom')

plt.tight_layout()
plt.show()
```

### Cross-Validation and Hyperparameter Tuning
```python
# Cross-validation for best model
best_model_name = best_model
if best_model_name == 'Polynomial (degree 2)':
    # Special handling for polynomial
    poly = PolynomialFeatures(degree=2)
    X_poly = poly.fit_transform(X_train_scaled)
    cv_scores = cross_val_score(LinearRegression(), X_poly, y_train, cv=5, scoring='r2')
elif best_model_name in scaled_models:
    cv_scores = cross_val_score(models[best_model_name], X_train_scaled, y_train, cv=5, scoring='r2')
else:
    cv_scores = cross_val_score(models[best_model_name], X_train, y_train, cv=5, scoring='r2')

print("Cross-Validation Results for Best Model:")
print(f"CV R² Scores: {cv_scores}")
print(f"Mean CV R² Score: {cv_scores.mean():.4f} (+/- {cv_scores.std() * 2:.4f})")
print()

# Hyperparameter tuning for Random Forest
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4]
}

rf_grid = GridSearchCV(RandomForestRegressor(random_state=42), param_grid, cv=3, scoring='r2', n_jobs=-1)
rf_grid.fit(X_train, y_train)

print("Random Forest Hyperparameter Tuning:")
print(f"Best parameters: {rf_grid.best_params_}")
print(f"Best CV R² score: {rf_grid.best_score_:.4f}")
print()

# Evaluate tuned model
rf_tuned_pred = rf_grid.predict(X_test)
print(f"Tuned Random Forest Test R²: {r2_score(y_test, rf_tuned_pred):.4f}")
print(f"Tuned Random Forest Test RMSE: {np.sqrt(mean_squared_error(y_test, rf_tuned_pred)):.4f}")
```

## Tasks

Create a Python file named `regression_algorithms.py` in this folder and implement the following:

1. Implement multiple regression algorithms (Linear, Polynomial, Ridge, Lasso, Decision Tree, Random Forest, SVR, KNN)
2. Evaluate models using regression metrics (R², MAE, MSE, RMSE)
3. Perform cross-validation and hyperparameter tuning
4. Visualize model performance and residual analysis
5. Compare different regression algorithms and select the best one

## Additional Resources

- Scikit-learn regression documentation
- Regression model evaluation metrics
- Choosing the right regression algorithm