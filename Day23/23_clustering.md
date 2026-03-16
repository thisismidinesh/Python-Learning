# Clustering Algorithms in Python

## Introduction

Clustering is an unsupervised machine learning technique that groups similar data points together. Unlike supervised learning, clustering doesn't require labeled data. Python's scikit-learn library provides several clustering algorithms, each with different approaches and use cases.

## Key Concepts

### Types of Clustering
- **Partitioning**: Divides data into non-overlapping subsets (K-means, K-medoids)
- **Hierarchical**: Creates a tree of clusters (Agglomerative, Divisive)
- **Density-based**: Groups based on density of points (DBSCAN)
- **Grid-based**: Uses a grid structure (STING, CLIQUE)
- **Model-based**: Assumes data generated from probabilistic model (Gaussian Mixture)

### Evaluation Metrics
- **Silhouette Score**: Measures how similar objects are to their cluster vs other clusters
- **Calinski-Harabasz Index**: Ratio of between-cluster dispersion to within-cluster dispersion
- **Davies-Bouldin Index**: Average similarity measure of each cluster with its most similar cluster

### Choosing K in K-means
- **Elbow Method**: Plot inertia vs k, look for "elbow"
- **Silhouette Analysis**: Find k with highest average silhouette score
- **Gap Statistic**: Compare within-cluster dispersion to reference distribution

## Examples

### K-Means Clustering
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score, calinski_harabasz_score, davies_bouldin_score
from sklearn.preprocessing import StandardScaler
import seaborn as sns

# Generate sample data
np.random.seed(42)
# Create three clusters
cluster1 = np.random.normal([2, 2], 0.5, (100, 2))
cluster2 = np.random.normal([8, 2], 0.5, (100, 2))
cluster3 = np.random.normal([5, 8], 0.5, (100, 2))

# Combine data
X = np.vstack([cluster1, cluster2, cluster3])
print("Dataset shape:", X.shape)

# Scale the data
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Determine optimal k using elbow method
inertias = []
silhouette_scores = []
k_range = range(2, 11)

for k in k_range:
    kmeans = KMeans(n_clusters=k, random_state=42, n_init=10)
    kmeans.fit(X_scaled)
    inertias.append(kmeans.inertia_)
    silhouette_scores.append(silhouette_score(X_scaled, kmeans.labels_))

# Plot elbow curve and silhouette scores
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

ax1.plot(k_range, inertias, 'bo-')
ax1.set_xlabel('Number of Clusters (k)')
ax1.set_ylabel('Inertia')
ax1.set_title('Elbow Method')
ax1.grid(True, alpha=0.3)

ax2.plot(k_range, silhouette_scores, 'ro-')
ax2.set_xlabel('Number of Clusters (k)')
ax2.set_ylabel('Silhouette Score')
ax2.set_title('Silhouette Analysis')
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Perform K-means with optimal k (k=3)
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
clusters = kmeans.fit_predict(X_scaled)

print("K-means Results:")
print(f"Cluster centers: {kmeans.cluster_centers_}")
print(f"Inertia: {kmeans.inertia_:.2f}")
print(f"Silhouette Score: {silhouette_score(X_scaled, clusters):.3f}")
print(f"Calinski-Harabasz Score: {calinski_harabasz_score(X_scaled, clusters):.2f}")
print(f"Davies-Bouldin Score: {davies_bouldin_score(X_scaled, clusters):.3f}")
print()

# Visualize clustering results
plt.figure(figsize=(10, 8))

# Original data
plt.subplot(1, 2, 1)
plt.scatter(X[:, 0], X[:, 1], alpha=0.6, c='gray')
plt.title('Original Data')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.grid(True, alpha=0.3)

# Clustered data
plt.subplot(1, 2, 2)
colors = ['red', 'blue', 'green']
for i in range(3):
    mask = clusters == i
    plt.scatter(X[mask, 0], X[mask, 1], alpha=0.6, color=colors[i], label=f'Cluster {i+1}')
    
# Plot cluster centers
centers = scaler.inverse_transform(kmeans.cluster_centers_)
plt.scatter(centers[:, 0], centers[:, 1], c='black', marker='x', s=200, linewidth=3, label='Centroids')

