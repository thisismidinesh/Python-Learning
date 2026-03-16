# Statistics Fundamentals in Python

## Introduction

Statistics is the science of collecting, analyzing, interpreting, and presenting data. Python provides powerful libraries like NumPy, SciPy, and StatsModels for statistical analysis. Understanding basic statistical concepts is essential for data analysis and machine learning.

## Key Concepts

### Descriptive Statistics
- **Measures of Central Tendency**: Mean, median, mode
- **Measures of Dispersion**: Variance, standard deviation, range, IQR
- **Measures of Shape**: Skewness, kurtosis
- **Percentiles and Quartiles**: Data distribution points

### Probability Distributions
- **Normal Distribution**: Bell-shaped curve
- **Binomial Distribution**: Discrete probability
- **Poisson Distribution**: Count data
- **Uniform Distribution**: Equal probability

### Statistical Tests
- **t-tests**: Compare means between groups
- **Chi-square tests**: Test relationships between categorical variables
- **ANOVA**: Compare means across multiple groups
- **Correlation tests**: Measure relationships between variables

### Hypothesis Testing
- **Null Hypothesis (H₀)**: No effect or relationship
- **Alternative Hypothesis (H₁)**: Effect or relationship exists
- **p-value**: Probability of observing data if null hypothesis is true
- **Significance level (α)**: Threshold for rejecting null hypothesis

## Examples

### Descriptive Statistics
```python
import numpy as np
import pandas as pd
import scipy.stats as stats
import matplotlib.pyplot as plt
import seaborn as sns

# Generate sample data
np.random.seed(42)
data = np.random.normal(50, 15, 1000)  # Normal distribution: mean=50, std=15

print("Descriptive Statistics:")
print(f"Count: {len(data)}")
print(f"Mean: {np.mean(data):.2f}")
print(f"Median: {np.median(data):.2f}")
print(f"Mode: {stats.mode(data, keepdims=True)[0][0]:.2f}")
print(f"Standard Deviation: {np.std(data, ddof=1):.2f}")  # ddof=1 for sample std
print(f"Variance: {np.var(data, ddof=1):.2f}")
print(f"Minimum: {np.min(data):.2f}")
print(f"Maximum: {np.max(data):.2f}")
print(f"Range: {np.max(data) - np.min(data):.2f}")
print(f"25th Percentile: {np.percentile(data, 25):.2f}")
print(f"75th Percentile: {np.percentile(data, 75):.2f}")
print(f"IQR: {np.percentile(data, 75) - np.percentile(data, 25):.2f}")
print(f"Skewness: {stats.skew(data):.2f}")
print(f"Kurtosis: {stats.kurtosis(data):.2f}")
print()

# Using pandas for descriptive statistics
df = pd.DataFrame({'values': data})
print("Pandas Descriptive Statistics:")
print(df.describe())
```

### Data Distribution Visualization
```python
# Plot histogram and normal curve
plt.figure(figsize=(12, 5))

# Histogram
plt.subplot(1, 2, 1)
plt.hist(data, bins=30, alpha=0.7, color='skyblue', density=True)
plt.title('Data Distribution')
plt.xlabel('Value')
plt.ylabel('Density')

# Add normal curve
x_range = np.linspace(np.min(data), np.max(data), 100)
plt.plot(x_range, stats.norm.pdf(x_range, np.mean(data), np.std(data)), 
         'r-', linewidth=2, label='Normal Curve')
plt.legend()

# Q-Q plot
plt.subplot(1, 2, 2)
stats.probplot(data, dist="norm", plot=plt)
plt.title('Q-Q Plot')

plt.tight_layout()
plt.show()
```

### Probability Distributions
```python
# Normal Distribution
plt.figure(figsize=(15, 10))

# Normal distribution
plt.subplot(2, 3, 1)
x = np.linspace(-4, 4, 100)
plt.plot(x, stats.norm.pdf(x), 'b-', linewidth=2)
plt.title('Normal Distribution')
plt.xlabel('x')
plt.ylabel('PDF')

# Binomial distribution
plt.subplot(2, 3, 2)
n, p = 10, 0.5
x_binom = np.arange(0, n+1)
plt.bar(x_binom, stats.binom.pmf(x_binom, n, p), alpha=0.7, color='lightgreen')
plt.title('Binomial Distribution (n=10, p=0.5)')
plt.xlabel('Successes')
plt.ylabel('PMF')

# Poisson distribution
plt.subplot(2, 3, 3)
mu = 3
x_poisson = np.arange(0, 10)
plt.bar(x_poisson, stats.poisson.pmf(x_poisson, mu), alpha=0.7, color='salmon')
plt.title('Poisson Distribution (μ=3)')
plt.xlabel('Events')
plt.ylabel('PMF')

# Uniform distribution
plt.subplot(2, 3, 4)
x_uniform = np.linspace(0, 1, 100)
plt.plot(x_uniform, stats.uniform.pdf(x_uniform), 'purple', linewidth=2)
plt.title('Uniform Distribution')
plt.xlabel('x')
plt.ylabel('PDF')

# Exponential distribution
plt.subplot(2, 3, 5)
x_exp = np.linspace(0, 5, 100)
plt.plot(x_exp, stats.expon.pdf(x_exp), 'orange', linewidth=2)
plt.title('Exponential Distribution')
plt.xlabel('x')
plt.ylabel('PDF')

# Chi-square distribution
plt.subplot(2, 3, 6)
x_chi2 = np.linspace(0, 10, 100)
plt.plot(x_chi2, stats.chi2.pdf(x_chi2, df=3), 'red', linewidth=2)
plt.title('Chi-square Distribution (df=3)')
plt.xlabel('x')
plt.ylabel('PDF')

plt.tight_layout()
plt.show()
```

