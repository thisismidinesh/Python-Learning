# Model Evaluation and Validation in Python

## Introduction

Model evaluation and validation are crucial steps in machine learning to assess how well a model performs on unseen data. This involves using appropriate metrics, cross-validation techniques, and understanding the bias-variance tradeoff to ensure models generalize well.

## Key Concepts

### Evaluation Metrics

#### Classification Metrics
- **Accuracy**: Fraction of correct predictions
- **Precision**: True positives / (True positives + False positives)
- **Recall (Sensitivity)**: True positives / (True positives + False negatives)
- **F1-Score**: Harmonic mean of precision and recall
- **ROC-AUC**: Area under the Receiver Operating Characteristic curve
- **PR-AUC**: Area under the Precision-Recall curve

#### Regression Metrics
- **Mean Absolute Error (MAE)**: Average absolute errors
- **Mean Squared Error (MSE)**: Average squared errors
- **Root Mean Squared Error (RMSE)**: Square root of MSE
- **R² Score**: Proportion of variance explained
- **Mean Absolute Percentage Error (MAPE)**: Average absolute percentage errors

### Validation Techniques
- **Train/Test Split**: Simple holdout validation
- **K-Fold Cross-Validation**: Divide data into k folds
- **Stratified K-Fold**: Maintains class distribution
- **Leave-One-Out**: Each sample is a test set
- **Time Series Split**: For temporal data

### Bias-Variance Tradeoff
- **Bias**: Error from incorrect assumptions
- **Variance**: Error from sensitivity to training data
- **Overfitting**: Low bias, high variance
- **Underfitting**: High bias, low variance

### Hyperparameter Tuning
- **Grid Search**: Exhaustive search over parameter combinations
- **Random Search**: Random sampling of parameter space
- **Bayesian Optimization**: Probabilistic model-based optimization

## Examples

### Data Preparation and Basic Evaluation
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import make_classification, make_regression, load_breast_cancer, load_diabetes
from sklearn.model_selection import (train_test_split, cross_val_score, 
                                   KFold, StratifiedKFold, TimeSeriesSplit,
                                   GridSearchCV, RandomizedSearchCV,
                                   validation_curve, learning_curve)
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import (accuracy_score, precision_score, recall_score, f1_score,
                           roc_auc_score, roc_curve, auc, precision_recall_curve,
                           mean_absolute_error, mean_squared_error, r2_score,
                           classification_report, confusion_matrix,
                           mean_absolute_percentage_error)
from sklearn.linear_model import LogisticRegression, LinearRegression, Ridge, Lasso
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
from sklearn.svm import SVC, SVR
from sklearn.neighbors import KNeighborsClassifier, KNeighborsRegressor
import warnings
warnings.filterwarnings('ignore')

# Load datasets
print("Loading datasets...")
cancer = load_breast_cancer()
diabetes = load_diabetes()

# Classification data
X_clf, y_clf = cancer.data, cancer.target
feature_names_clf = cancer.feature_names

# Regression data
X_reg, y_reg = diabetes.data, diabetes.target
feature_names_reg = diabetes.feature_names

print("Classification dataset:", X_clf.shape, "Target distribution:", np.bincount(y_clf))
print("Regression dataset:", X_reg.shape)
print()

# Split data
X_clf_train, X_clf_test, y_clf_train, y_clf_test = train_test_split(
    X_clf, y_clf, test_size=0.2, random_state=42, stratify=y_clf)

X_reg_train, X_reg_test, y_reg_train, y_reg_test = train_test_split(
    X_reg, y_reg, test_size=0.2, random_state=42)

print("Classification splits:")
print("Train:", X_clf_train.shape, "Test:", X_clf_test.shape)
print("Train target distribution:", np.bincount(y_clf_train))
print("Test target distribution:", np.bincount(y_clf_test))
print()