plt.title('K-means Clustering Results')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.legend()
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Hierarchical Clustering
```python
from scipy.cluster.hierarchy import dendrogram, linkage, fcluster
from sklearn.cluster import AgglomerativeClustering

# Perform hierarchical clustering
linkage_matrix = linkage(X_scaled, method='ward')

# Plot dendrogram
plt.figure(figsize=(12, 8))
dendrogram(linkage_matrix, truncate_mode='level', p=5)
plt.title('Hierarchical Clustering Dendrogram')
plt.xlabel('Sample Index')
plt.ylabel('Distance')
plt.show()

# Perform agglomerative clustering
agg_clustering = AgglomerativeClustering(n_clusters=3, linkage='ward')
agg_clusters = agg_clustering.fit_predict(X_scaled)

print("Hierarchical Clustering Results:")
print(f"Silhouette Score: {silhouette_score(X_scaled, agg_clusters):.3f}")
print(f"Calinski-Harabasz Score: {calinski_harabasz_score(X_scaled, agg_clusters):.3f}")
print()

# Compare K-means and Hierarchical clustering
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# K-means results
colors = ['red', 'blue', 'green']
for i in range(3):
    mask = clusters == i
    ax1.scatter(X[mask, 0], X[mask, 1], alpha=0.6, color=colors[i], label=f'Cluster {i+1}')
ax1.scatter(centers[:, 0], centers[:, 1], c='black', marker='x', s=200, linewidth=3)
ax1.set_title('K-means Clustering')
ax1.set_xlabel('Feature 1')
ax1.set_ylabel('Feature 2')
ax1.legend()
ax1.grid(True, alpha=0.3)

# Hierarchical results
for i in range(3):
    mask = agg_clusters == i
    ax2.scatter(X[mask, 0], X[mask, 1], alpha=0.6, color=colors[i], label=f'Cluster {i+1}')
ax2.set_title('Hierarchical Clustering')
ax2.set_xlabel('Feature 1')
ax2.set_ylabel('Feature 2')
ax2.legend()
ax2.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### DBSCAN (Density-Based Spatial Clustering)
```python
from sklearn.cluster import DBSCAN
from sklearn.neighbors import NearestNeighbors

# Find optimal eps using k-distance plot
neighbors = NearestNeighbors(n_neighbors=5)
neighbors_fit = neighbors.fit(X_scaled)
distances, indices = neighbors_fit.kneighbors(X_scaled)
distances = np.sort(distances[:, 4])  # 5th nearest neighbor

plt.figure(figsize=(8, 6))
plt.plot(distances)
plt.title('K-distance Plot (k=5)')
plt.xlabel('Points sorted by distance')
plt.ylabel('5th nearest neighbor distance')
plt.grid(True, alpha=0.3)
plt.show()

# Perform DBSCAN
dbscan = DBSCAN(eps=0.3, min_samples=10)
dbscan_clusters = dbscan.fit_predict(X_scaled)

# Get number of clusters (excluding noise labeled as -1)
n_clusters = len(set(dbscan_clusters)) - (1 if -1 in dbscan_clusters else 0)
n_noise = list(dbscan_clusters).count(-1)

print("DBSCAN Results:")
print(f"Number of clusters: {n_clusters}")
print(f"Number of noise points: {n_noise}")
if n_clusters > 1:
    print(f"Silhouette Score: {silhouette_score(X_scaled, dbscan_clusters):.3f}")
print()

# Visualize DBSCAN results
plt.figure(figsize=(8, 6))

# Plot clusters
unique_labels = set(dbscan_clusters)
colors = plt.cm.Spectral(np.linspace(0, 1, len(unique_labels)))

for k, col in zip(unique_labels, colors):
    if k == -1:
        # Black for noise
        col = 'black'
    
    class_member_mask = (dbscan_clusters == k)
    xy = X[class_member_mask]
    plt.scatter(xy[:, 0], xy[:, 1], c=[col], alpha=0.6, s=50, label=f'Cluster {k}' if k != -1 else 'Noise')

plt.title('DBSCAN Clustering Results')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Gaussian Mixture Models
```python
from sklearn.mixture import GaussianMixture
from sklearn.metrics import silhouette_score

# Fit Gaussian Mixture Models with different numbers of components
n_components_range = range(1, 6)
bic_scores = []
aic_scores = []

for n_components in n_components_range:
    gmm = GaussianMixture(n_components=n_components, random_state=42)
    gmm.fit(X_scaled)
    bic_scores.append(gmm.bic(X_scaled))
    aic_scores.append(gmm.aic(X_scaled))

# Plot BIC and AIC
plt.figure(figsize=(10, 6))
plt.plot(n_components_range, bic_scores, 'bo-', label='BIC')
plt.plot(n_components_range, aic_scores, 'ro-', label='AIC')
plt.xlabel('Number of Components')
plt.ylabel('Score')
plt.title('BIC and AIC for Gaussian Mixture Models')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()

# Fit optimal GMM (3 components)
gmm = GaussianMixture(n_components=3, random_state=42)
gmm_clusters = gmm.fit_predict(X_scaled)
gmm_probs = gmm.predict_proba(X_scaled)

print("Gaussian Mixture Model Results:")
print(f"BIC: {gmm.bic(X_scaled):.2f}")
print(f"AIC: {gmm.aic(X_scaled):.2f}")
print(f"Silhouette Score: {silhouette_score(X_scaled, gmm_clusters):.3f}")
print()

# Visualize GMM results
plt.figure(figsize=(12, 5))

# Cluster assignments
plt.subplot(1, 2, 1)
colors = ['red', 'blue', 'green']
for i in range(3):
    mask = gmm_clusters == i
    plt.scatter(X[mask, 0], X[mask, 1], alpha=0.6, color=colors[i], label=f'Cluster {i+1}')

# Plot cluster centers
centers = scaler.inverse_transform(gmm.means_)
plt.scatter(centers[:, 0], centers[:, 1], c='black', marker='x', s=200, linewidth=3, label='Centers')

plt.title('GMM Cluster Assignments')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.legend()
plt.grid(True, alpha=0.3)

# Probability contours
plt.subplot(1, 2, 2)
x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, 0.1),
                     np.arange(y_min, y_max, 0.1))

# Predict probabilities for grid points
grid_points = np.c_[xx.ravel(), yy.ravel()]
grid_points_scaled = scaler.transform(grid_points)
Z = gmm.predict_proba(grid_points_scaled)
Z = Z.reshape(xx.shape + (3,))

# Plot probability contours
plt.contourf(xx, yy, Z[:, :, 0], alpha=0.3, cmap='Reds')
plt.contourf(xx, yy, Z[:, :, 1], alpha=0.3, cmap='Blues')
plt.contourf(xx, yy, Z[:, :, 2], alpha=0.3, cmap='Greens')

plt.scatter(X[:, 0], X[:, 1], c=gmm_clusters, alpha=0.6, cmap='viridis', edgecolors='black')
plt.title('GMM Probability Contours')
plt.xlabel('Feature 1')
plt.ylabel('Feature 2')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Clustering Comparison and Evaluation
```python
# Compare different clustering algorithms
algorithms = {
    'K-means': KMeans(n_clusters=3, random_state=42, n_init=10),
    'Hierarchical': AgglomerativeClustering(n_clusters=3),
    'DBSCAN': DBSCAN(eps=0.3, min_samples=10),
    'GMM': GaussianMixture(n_components=3, random_state=42)
}

