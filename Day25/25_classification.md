# Classification Algorithms in Python

## Introduction

Classification is a supervised machine learning technique used to predict categorical labels or classes. Python's scikit-learn library provides a comprehensive set of classification algorithms. Understanding different algorithms and their strengths helps in choosing the right approach for specific problems.

## Key Concepts

### Types of Classification
- **Binary Classification**: Two classes (e.g., spam/not spam)
- **Multi-class Classification**: More than two classes (e.g., iris species)
- **Multi-label Classification**: Multiple labels per instance

### Common Algorithms
- **Logistic Regression**: Linear model for binary/multi-class classification
- **Decision Trees**: Tree-based model with if-then rules
- **Random Forest**: Ensemble of decision trees
- **Support Vector Machines (SVM)**: Finds optimal hyperplane
- **K-Nearest Neighbors (KNN)**: Instance-based learning
- **Naive Bayes**: Probabilistic classifier based on Bayes' theorem

### Evaluation Metrics
- **Accuracy**: Fraction of correct predictions
- **Precision**: True positives / (True positives + False positives)
- **Recall**: True positives / (True positives + False negatives)
- **F1-Score**: Harmonic mean of precision and recall
- **ROC-AUC**: Area under Receiver Operating Characteristic curve

## Examples

### Data Preparation and Exploration
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import load_iris, make_classification
from sklearn.model_selection import train_test_split, cross_val_score, GridSearchCV
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.metrics import (accuracy_score, precision_score, recall_score, f1_score, 
                           classification_report, confusion_matrix, roc_curve, auc)
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.neighbors import KNeighborsClassifier
from sklearn.naive_bayes import GaussianNB

# Load iris dataset
iris = load_iris()
X = iris.data
y = iris.target
feature_names = iris.feature_names
target_names = iris.target_names

print("Dataset Information:")
print(f"Features: {feature_names}")
print(f"Target classes: {target_names}")
print(f"Dataset shape: {X.shape}")
print(f"Target distribution: {np.bincount(y)}")
print()

# Create DataFrame for easier analysis
df = pd.DataFrame(X, columns=feature_names)
df['species'] = y
df['species_name'] = df['species'].map({0: 'setosa', 1: 'versicolor', 2: 'virginica'})

print("First 5 rows:")
print(df.head())
print()

# Basic statistics
print("Feature statistics by species:")
print(df.groupby('species_name')[feature_names].mean())
print()

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42, stratify=y)

print("Training set shape:", X_train.shape)
print("Testing set shape:", X_test.shape)
print()

# Scale the features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

### Logistic Regression
```python
# Train Logistic Regression
lr = LogisticRegression(random_state=42, max_iter=1000)
lr.fit(X_train_scaled, y_train)

# Make predictions
lr_pred = lr.predict(X_test_scaled)
lr_pred_proba = lr.predict_proba(X_test_scaled)

print("Logistic Regression Results:")
print(f"Accuracy: {accuracy_score(y_test, lr_pred):.3f}")
print(f"Precision (macro): {precision_score(y_test, lr_pred, average='macro'):.3f}")
print(f"Recall (macro): {recall_score(y_test, lr_pred, average='macro'):.3f}")
print(f"F1-Score (macro): {f1_score(y_test, lr_pred, average='macro'):.3f}")
print()

print("Classification Report:")
print(classification_report(y_test, lr_pred, target_names=target_names))
print()

print("Confusion Matrix:")
cm = confusion_matrix(y_test, lr_pred)
print(cm)
print()

# Feature importance (coefficients)
print("Feature Coefficients:")
for i, feature in enumerate(feature_names):
    print(f"{feature}: {lr.coef_[:, i]}")
print()
```