print("Regression splits:")
print("Train:", X_reg_train.shape, "Test:", X_reg_test.shape)
print()
```

### Classification Model Evaluation
```python
# Train multiple classification models
clf_models = {
    'Logistic Regression': LogisticRegression(random_state=42, max_iter=1000),
    'Decision Tree': DecisionTreeClassifier(random_state=42),
    'Random Forest': RandomForestClassifier(n_estimators=100, random_state=42),
    'SVM': SVC(probability=True, random_state=42),
    'KNN': KNeighborsClassifier(n_neighbors=5)
}

# Scale data for SVM and KNN
scaler_clf = StandardScaler()
X_clf_train_scaled = scaler_clf.fit_transform(X_clf_train)
X_clf_test_scaled = scaler_clf.transform(X_clf_test)

clf_results = {}

for name, model in clf_models.items():
    print(f"Training {name}...")
    
    # Use scaled data for SVM and KNN
    if name in ['SVM', 'KNN']:
        model.fit(X_clf_train_scaled, y_clf_train)
        y_pred = model.predict(X_clf_test_scaled)
        y_pred_proba = model.predict_proba(X_clf_test_scaled)[:, 1]
    else:
        model.fit(X_clf_train, y_clf_train)
        y_pred = model.predict(X_clf_test)
        y_pred_proba = model.predict_proba(X_clf_test)[:, 1]
    
    # Calculate metrics
    accuracy = accuracy_score(y_clf_test, y_pred)
    precision = precision_score(y_clf_test, y_pred)
    recall = recall_score(y_clf_test, y_pred)
    f1 = f1_score(y_clf_test, y_pred)
    roc_auc = roc_auc_score(y_clf_test, y_pred_proba)
    
    clf_results[name] = {
        'accuracy': accuracy,
        'precision': precision,
        'recall': recall,
        'f1': f1,
        'roc_auc': roc_auc,
        'predictions': y_pred,
        'probabilities': y_pred_proba
    }
    
    print(f"  Accuracy: {accuracy:.4f}")
    print(f"  Precision: {precision:.4f}")
    print(f"  Recall: {recall:.4f}")
    print(f"  F1-Score: {f1:.4f}")
    print(f"  ROC-AUC: {roc_auc:.4f}")
    print()

# Display classification results
print("Classification Model Comparison:")
print("=" * 80)
print(f"{'Model':<20} {'Accuracy':<10} {'Precision':<10} {'Recall':<10} {'F1':<10} {'ROC-AUC':<10}")
print("-" * 80)

for name, results in clf_results.items():
    print(f"{name:<20} {results['accuracy']:<10.4f} {results['precision']:<10.4f} {results['recall']:<10.4f} {results['f1']:<10.4f} {results['roc_auc']:<10.4f}")

print()

# Confusion Matrix for best model
best_clf_model = max(clf_results.keys(), key=lambda x: clf_results[x]['f1'])
best_clf_results = clf_results[best_clf_model]

print(f"Confusion Matrix for {best_clf_model}:")
cm = confusion_matrix(y_clf_test, best_clf_results['predictions'])
print(cm)
print()

# Classification report
print(f"Classification Report for {best_clf_model}:")
print(classification_report(y_clf_test, best_clf_results['predictions'], target_names=cancer.target_names))
print()

# ROC Curves
plt.figure(figsize=(10, 8))

for name, results in clf_results.items():
    fpr, tpr, _ = roc_curve(y_clf_test, results['probabilities'])
    roc_auc = auc(fpr, tpr)
    plt.plot(fpr, tpr, label=f'{name} (AUC = {roc_auc:.3f})')

plt.plot([0, 1], [0, 1], 'k--', label='Random Classifier')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curves for Classification Models')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# Precision-Recall Curves
plt.figure(figsize=(10, 8))

for name, results in clf_results.items():
    precision_curve, recall_curve, _ = precision_recall_curve(y_clf_test, results['probabilities'])
    pr_auc = auc(recall_curve, precision_curve)
    plt.plot(recall_curve, precision_curve, label=f'{name} (AUC = {pr_auc:.3f})')

