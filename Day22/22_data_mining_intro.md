# Introduction to Data Mining

## Introduction

Data mining is the process of discovering patterns, correlations, and insights from large datasets. It involves applying algorithms and statistical methods to extract valuable information from data. Python provides excellent libraries for data mining tasks including scikit-learn, pandas, and NumPy.

## Key Concepts

### Data Mining Process
1. **Data Collection**: Gathering relevant data
2. **Data Cleaning**: Handling missing values, outliers, inconsistencies
3. **Data Integration**: Combining data from multiple sources
4. **Data Transformation**: Normalizing, scaling, encoding
5. **Data Mining**: Applying algorithms to discover patterns
6. **Pattern Evaluation**: Assessing discovered patterns
7. **Knowledge Presentation**: Visualizing and presenting results

### Types of Data Mining
- **Descriptive Mining**: What happened?
- **Predictive Mining**: What will happen?
- **Prescriptive Mining**: What should we do?

### Common Techniques
- **Classification**: Categorizing data into classes
- **Regression**: Predicting continuous values
- **Clustering**: Grouping similar data points
- **Association Rules**: Finding relationships between items
- **Anomaly Detection**: Identifying unusual patterns

## Examples

### Data Preparation for Mining
```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.model_selection import train_test_split
import matplotlib.pyplot as plt
import seaborn as sns

# Load sample dataset
df = sns.load_dataset('iris')
print("Original dataset:")
print(df.head())
print()

# Check for missing values
print("Missing values:")
print(df.isnull().sum())
print()

# Encode categorical variables
le = LabelEncoder()
df['species_encoded'] = le.fit_transform(df['species'])
print("Encoded species:")
print(df[['species', 'species_encoded']].drop_duplicates())
print()

# Split features and target
X = df.drop(['species', 'species_encoded'], axis=1)
y = df['species_encoded']

print("Features shape:", X.shape)
print("Target shape:", y.shape)
print()

# Split into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
print("Training set shape:", X_train.shape)
print("Testing set shape:", X_test.shape)
```

### Data Scaling and Normalization
```python
# Standard scaling (z-score normalization)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

print("Original data statistics:")
print(f"Mean: {X_train.mean().round(3)}")
print(f"Std: {X_train.std().round(3)}")
print()

print("Scaled data statistics:")
print(f"Mean: {X_train_scaled.mean(axis=0).round(3)}")
print(f"Std: {X_train_scaled.std(axis=0).round(3)}")
print()

# Min-Max scaling
from sklearn.preprocessing import MinMaxScaler
minmax_scaler = MinMaxScaler()
X_train_minmax = minmax_scaler.fit_transform(X_train)
X_test_minmax = minmax_scaler.transform(X_test)

print("Min-Max scaled data range:")
print(f"Min: {X_train_minmax.min(axis=0).round(3)}")
print(f"Max: {X_train_minmax.max(axis=0).round(3)}")
```

### Classification Example
```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# Train KNN classifier
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train_scaled, y_train)

# Make predictions
y_pred = knn.predict(X_test_scaled)

print("K-Nearest Neighbors Classification Results:")
print(f"Accuracy: {accuracy_score(y_test, y_pred):.3f}")
print()

print("Classification Report:")
print(classification_report(y_test, y_pred, target_names=le.classes_))
print()

print("Confusion Matrix:")
cm = confusion_matrix(y_test, y_pred)
print(cm)
```

### Regression Example
```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# Create a regression problem using sepal length as target
X_reg = df[['sepal_width', 'petal_length', 'petal_width']]
y_reg = df['sepal_length']

X_train_reg, X_test_reg, y_train_reg, y_test_reg = train_test_split(X_reg, y_reg, test_size=0.3, random_state=42)

# Scale the features
scaler_reg = StandardScaler()
X_train_reg_scaled = scaler_reg.fit_transform(X_train_reg)
X_test_reg_scaled = scaler_reg.transform(X_test_reg)

# Train linear regression
lr = LinearRegression()
lr.fit(X_train_reg_scaled, y_train_reg)

# Make predictions
y_pred_reg = lr.predict(X_test_reg_scaled)

print("Linear Regression Results:")
print(f"R² Score: {r2_score(y_test_reg, y_pred_reg):.3f}")
print(f"Mean Squared Error: {mean_squared_error(y_test_reg, y_pred_reg):.3f}")
print(f"Root Mean Squared Error: {np.sqrt(mean_squared_error(y_test_reg, y_pred_reg)):.3f}")
print()

# Display coefficients
feature_names = X_reg.columns
for name, coef in zip(feature_names, lr.coef_):
    print(f"{name}: {coef:.3f}")
print(f"Intercept: {lr.intercept_:.3f}")
```

### Clustering Example
```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# Prepare data for clustering (unsupervised learning)
X_cluster = df.drop(['species', 'species_encoded'], axis=1)
X_cluster_scaled = StandardScaler().fit_transform(X_cluster)

# Determine optimal number of clusters using elbow method
inertias = []
silhouette_scores = []
k_range = range(2, 8)

for k in k_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    kmeans.fit(X_cluster_scaled)
    inertias.append(kmeans.inertia_)
    silhouette_scores.append(silhouette_score(X_cluster_scaled, kmeans.labels_))

# Plot elbow curve
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.plot(k_range, inertias, 'bo-')
plt.xlabel('Number of Clusters (k)')
plt.ylabel('Inertia')
plt.title('Elbow Method')
plt.grid(True, alpha=0.3)

plt.subplot(1, 2, 2)
plt.plot(k_range, silhouette_scores, 'ro-')
plt.xlabel('Number of Clusters (k)')
plt.ylabel('Silhouette Score')
plt.title('Silhouette Analysis')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Perform clustering with optimal k (we know it's 3 for iris dataset)
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
clusters = kmeans.fit_predict(X_cluster_scaled)

df['cluster'] = clusters
print("Cluster distribution:")
print(df['cluster'].value_counts())
print()

# Compare clusters with actual species
print("Cluster vs Species comparison:")
comparison = pd.crosstab(df['cluster'], df['species'])
print(comparison)
```