### Decision Trees
```python
# Train Decision Tree
dt = DecisionTreeClassifier(random_state=42, max_depth=3)
dt.fit(X_train, y_train)  # Decision trees don't need scaling

# Make predictions
dt_pred = dt.predict(X_test)

print("Decision Tree Results:")
print(f"Accuracy: {accuracy_score(y_test, dt_pred):.3f}")
print(f"Tree depth: {dt.get_depth()}")
print(f"Number of leaves: {dt.get_n_leaves()}")
print()

print("Feature Importances:")
for feature, importance in zip(feature_names, dt.feature_importances_):
    print(f"{feature}: {importance:.3f}")
print()

# Visualize decision tree (if graphviz is available)
try:
    from sklearn.tree import plot_tree
    plt.figure(figsize=(12, 8))
    plot_tree(dt, feature_names=feature_names, class_names=target_names, 
              filled=True, rounded=True, fontsize=10)
    plt.title("Decision Tree Visualization")
    plt.show()
except ImportError:
    print("Graphviz not available for tree visualization")
```

### Random Forest
```python
# Train Random Forest
rf = RandomForestClassifier(n_estimators=100, random_state=42, max_depth=3)
rf.fit(X_train, y_train)

# Make predictions
rf_pred = rf.predict(X_test)

print("Random Forest Results:")
print(f"Accuracy: {accuracy_score(y_test, rf_pred):.3f}")
print()

print("Feature Importances:")
for feature, importance in zip(feature_names, rf.feature_importances_):
    print(f"{feature}: {importance:.3f}")
print()

# Individual tree analysis
print("Individual tree statistics:")
tree_depths = [estimator.get_depth() for estimator in rf.estimators_]
tree_leaves = [estimator.get_n_leaves() for estimator in rf.estimators_]
print(f"Average tree depth: {np.mean(tree_depths):.1f}")
print(f"Average number of leaves: {np.mean(tree_leaves):.1f}")
print(f"Tree depth range: {min(tree_depths)} - {max(tree_depths)}")
```

### Support Vector Machines
```python
# Train SVM with different kernels
kernels = ['linear', 'rbf', 'poly']
svm_results = {}

for kernel in kernels:
    svm = SVC(kernel=kernel, random_state=42, probability=True)
    svm.fit(X_train_scaled, y_train)
    svm_pred = svm.predict(X_test_scaled)
    accuracy = accuracy_score(y_test, svm_pred)
    svm_results[kernel] = accuracy
    
    print(f"SVM ({kernel} kernel) - Accuracy: {accuracy:.3f}")

print()

# Best SVM model
best_kernel = max(svm_results, key=svm_results.get)
svm_best = SVC(kernel=best_kernel, random_state=42, probability=True)
svm_best.fit(X_train_scaled, y_train)
svm_pred = svm_best.predict(X_test_scaled)

print(f"Best SVM Results ({best_kernel} kernel):")
print(f"Accuracy: {accuracy_score(y_test, svm_pred):.3f}")
print()

# SVM decision function (for binary classification, we'd show this)
# For multi-class, we can show prediction probabilities
svm_proba = svm_best.predict_proba(X_test_scaled)
print("Prediction probabilities for first test sample:")
print(dict(zip(target_names, svm_proba[0])))
```

### K-Nearest Neighbors
```python
# Test different k values
k_values = range(1, 11)
knn_accuracies = []

for k in k_values:
    knn = KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train_scaled, y_train)
    knn_pred = knn.predict(X_test_scaled)
    accuracy = accuracy_score(y_test, knn_pred)
    knn_accuracies.append(accuracy)

# Plot accuracy vs k
plt.figure(figsize=(8, 6))
plt.plot(k_values, knn_accuracies, 'bo-', linewidth=2, markersize=8)
plt.xlabel('Number of Neighbors (k)')
plt.ylabel('Accuracy')
plt.title('KNN Accuracy vs Number of Neighbors')
plt.grid(True, alpha=0.3)
plt.xticks(k_values)
plt.show()

# Best k
best_k = k_values[np.argmax(knn_accuracies)]
knn_best = KNeighborsClassifier(n_neighbors=best_k)
knn_best.fit(X_train_scaled, y_train)
knn_pred = knn_best.predict(X_test_scaled)

print(f"Best KNN Results (k={best_k}):")
print(f"Accuracy: {accuracy_score(y_test, knn_pred):.3f}")
print()

# Distance metrics comparison
metrics = ['euclidean', 'manhattan', 'minkowski']
for metric in metrics:
    knn = KNeighborsClassifier(n_neighbors=best_k, metric=metric)
    knn.fit(X_train_scaled, y_train)
    knn_pred = knn.predict(X_test_scaled)
    accuracy = accuracy_score(y_test, knn_pred)
    print(f"KNN ({metric} distance) - Accuracy: {accuracy:.3f}")
```