plt.xlabel('Recall')
plt.ylabel('Precision')
plt.title('Precision-Recall Curves for Classification Models')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Regression Model Evaluation
```python
# Train multiple regression models
reg_models = {
    'Linear Regression': LinearRegression(),
    'Ridge': Ridge(alpha=1.0),
    'Lasso': Lasso(alpha=0.1),
    'Decision Tree': DecisionTreeRegressor(random_state=42),
    'Random Forest': RandomForestRegressor(n_estimators=100, random_state=42),
    'SVR': SVR(kernel='rbf'),
    'KNN': KNeighborsRegressor(n_neighbors=5)
}

# Scale data for SVR and KNN
scaler_reg = StandardScaler()
X_reg_train_scaled = scaler_reg.fit_transform(X_reg_train)
X_reg_test_scaled = scaler_reg.transform(X_reg_test)

reg_results = {}

for name, model in reg_models.items():
    print(f"Training {name}...")
    
    # Use scaled data for SVR and KNN
    if name in ['SVR', 'KNN']:
        model.fit(X_reg_train_scaled, y_reg_train)
        y_pred = model.predict(X_reg_test_scaled)
    else:
        model.fit(X_reg_train, y_reg_train)
        y_pred = model.predict(X_reg_test)
    
    # Calculate metrics
    mae = mean_absolute_error(y_reg_test, y_pred)
    mse = mean_squared_error(y_reg_test, y_pred)
    rmse = np.sqrt(mse)
    r2 = r2_score(y_reg_test, y_pred)
    mape = mean_absolute_percentage_error(y_reg_test, y_pred)
    
    reg_results[name] = {
        'mae': mae,
        'mse': mse,
        'rmse': rmse,
        'r2': r2,
        'mape': mape,
        'predictions': y_pred
    }
    
    print(f"  MAE: {mae:.4f}")
    print(f"  MSE: {mse:.4f}")
    print(f"  RMSE: {rmse:.4f}")
    print(f"  R²: {r2:.4f}")
    print(f"  MAPE: {mape:.4f}")
    print()

# Display regression results
print("Regression Model Comparison:")
print("=" * 80)
print(f"{'Model':<15} {'MAE':<10} {'MSE':<12} {'RMSE':<10} {'R²':<10} {'MAPE':<10}")
print("-" * 80)

for name, results in reg_results.items():
    print(f"{name:<15} {results['mae']:<10.4f} {results['mse']:<12.4f} {results['rmse']:<10.4f} {results['r2']:<10.4f} {results['mape']:<10.4f}")

print()

# Residual analysis for best regression model
best_reg_model = max(reg_results.keys(), key=lambda x: reg_results[x]['r2'])
best_reg_results = reg_results[best_reg_model]

residuals = y_reg_test - best_reg_results['predictions']

fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Residuals vs Predicted
axes[0,0].scatter(best_reg_results['predictions'], residuals, alpha=0.6)
axes[0,0].axhline(y=0, color='r', linestyle='--')
axes[0,0].set_xlabel('Predicted Values')
axes[0,0].set_ylabel('Residuals')
axes[0,0].set_title(f'Residuals vs Predicted ({best_reg_model})')
axes[0,0].grid(True, alpha=0.3)

# Residuals distribution
axes[0,1].hist(residuals, bins=30, alpha=0.7, color='skyblue')
axes[0,1].set_xlabel('Residuals')
axes[0,1].set_ylabel('Frequency')
axes[0,1].set_title('Residual Distribution')
axes[0,1].grid(True, alpha=0.3)

# Q-Q plot
from scipy import stats
stats.probplot(residuals, dist="norm", plot=axes[1,0])
axes[1,0].set_title('Q-Q Plot of Residuals')

# Predicted vs Actual
axes[1,1].scatter(y_reg_test, best_reg_results['predictions'], alpha=0.6)
axes[1,1].plot([y_reg_test.min(), y_reg_test.max()], [y_reg_test.min(), y_reg_test.max()], 'r--')
axes[1,1].set_xlabel('Actual Values')
axes[1,1].set_ylabel('Predicted Values')
axes[1,1].set_title('Predicted vs Actual')
axes[1,1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Cross-Validation Techniques
```python
# Cross-validation for classification
print("Cross-Validation for Classification:")
print("-" * 40)