results = {}

for name, algorithm in algorithms.items():
    if name == 'GMM':
        clusters = algorithm.fit_predict(X_scaled)
    else:
        clusters = algorithm.fit_predict(X_scaled)
    
    # Calculate metrics (skip for DBSCAN if only noise)
    if len(set(clusters)) > 1 and -1 not in clusters:
        silhouette = silhouette_score(X_scaled, clusters)
        ch_score = calinski_harabasz_score(X_scaled, clusters)
        db_score = davies_bouldin_score(X_scaled, clusters)
    else:
        silhouette = ch_score = db_score = None
    
    results[name] = {
        'n_clusters': len(set(clusters)) - (1 if -1 in clusters else 0),
        'silhouette': silhouette,
        'calinski_harabasz': ch_score,
        'davies_bouldin': db_score
    }

# Display comparison
print("Clustering Algorithm Comparison:")
print("=" * 60)
print(f"{'Algorithm':<15} {'Clusters':<10} {'Silhouette':<12} {'CH Score':<12} {'DB Score':<12}")
print("-" * 60)

for name, metrics in results.items():
    sil = f"{metrics['silhouette']:.3f}" if metrics['silhouette'] else "N/A"
    ch = f"{metrics['calinski_harabasz']:.1f}" if metrics['calinski_harabasz'] else "N/A"
    db = f"{metrics['davies_bouldin']:.3f}" if metrics['davies_bouldin'] else "N/A"
    print(f"{name:<15} {metrics['n_clusters']:<10} {sil:<12} {ch:<12} {db:<12}")

print()

# Visualize all clustering results
fig, axes = plt.subplots(2, 2, figsize=(15, 12))
axes = axes.ravel()

for i, (name, algorithm) in enumerate(algorithms.items()):
    if name == 'GMM':
        clusters = algorithm.fit_predict(X_scaled)
    else:
        clusters = algorithm.fit_predict(X_scaled)
    
    ax = axes[i]
    
    # Handle different cluster labels
    if -1 in clusters:  # DBSCAN noise
        unique_labels = set(clusters)
        colors = plt.cm.Spectral(np.linspace(0, 1, len(unique_labels)))
        for k, col in zip(unique_labels, colors):
            if k == -1:
                col = 'black'
            class_member_mask = (clusters == k)
            xy = X[class_member_mask]
            ax.scatter(xy[:, 0], xy[:, 1], c=[col], alpha=0.6, s=50)
    else:
        scatter = ax.scatter(X[:, 0], X[:, 1], c=clusters, cmap='viridis', alpha=0.6)
        plt.colorbar(scatter, ax=ax)
    
    ax.set_title(f'{name} Clustering')
    ax.set_xlabel('Feature 1')
    ax.set_ylabel('Feature 2')
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

## Tasks

Create a Python file named `clustering_algorithms.py` in this folder and implement the following:

1. Implement K-means clustering with elbow method for optimal k selection
2. Perform hierarchical clustering and visualize dendrograms
3. Apply DBSCAN clustering with parameter tuning
4. Use Gaussian Mixture Models for probabilistic clustering
5. Compare different clustering algorithms using evaluation metrics

## Additional Resources

- Scikit-learn clustering documentation
- Clustering algorithm comparisons
- Choosing the right clustering algorithm