### Naive Bayes
```python
# Train Naive Bayes
nb = GaussianNB()
nb.fit(X_train, y_train)  # NB doesn't need scaling

# Make predictions
nb_pred = nb.predict(X_test)

print("Naive Bayes Results:")
print(f"Accuracy: {accuracy_score(y_test, nb_pred):.3f}")
print()

# Class priors and likelihood parameters
print("Class priors:")
for i, prior in enumerate(nb.class_prior_):
    print(f"{target_names[i]}: {prior:.3f}")

print()
print("Class means:")
for i, mean in enumerate(nb.theta_):
    print(f"{target_names[i]}: {dict(zip(feature_names, mean))}")

print()
print("Class variances:")
for i, var in enumerate(nb.var_):
    print(f"{target_names[i]}: {dict(zip(feature_names, var))}")
```

### Model Comparison and Evaluation
```python
# Define all models
models = {
    'Logistic Regression': LogisticRegression(random_state=42, max_iter=1000),
    'Decision Tree': DecisionTreeClassifier(random_state=42),
    'Random Forest': RandomForestClassifier(n_estimators=100, random_state=42),
    'SVM': SVC(random_state=42),
    'KNN': KNeighborsClassifier(n_neighbors=best_k),
    'Naive Bayes': GaussianNB()
}

# Models that need scaling
scaled_models = ['Logistic Regression', 'SVM', 'KNN']

# Evaluate all models
results = {}

for name, model in models.items():
    # Use scaled data for models that need it
    if name in scaled_models:
        model.fit(X_train_scaled, y_train)
        pred = model.predict(X_test_scaled)
        pred_proba = model.predict_proba(X_test_scaled) if hasattr(model, 'predict_proba') else None
    else:
        model.fit(X_train, y_train)
        pred = model.predict(X_test)
        pred_proba = model.predict_proba(X_test) if hasattr(model, 'predict_proba') else None
    
    # Calculate metrics
    accuracy = accuracy_score(y_test, pred)
    precision = precision_score(y_test, pred, average='macro')
    recall = recall_score(y_test, pred, average='macro')
    f1 = f1_score(y_test, pred, average='macro')
    
    results[name] = {
        'accuracy': accuracy,
        'precision': precision,
        'recall': recall,
        'f1': f1,
        'predictions': pred,
        'probabilities': pred_proba
    }

# Display comparison
print("Model Comparison:")
print("=" * 80)
print(f"{'Model':<20} {'Accuracy':<10} {'Precision':<10} {'Recall':<10} {'F1-Score':<10}")
print("-" * 80)

for name, metrics in results.items():
    print(f"{name:<20} {metrics['accuracy']:<10.3f} {metrics['precision']:<10.3f} {metrics['recall']:<10.3f} {metrics['f1']:<10.3f}")

print()

# Find best model
best_model = max(results.keys(), key=lambda x: results[x]['accuracy'])
print(f"Best performing model: {best_model} (Accuracy: {results[best_model]['accuracy']:.3f})")
```