### Association Rules Example
```python
from mlxtend.frequent_patterns import apriori, association_rules
import pandas as pd

# Create sample transaction data
transactions = [
    ['bread', 'milk', 'eggs'],
    ['bread', 'butter', 'cheese'],
    ['milk', 'butter', 'bread'],
    ['bread', 'milk', 'butter', 'cheese'],
    ['eggs', 'butter', 'cheese'],
    ['bread', 'eggs'],
    ['milk', 'bread', 'butter'],
    ['bread', 'milk', 'eggs', 'butter'],
    ['cheese', 'bread'],
    ['milk', 'eggs']
]

# Get unique items
all_items = set()
for transaction in transactions:
    all_items.update(transaction)
all_items = sorted(list(all_items))

# Create one-hot encoded DataFrame
transaction_df = pd.DataFrame(0, index=range(len(transactions)), columns=all_items)

for i, transaction in enumerate(transactions):
    for item in transaction:
        transaction_df.loc[i, item] = 1

print("Transaction data:")
print(transaction_df.head())
print()

# Find frequent itemsets
frequent_itemsets = apriori(transaction_df, min_support=0.3, use_colnames=True)
print("Frequent itemsets:")
print(frequent_itemsets)
print()

# Generate association rules
rules = association_rules(frequent_itemsets, metric="confidence", min_threshold=0.6)
print("Association rules:")
print(rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
```

### Anomaly Detection Example
```python
from sklearn.ensemble import IsolationForest
from sklearn.svm import OneClassSVM

# Generate normal data with some anomalies
np.random.seed(42)
normal_data = np.random.normal(0, 1, (1000, 2))
anomalies = np.random.uniform(-5, 5, (50, 2))

# Combine data
all_data = np.vstack([normal_data, anomalies])
labels = np.hstack([np.ones(len(normal_data)), -np.ones(len(anomalies))])

# Isolation Forest
iso_forest = IsolationForest(contamination=0.05, random_state=42)
iso_predictions = iso_forest.fit_predict(all_data)

print("Anomaly Detection Results:")
print(f"Isolation Forest - Detected anomalies: {np.sum(iso_predictions == -1)}")
print(f"Actual anomalies: {np.sum(labels == -1)}")
print()

# One-Class SVM
oc_svm = OneClassSVM(nu=0.05)
svm_predictions = oc_svm.fit_predict(all_data)

print(f"One-Class SVM - Detected anomalies: {np.sum(svm_predictions == -1)}")
print()

# Visualize results
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.scatter(all_data[:, 0], all_data[:, 1], c=iso_predictions, cmap='coolwarm', alpha=0.6)
plt.title('Isolation Forest Results')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.colorbar(label='Prediction')

plt.subplot(1, 2, 2)
plt.scatter(all_data[:, 0], all_data[:, 1], c=svm_predictions, cmap='coolwarm', alpha=0.6)
plt.title('One-Class SVM Results')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.colorbar(label='Prediction')

plt.tight_layout()
plt.show()
```

### Model Evaluation and Comparison
```python
from sklearn.model_selection import cross_val_score
from sklearn.tree import DecisionTreeClassifier
from sklearn.naive_bayes import GaussianNB
from sklearn.svm import SVC

# Prepare data
X_eval = df.drop(['species', 'species_encoded', 'cluster'], axis=1)
y_eval = df['species_encoded']
X_eval_scaled = StandardScaler().fit_transform(X_eval)

# Define models
models = {
    'KNN': KNeighborsClassifier(n_neighbors=3),
    'Decision Tree': DecisionTreeClassifier(random_state=42),
    'Naive Bayes': GaussianNB(),
    'SVM': SVC(random_state=42)
}

# Evaluate models using cross-validation
print("Model Comparison (Cross-validation scores):")
print("-" * 50)

for name, model in models.items():
    scores = cross_val_score(model, X_eval_scaled, y_eval, cv=5, scoring='accuracy')
    print(f"{name:15}: {scores.mean():.3f} (+/- {scores.std() * 2:.3f})")

print()

# Train and evaluate on test set
X_train_eval, X_test_eval, y_train_eval, y_test_eval = train_test_split(
    X_eval_scaled, y_eval, test_size=0.3, random_state=42)

print("Model Comparison (Test set accuracy):")
print("-" * 40)

for name, model in models.items():
    model.fit(X_train_eval, y_train_eval)
    accuracy = model.score(X_test_eval, y_test_eval)
    print(f"{name:15}: {accuracy:.3f}")
```

## Tasks

Create a Python file named `data_mining_intro.py` in this folder and implement the following:

1. Perform data preprocessing and feature engineering
2. Implement classification and regression algorithms
3. Apply clustering techniques and evaluate results
4. Generate association rules from transaction data
5. Implement anomaly detection methods
6. Compare different machine learning models

## Additional Resources

- Scikit-learn documentation
- Data mining algorithms overview
- Machine learning model evaluation metrics