cv_scores_clf = {}

for name, model in clf_models.items():
    if name in ['SVM', 'KNN']:
        scores = cross_val_score(model, X_clf_train_scaled, y_clf_train, cv=5, scoring='f1')
    else:
        scores = cross_val_score(model, X_clf_train, y_clf_train, cv=5, scoring='f1')
    
    cv_scores_clf[name] = scores
    print(f"{name}: Mean F1 = {scores.mean():.4f} (+/- {scores.std() * 2:.4f})")

print()

# Cross-validation for regression
print("Cross-Validation for Regression:")
print("-" * 40)

cv_scores_reg = {}

for name, model in reg_models.items():
    if name in ['SVR', 'KNN']:
        scores = cross_val_score(model, X_reg_train_scaled, y_reg_train, cv=5, scoring='r2')
    else:
        scores = cross_val_score(model, X_reg_train, y_reg_train, cv=5, scoring='r2')
    
    cv_scores_reg[name] = scores
    print(f"{name}: Mean R² = {scores.mean():.4f} (+/- {scores.std() * 2:.4f})")

print()

# Different cross-validation strategies
print("Different Cross-Validation Strategies:")
print("-" * 45)

# Stratified K-Fold for classification
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
stratified_scores = cross_val_score(
    RandomForestClassifier(n_estimators=100, random_state=42), 
    X_clf_train, y_clf_train, cv=skf, scoring='f1')

print(f"Stratified K-Fold (Classification): Mean F1 = {stratified_scores.mean():.4f} (+/- {stratified_scores.std() * 2:.4f})")

# K-Fold for regression
kf = KFold(n_splits=5, shuffle=True, random_state=42)
kfold_scores = cross_val_score(
    RandomForestRegressor(n_estimators=100, random_state=42), 
    X_reg_train, y_reg_train, cv=kf, scoring='r2')

print(f"K-Fold (Regression): Mean R² = {kfold_scores.mean():.4f} (+/- {kfold_scores.std() * 2:.4f})")

# Visualize cross-validation scores
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Classification CV scores
clf_names = list(cv_scores_clf.keys())
clf_means = [cv_scores_clf[name].mean() for name in clf_names]
clf_stds = [cv_scores_clf[name].std() for name in clf_names]

ax1.bar(clf_names, clf_means, yerr=clf_stds, capsize=5, alpha=0.7, color='skyblue')
ax1.set_ylabel('F1 Score')
ax1.set_title('Cross-Validation Scores (Classification)')
ax1.tick_params(axis='x', rotation=45)
ax1.grid(True, alpha=0.3)

# Regression CV scores
reg_names = list(cv_scores_reg.keys())
reg_means = [cv_scores_reg[name].mean() for name in reg_names]
reg_stds = [cv_scores_reg[name].std() for name in reg_names]