### Cross-Validation and Hyperparameter Tuning
```python
# Cross-validation for best model
best_model_instance = models[best_model]
if best_model in scaled_models:
    cv_scores = cross_val_score(best_model_instance, X_train_scaled, y_train, cv=5, scoring='accuracy')
else:
    cv_scores = cross_val_score(best_model_instance, X_train, y_train, cv=5, scoring='accuracy')

print("Cross-Validation Results for Best Model:")
print(f"CV Scores: {cv_scores}")
print(f"Mean CV Score: {cv_scores.mean():.3f} (+/- {cv_scores.std() * 2:.3f})")
print()

# Hyperparameter tuning for Random Forest
param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 3, 5, 10],
    'min_samples_split': [2, 5, 10]
}

rf_grid = GridSearchCV(RandomForestClassifier(random_state=42), param_grid, cv=3, scoring='accuracy')
rf_grid.fit(X_train, y_train)

print("Random Forest Hyperparameter Tuning:")
print(f"Best parameters: {rf_grid.best_params_}")
print(f"Best CV score: {rf_grid.best_score_:.3f}")
print()

# Evaluate tuned model
rf_tuned_pred = rf_grid.predict(X_test)
print(f"Tuned Random Forest Test Accuracy: {accuracy_score(y_test, rf_tuned_pred):.3f}")
```

### Confusion Matrix Visualization
```python
# Plot confusion matrices for all models
fig, axes = plt.subplots(2, 3, figsize=(15, 10))
axes = axes.ravel()

for i, (name, metrics) in enumerate(results.items()):
    cm = confusion_matrix(y_test, metrics['predictions'])
    
    # Plot confusion matrix
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=axes[i],
                xticklabels=target_names, yticklabels=target_names)
    axes[i].set_title(f'{name}\nAccuracy: {metrics["accuracy"]:.3f}')
    axes[i].set_xlabel('Predicted')
    axes[i].set_ylabel('Actual')

plt.tight_layout()
plt.show()
```

### ROC Curves (for binary classification)
```python
# Create binary classification problem for demonstration
X_binary = X[y != 2]  # Remove virginica class
y_binary = y[y != 2]  # 0 for setosa, 1 for versicolor

X_train_bin, X_test_bin, y_train_bin, y_test_bin = train_test_split(
    X_binary, y_binary, test_size=0.3, random_state=42)

X_train_bin_scaled = scaler.fit_transform(X_train_bin)
X_test_bin_scaled = scaler.transform(X_test_bin)

# Train models for binary classification
binary_models = {
    'Logistic Regression': LogisticRegression(random_state=42),
    'Random Forest': RandomForestClassifier(n_estimators=100, random_state=42),
    'SVM': SVC(random_state=42, probability=True)
}

plt.figure(figsize=(8, 6))

for name, model in binary_models.items():
    if name == 'SVM':
        model.fit(X_train_bin_scaled, y_train_bin)
        y_proba = model.predict_proba(X_test_bin_scaled)[:, 1]
    else:
        model.fit(X_train_bin, y_train_bin)
        y_proba = model.predict_proba(X_test_bin)[:, 1]
    
    fpr, tpr, _ = roc_curve(y_test_bin, y_proba)
    roc_auc = auc(fpr, tpr)
    
    plt.plot(fpr, tpr, label=f'{name} (AUC = {roc_auc:.2f})')

plt.plot([0, 1], [0, 1], 'k--', label='Random')
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('ROC Curves (Binary Classification: Setosa vs Versicolor)')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## Tasks

Create a Python file named `classification_algorithms.py` in this folder and implement the following:

1. Implement multiple classification algorithms (Logistic Regression, Decision Trees, Random Forest, SVM, KNN, Naive Bayes)
2. Evaluate models using various metrics (accuracy, precision, recall, F1-score)
3. Perform cross-validation and hyperparameter tuning
4. Visualize confusion matrices and ROC curves
5. Compare model performance and select the best algorithm

## Additional Resources

- Scikit-learn classification documentation
- Model evaluation metrics guide
- Choosing the right classification algorithm