### Hypothesis Testing - t-tests
```python
# Generate two sample groups
np.random.seed(42)
group1 = np.random.normal(50, 10, 100)
group2 = np.random.normal(55, 10, 100)

print("Two-Sample t-test:")
print(f"Group 1 mean: {np.mean(group1):.2f}")
print(f"Group 2 mean: {np.mean(group2):.2f}")

# Perform t-test
t_stat, p_value = stats.ttest_ind(group1, group2)
print(f"t-statistic: {t_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"Significant difference: {'Yes' if p_value < 0.05 else 'No'}")
print()

# One-sample t-test
population_mean = 50
sample = np.random.normal(52, 8, 50)

print("One-Sample t-test:")
print(f"Sample mean: {np.mean(sample):.2f}")
print(f"Population mean: {population_mean}")

t_stat_one, p_value_one = stats.ttest_1samp(sample, population_mean)
print(f"t-statistic: {t_stat_one:.4f}")
print(f"p-value: {p_value_one:.4f}")
print(f"Sample differs from population: {'Yes' if p_value_one < 0.05 else 'No'}")
```

### Chi-Square Test
```python
# Create contingency table
observed = np.array([[30, 10], [20, 40]])
print("Contingency Table:")
print(observed)
print()

# Perform chi-square test
chi2_stat, p_value, dof, expected = stats.chi2_contingency(observed)
print("Chi-Square Test:")
print(f"Chi-square statistic: {chi2_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"Degrees of freedom: {dof}")
print("Expected frequencies:")
print(expected)
print(f"Significant association: {'Yes' if p_value < 0.05 else 'No'}")
```

### ANOVA (Analysis of Variance)
```python
# Generate three groups
np.random.seed(42)
group_a = np.random.normal(50, 5, 30)
group_b = np.random.normal(55, 5, 30)
group_c = np.random.normal(60, 5, 30)

print("One-way ANOVA:")
print(f"Group A mean: {np.mean(group_a):.2f}")
print(f"Group B mean: {np.mean(group_b):.2f}")
print(f"Group C mean: {np.mean(group_c):.2f}")

# Perform ANOVA
f_stat, p_value = stats.f_oneway(group_a, group_b, group_c)
print(f"F-statistic: {f_stat:.4f}")
print(f"p-value: {p_value:.4f}")
print(f"Significant differences between groups: {'Yes' if p_value < 0.05 else 'No'}")
```

### Correlation Analysis
```python
# Generate correlated data
np.random.seed(42)
x = np.random.normal(0, 1, 100)
y = 2 * x + np.random.normal(0, 0.5, 100)  # y is correlated with x

print("Correlation Analysis:")
print(f"Pearson correlation: {stats.pearsonr(x, y)[0]:.4f}")
print(f"Spearman correlation: {stats.spearmanr(x, y)[0]:.4f}")

# Scatter plot with regression line
plt.figure(figsize=(8, 6))
plt.scatter(x, y, alpha=0.6, color='blue')
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Correlation Scatter Plot')

# Add regression line
slope, intercept = np.polyfit(x, y, 1)
plt.plot(x, slope * x + intercept, color='red', linewidth=2, label='Regression Line')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### Confidence Intervals
```python
# Calculate confidence interval for mean
data_ci = np.random.normal(50, 10, 100)
mean = np.mean(data_ci)
std_err = stats.sem(data_ci)  # Standard error of the mean

# 95% confidence interval
confidence_level = 0.95
ci_lower, ci_upper = stats.t.interval(confidence_level, len(data_ci)-1, 
                                     loc=mean, scale=std_err)

print("Confidence Interval for Mean:")
print(f"Sample mean: {mean:.2f}")
print(f"95% CI: ({ci_lower:.2f}, {ci_upper:.2f})")
print(f"Margin of error: {std_err * stats.t.ppf((1 + confidence_level) / 2, len(data_ci)-1):.2f}")
```

### Statistical Power and Sample Size
```python
# Calculate statistical power
from statsmodels.stats.power import TTestIndPower

# Parameters
effect_size = 0.5  # Cohen's d
alpha = 0.05       # Significance level
power = 0.8        # Desired power

# Calculate sample size
analysis = TTestIndPower()
sample_size = analysis.solve_power(effect_size=effect_size, alpha=alpha, power=power)
print(f"Required sample size per group: {int(np.ceil(sample_size))}")

# Calculate power for given sample size
n = 50
calculated_power = analysis.solve_power(effect_size=effect_size, nobs1=n, alpha=alpha)
print(f"Power with n={n} per group: {calculated_power:.3f}")
```

## Tasks

Create a Python file named `statistics_fundamentals.py` in this folder and implement the following:

1. Calculate comprehensive descriptive statistics for a dataset
2. Perform hypothesis testing (t-tests, chi-square, ANOVA)
3. Analyze different probability distributions
4. Compute correlation coefficients and confidence intervals
5. Create statistical visualizations and interpret results

## Additional Resources

- Statistics with Python documentation
- Statistical testing guidelines
- Probability distributions reference