ax2.bar(reg_names, reg_means, yerr=reg_stds, capsize=5, alpha=0.7, color='lightcoral')
ax2.set_ylabel('R² Score')
ax2.set_title('Cross-Validation Scores (Regression)')
ax2.tick_params(axis='x', rotation=45)
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Hyperparameter Tuning
```python
# Grid Search for Random Forest Classification
print("Hyperparameter Tuning - Random Forest Classification:")
print("-" * 55)

rf_clf = RandomForestClassifier(random_state=42)

param_grid_clf = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 10, 20, 30],
    'min_samples_split': [2, 5, 10],
    'min_samples_leaf': [1, 2, 4]
}

grid_search_clf = GridSearchCV(
    rf_clf, param_grid_clf, cv=3, scoring='f1', n_jobs=-1, verbose=1
)
grid_search_clf.fit(X_clf_train, y_clf_train)

print(f"Best parameters: {grid_search_clf.best_params_}")
print(f"Best CV score: {grid_search_clf.best_score_:.4f}")
print()

# Evaluate best model
best_rf_clf = grid_search_clf.best_estimator_
y_pred_best_clf = best_rf_clf.predict(X_clf_test)
print("Test set performance with best parameters:")
print(f"Accuracy: {accuracy_score(y_clf_test, y_pred_best_clf):.4f}")
print(f"F1-Score: {f1_score(y_clf_test, y_pred_best_clf):.4f}")
print()

# Random Search for Random Forest Regression
print("Hyperparameter Tuning - Random Forest Regression:")
print("-" * 50)

rf_reg = RandomForestRegressor(random_state=42)

param_dist_reg = {
    'n_estimators': [50, 100, 150, 200],
    'max_depth': [None, 10, 20, 30, 40],
    'min_samples_split': [2, 5, 10, 15],
    'min_samples_leaf': [1, 2, 4, 6],
    'max_features': ['auto', 'sqrt', 'log2']
}

random_search_reg = RandomizedSearchCV(
    rf_reg, param_dist_reg, n_iter=20, cv=3, scoring='r2', 
    n_jobs=-1, random_state=42, verbose=1
)
random_search_reg.fit(X_reg_train, y_reg_train)

print(f"Best parameters: {random_search_reg.best_params_}")
print(f"Best CV score: {random_search_reg.best_score_:.4f}")
print()

# Evaluate best model
best_rf_reg = random_search_reg.best_estimator_
y_pred_best_reg = best_rf_reg.predict(X_reg_test)
print("Test set performance with best parameters:")
print(f"R²: {r2_score(y_reg_test, y_pred_best_reg):.4f}")
print(f"RMSE: {np.sqrt(mean_squared_error(y_reg_test, y_pred_best_reg)):.4f}")
print()
```

### Learning and Validation Curves
```python
# Learning curves for classification
from sklearn.model_selection import learning_curve

print("Learning Curves:")
print("-" * 20)

def plot_learning_curve(estimator, title, X, y, cv=None, n_jobs=None, train_sizes=np.linspace(.1, 1.0, 5)):
    plt.figure(figsize=(10, 6))
    plt.title(title)
    plt.xlabel("Training examples")
    plt.ylabel("Score")
    
    train_sizes, train_scores, validation_scores = learning_curve(
        estimator, X, y, cv=cv, n_jobs=n_jobs, train_sizes=train_sizes, scoring='f1')
    
    train_scores_mean = np.mean(train_scores, axis=1)
    train_scores_std = np.std(train_scores, axis=1)
    validation_scores_mean = np.mean(validation_scores, axis=1)
    validation_scores_std = np.std(validation_scores, axis=1)
    
    plt.fill_between(train_sizes, train_scores_mean - train_scores_std,
                     train_scores_mean + train_scores_std, alpha=0.1, color="r")
    plt.fill_between(train_sizes, validation_scores_mean - validation_scores_std,
                     validation_scores_mean + validation_scores_std, alpha=0.1, color="g")
    plt.plot(train_sizes, train_scores_mean, 'o-', color="r", label="Training score")
    plt.plot(train_sizes, validation_scores_mean, 'o-', color="g", label="Cross-validation score")
    
    plt.legend(loc="best")
    plt.grid(True, alpha=0.3)
    plt.show()

# Plot learning curve for Random Forest
plot_learning_curve(
    RandomForestClassifier(n_estimators=100, random_state=42), 
    "Learning Curve (Random Forest Classification)", 
    X_clf_train, y_clf_train, cv=5
)

# Validation curves
from sklearn.model_selection import validation_curve

def plot_validation_curve(estimator, title, X, y, param_name, param_range, cv=None, scoring='f1'):
    train_scores, validation_scores = validation_curve(
        estimator, X, y, param_name=param_name, param_range=param_range,
        cv=cv, scoring=scoring)
    
    train_scores_mean = np.mean(train_scores, axis=1)
    train_scores_std = np.std(train_scores, axis=1)
    validation_scores_mean = np.mean(validation_scores, axis=1)
    validation_scores_std = np.std(validation_scores, axis=1)
    
    plt.figure(figsize=(10, 6))
    plt.title(title)
    plt.xlabel(param_name)
    plt.ylabel(scoring)
    
    plt.plot(param_range, train_scores_mean, 'o-', color="r", label="Training score")
    plt.plot(param_range, validation_scores_mean, 'o-', color="g", label="Cross-validation score")
    
    plt.fill_between(param_range, train_scores_mean - train_scores_std,
                     train_scores_mean + train_scores_std, alpha=0.1, color="r")
    plt.fill_between(param_range, validation_scores_mean - validation_scores_std,
                     validation_scores_mean + validation_scores_std, alpha=0.1, color="g")
    
    plt.legend(loc="best")
    plt.grid(True, alpha=0.3)
    plt.show()

# Plot validation curve for max_depth
plot_validation_curve(
    RandomForestClassifier(n_estimators=100, random_state=42),
    "Validation Curve (Random Forest - max_depth)",
    X_clf_train, y_clf_train, 
    param_name="max_depth", 
    param_range=[3, 5, 7, 9, 11, 13, 15, None],
    cv=5
)
```

