# Data Visualization in Python

## Introduction

Data visualization is the graphical representation of data and information. Effective visualizations help communicate insights, patterns, and trends in data. Python offers several powerful libraries for creating visualizations, with matplotlib and seaborn being the most popular for static plots, and plotly for interactive visualizations.

## Key Concepts

### Visualization Types
- **Line plots**: Show trends over time or continuous data
- **Bar charts**: Compare categories or show frequencies
- **Scatter plots**: Show relationships between two variables
- **Histograms**: Show distribution of continuous data
- **Box plots**: Show statistical summaries and outliers
- **Heatmaps**: Show correlations or matrix data
- **Pie charts**: Show proportions of categorical data

### Libraries
- **Matplotlib**: Foundation for Python plotting
- **Seaborn**: Statistical data visualization (built on matplotlib)
- **Plotly**: Interactive visualizations
- **Pandas**: Built-in plotting methods

### Best Practices
- Choose appropriate chart types
- Use clear labels and titles
- Avoid clutter and unnecessary elements
- Use color effectively
- Consider your audience

## Examples

### Basic Matplotlib Plots
```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns

# Set style
plt.style.use('seaborn-v0_8')

# Sample data
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# Line plot
plt.figure(figsize=(10, 6))
plt.plot(x, y1, label='sin(x)', color='blue', linewidth=2)
plt.plot(x, y2, label='cos(x)', color='red', linewidth=2, linestyle='--')
plt.title('Trigonometric Functions')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Bar Charts
```python
# Sample data
categories = ['A', 'B', 'C', 'D', 'E']
values = [23, 45, 56, 78, 32]

plt.figure(figsize=(8, 6))
bars = plt.bar(categories, values, color=['skyblue', 'lightgreen', 'salmon', 'gold', 'purple'])
plt.title('Category Values')
plt.xlabel('Categories')
plt.ylabel('Values')

# Add value labels on bars
for bar, value in zip(bars, values):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1, 
             str(value), ha='center', va='bottom')

plt.show()
```

### Scatter Plots
```python
# Generate sample data
np.random.seed(42)
x = np.random.randn(100)
y = 2 * x + np.random.randn(100) * 0.5
colors = np.random.rand(100)
sizes = 100 * np.random.rand(100)

plt.figure(figsize=(8, 6))
scatter = plt.scatter(x, y, c=colors, s=sizes, alpha=0.6, cmap='viridis')
plt.colorbar(scatter)
plt.title('Scatter Plot with Color and Size')
plt.xlabel('X values')
plt.ylabel('Y values')
plt.grid(True, alpha=0.3)
plt.show()
```

### Histograms and Box Plots
```python
# Generate sample data
data1 = np.random.normal(0, 1, 1000)
data2 = np.random.normal(2, 1.5, 1000)

# Histogram
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.hist(data1, bins=30, alpha=0.7, label='Dataset 1', color='skyblue')
plt.hist(data2, bins=30, alpha=0.7, label='Dataset 2', color='salmon')
plt.title('Histogram Comparison')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.legend()

# Box plot
plt.subplot(1, 2, 2)
plt.boxplot([data1, data2], labels=['Dataset 1', 'Dataset 2'])
plt.title('Box Plot Comparison')
plt.ylabel('Value')

plt.tight_layout()
plt.show()
```

### Seaborn Visualizations
```python
# Load sample dataset
df = sns.load_dataset('iris')

# Pair plot
plt.figure(figsize=(10, 8))
sns.pairplot(df, hue='species', diag_kind='kde', markers=['o', 's', 'D'])
plt.suptitle('Iris Dataset Pair Plot', y=1.02)
plt.show()

# Box plots by category
plt.figure(figsize=(12, 8))
for i, column in enumerate(df.select_dtypes(include=[np.number]).columns):
    plt.subplot(2, 2, i+1)
    sns.boxplot(x='species', y=column, data=df)
    plt.title(f'{column.replace("_", " ").title()} by Species')

plt.tight_layout()
plt.show()
```

### Correlation Heatmap
```python
# Calculate correlation matrix
numeric_df = df.select_dtypes(include=[np.number])
correlation_matrix = numeric_df.corr()

plt.figure(figsize=(8, 6))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', center=0,
            square=True, linewidths=0.5, cbar_kws={"shrink": 0.8})
plt.title('Correlation Heatmap')
plt.show()
```

### Time Series Visualization
```python
# Generate time series data
dates = pd.date_range('2020-01-01', periods=365, freq='D')
values = np.cumsum(np.random.randn(365)) + 100