### Model Interpretability and Diagnostics
```python
# Feature importance analysis
print("Feature Importance Analysis:")
print("-" * 30)

# For classification
rf_clf_best = grid_search_clf.best_estimator_
clf_importance = pd.DataFrame({
    'feature': feature_names_clf,
    'importance': rf_clf_best.feature_importances_
}).sort_values('importance', ascending=False)

print("Top 10 features (Classification):")
print(clf_importance.head(10))
print()

# For regression
rf_reg_best = random_search_reg.best_estimator_
reg_importance = pd.DataFrame({
    'feature': feature_names_reg,
    'importance': rf_reg_best.feature_importances_
}).sort_values('importance', ascending=False)

print("Top 10 features (Regression):")
print(reg_importance.head(10))
print()

# Visualize feature importances
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Classification
ax1.barh(clf_importance['feature'][:10], clf_importance['importance'][:10], color='skyblue')
ax1.set_xlabel('Importance')
ax1.set_title('Feature Importance (Classification)')
ax1.grid(True, alpha=0.3)

# Regression
ax2.barh(reg_importance['feature'][:10], reg_importance['importance'][:10], color='lightcoral')
ax2.set_xlabel('Importance')
ax2.set_title('Feature Importance (Regression)')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Model calibration for classification
from sklearn.calibration import calibration_curve

plt.figure(figsize=(10, 8))

for name, results in clf_results.items():
    if hasattr(clf_models[name], 'predict_proba'):
        prob_true, prob_pred = calibration_curve(y_clf_test, results['probabilities'], n_bins=10)
        plt.plot(prob_pred, prob_true, marker='o', label=name)

plt.plot([0, 1], [0, 1], 'k--', label='Perfectly calibrated')
plt.xlabel('Mean predicted probability')
plt.ylabel('Fraction of positives')
plt.title('Calibration Curves')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## Tasks

Create a Python file named `model_evaluation.py` in this folder and implement the following:

1. Implement comprehensive model evaluation for both classification and regression
2. Use cross-validation techniques (K-fold, stratified K-fold)
3. Perform hyperparameter tuning (Grid Search, Random Search)
4. Create learning curves and validation curves
5. Analyze feature importance and model calibration
6. Compare different evaluation metrics and validation strategies

## Additional Resources

- Scikit-learn model evaluation documentation
- Cross-validation techniques
- Hyperparameter tuning best practices
- Model interpretability methods