ts_df = pd.DataFrame({'Date': dates, 'Value': values})
ts_df.set_index('Date', inplace=True)

plt.figure(figsize=(12, 6))
plt.plot(ts_df.index, ts_df['Value'], linewidth=2, color='darkblue')
plt.title('Time Series Data')
plt.xlabel('Date')
plt.ylabel('Value')
plt.grid(True, alpha=0.3)

# Add rolling average
rolling_avg = ts_df['Value'].rolling(window=30).mean()
plt.plot(ts_df.index, rolling_avg, color='red', linewidth=2, label='30-day Rolling Average')
plt.legend()

plt.show()
```

### Pie Charts and Donut Charts
```python
# Sample data
labels = ['Category A', 'Category B', 'Category C', 'Category D']
sizes = [35, 25, 20, 20]
colors = ['skyblue', 'lightgreen', 'salmon', 'gold']
explode = (0.1, 0, 0, 0)  # explode first slice

plt.figure(figsize=(12, 5))

# Pie chart
plt.subplot(1, 2, 1)
plt.pie(sizes, explode=explode, labels=labels, colors=colors, autopct='%1.1f%%',
        shadow=True, startangle=90)
plt.title('Pie Chart')
plt.axis('equal')

# Donut chart
plt.subplot(1, 2, 2)
plt.pie(sizes, labels=labels, colors=colors, autopct='%1.1f%%', startangle=90)
# Draw a circle at the center to make it a donut
centre_circle = plt.Circle((0,0),0.70,fc='white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)
plt.title('Donut Chart')
plt.axis('equal')

plt.tight_layout()
plt.show()
```

### Subplots and Multiple Charts
```python
# Create multiple subplots
fig, axes = plt.subplots(2, 3, figsize=(15, 10))
fig.suptitle('Multiple Visualization Types', fontsize=16)

# Line plot
x = np.linspace(0, 10, 100)
axes[0, 0].plot(x, np.sin(x), color='blue')
axes[0, 0].set_title('Line Plot')
axes[0, 0].grid(True, alpha=0.3)

# Bar chart
categories = ['A', 'B', 'C']
values = [10, 15, 12]
axes[0, 1].bar(categories, values, color='lightgreen')
axes[0, 1].set_title('Bar Chart')

# Scatter plot
x_scatter = np.random.randn(50)
y_scatter = np.random.randn(50)
axes[0, 2].scatter(x_scatter, y_scatter, alpha=0.6, color='red')
axes[0, 2].set_title('Scatter Plot')
axes[0, 2].grid(True, alpha=0.3)

# Histogram
data_hist = np.random.normal(0, 1, 1000)
axes[1, 0].hist(data_hist, bins=30, alpha=0.7, color='purple')
axes[1, 0].set_title('Histogram')

# Box plot
data_box = [np.random.normal(0, 1, 100), np.random.normal(1, 1.5, 100)]
axes[1, 1].boxplot(data_box, labels=['Group 1', 'Group 2'])
axes[1, 1].set_title('Box Plot')

# Pie chart
sizes_pie = [30, 25, 20, 25]
axes[1, 2].pie(sizes_pie, labels=['A', 'B', 'C', 'D'], autopct='%1.1f%%', 
               colors=['skyblue', 'lightgreen', 'salmon', 'gold'])
axes[1, 2].set_title('Pie Chart')

plt.tight_layout()
plt.show()
```

### Saving Plots
```python
# Create a sample plot
plt.figure(figsize=(8, 6))
x = np.linspace(0, 10, 100)
plt.plot(x, np.sin(x), label='sin(x)', linewidth=2)
plt.plot(x, np.cos(x), label='cos(x)', linewidth=2, linestyle='--')
plt.title('Trigonometric Functions')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.grid(True, alpha=0.3)

# Save in different formats
plt.savefig('trigonometric_plot.png', dpi=300, bbox_inches='tight')
plt.savefig('trigonometric_plot.pdf', bbox_inches='tight')
plt.savefig('trigonometric_plot.svg', bbox_inches='tight')

plt.show()
```

## Tasks

Create a Python file named `data_visualization.py` in this folder and implement the following:

1. Create various types of plots (line, bar, scatter, histogram)
2. Use seaborn for statistical visualizations
3. Create subplots with multiple chart types
4. Implement interactive visualizations (if using plotly)
5. Save plots in different formats
6. Create a comprehensive visualization dashboard

## Additional Resources

- Matplotlib documentation
- Seaborn gallery
- Data visualization best practices
- Color theory for data visualization