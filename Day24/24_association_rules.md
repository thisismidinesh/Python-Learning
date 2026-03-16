# Association Rule Mining in Python

## Introduction

Association rule mining is a technique used to discover interesting relationships between variables in large datasets. It's commonly used in market basket analysis to find patterns like "customers who buy bread also tend to buy butter". The Apriori algorithm and FP-Growth are popular methods for mining association rules.

## Key Concepts

### Basic Concepts
- **Itemset**: A collection of items
- **Support**: Frequency of occurrence of an itemset
- **Confidence**: How often the rule is true
- **Lift**: Ratio of observed support to expected support if independent
- **Conviction**: Measure of implication strength

### Metrics Formulas
- Support(X) = count(X) / total_transactions
- Confidence(X→Y) = support(X∪Y) / support(X)
- Lift(X→Y) = confidence(X→Y) / support(Y)
- Conviction(X→Y) = (1 - support(Y)) / (1 - confidence(X→Y))

### Algorithms
- **Apriori**: Uses candidate generation
- **FP-Growth**: Uses frequent pattern tree
- **ECLAT**: Uses vertical data format

### Applications
- Market basket analysis
- Recommendation systems
- Medical diagnosis
- Web usage mining

## Examples

### Data Preparation for Association Rules
```python
import pandas as pd
import numpy as np
from mlxtend.frequent_patterns import apriori, association_rules, fpgrowth
from mlxtend.preprocessing import TransactionEncoder
import matplotlib.pyplot as plt
import seaborn as sns

# Sample transaction data
transactions = [
    ['bread', 'milk', 'eggs', 'butter'],
    ['bread', 'butter', 'cheese'],
    ['milk', 'butter', 'bread', 'eggs'],
    ['bread', 'milk', 'butter', 'cheese'],
    ['eggs', 'butter', 'cheese', 'bread'],
    ['bread', 'eggs', 'milk'],
    ['milk', 'bread', 'butter', 'cheese'],
    ['bread', 'milk', 'eggs', 'butter', 'cheese'],
    ['cheese', 'bread', 'milk'],
    ['milk', 'eggs', 'butter'],
    ['bread', 'cheese'],
    ['butter', 'eggs'],
    ['milk', 'bread'],
    ['cheese', 'butter', 'milk'],
    ['bread', 'eggs', 'cheese']
]

print("Sample transactions:")
for i, transaction in enumerate(transactions[:5], 1):
    print(f"{i}. {transaction}")
print("...")
print()

# Transform to one-hot encoded format
te = TransactionEncoder()
te_ary = te.fit_transform(transactions)
df = pd.DataFrame(te_ary, columns=te.columns_)

print("One-hot encoded transaction data:")
print(df.head())
print()
print("Shape:", df.shape)
print("Items:", list(df.columns))
```

### Apriori Algorithm
```python
# Find frequent itemsets using Apriori
frequent_itemsets_apriori = apriori(df, min_support=0.3, use_colnames=True)

print("Frequent Itemsets (Apriori, min_support=0.3):")
print(frequent_itemsets_apriori.sort_values('support', ascending=False))
print()

# Generate association rules
rules_apriori = association_rules(frequent_itemsets_apriori, metric="confidence", min_threshold=0.6)

print("Association Rules (Apriori, min_confidence=0.6):")
print(rules_apriori[['antecedents', 'consequents', 'support', 'confidence', 'lift', 'conviction']])
print()

# Sort rules by lift
print("Top rules by lift:")
print(rules_apriori.nlargest(5, 'lift')[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
```

### FP-Growth Algorithm
```python
# Find frequent itemsets using FP-Growth
frequent_itemsets_fp = fpgrowth(df, min_support=0.3, use_colnames=True)

print("Frequent Itemsets (FP-Growth, min_support=0.3):")
print(frequent_itemsets_fp.sort_values('support', ascending=False))
print()

# Generate association rules
rules_fp = association_rules(frequent_itemsets_fp, metric="confidence", min_threshold=0.6)

print("Association Rules (FP-Growth, min_confidence=0.6):")
print(rules_fp[['antecedents', 'consequents', 'support', 'confidence', 'lift', 'conviction']])
print()

# Verify both algorithms give same results
print("Results comparison:")
print(f"Apriori itemsets: {len(frequent_itemsets_apriori)}")
print(f"FP-Growth itemsets: {len(frequent_itemsets_fp)}")
print(f"Apriori rules: {len(rules_apriori)}")
print(f"FP-Growth rules: {len(rules_fp)}")
```

### Rule Visualization and Analysis
```python
# Visualize support vs confidence
plt.figure(figsize=(12, 8))

# Scatter plot of support vs confidence
plt.subplot(2, 2, 1)
plt.scatter(rules_apriori['support'], rules_apriori['confidence'], alpha=0.6, s=rules_apriori['lift']*50)
plt.xlabel('Support')
plt.ylabel('Confidence')
plt.title('Support vs Confidence')
plt.grid(True, alpha=0.3)

# Scatter plot of support vs lift
plt.subplot(2, 2, 2)
plt.scatter(rules_apriori['support'], rules_apriori['lift'], alpha=0.6, c=rules_apriori['confidence'], cmap='viridis')
plt.colorbar(label='Confidence')
plt.xlabel('Support')
plt.ylabel('Lift')
plt.title('Support vs Lift')
plt.grid(True, alpha=0.3)

# Histogram of lift values
plt.subplot(2, 2, 3)
plt.hist(rules_apriori['lift'], bins=20, alpha=0.7, color='skyblue')
plt.xlabel('Lift')
plt.ylabel('Frequency')
plt.title('Distribution of Lift Values')
plt.grid(True, alpha=0.3)

# Confidence vs lift
plt.subplot(2, 2, 4)
plt.scatter(rules_apriori['confidence'], rules_apriori['lift'], alpha=0.6, s=rules_apriori['support']*1000)
plt.xlabel('Confidence')
plt.ylabel('Lift')
plt.title('Confidence vs Lift')
plt.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Rule Filtering and Interpretation
```python
# Filter rules by different criteria
print("High-confidence rules (confidence > 0.8):")
high_conf_rules = rules_apriori[rules_apriori['confidence'] > 0.8]
print(high_conf_rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
print()

print("High-lift rules (lift > 1.5):")
high_lift_rules = rules_apriori[rules_apriori['lift'] > 1.5]
print(high_lift_rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
print()

print("Rules with conviction > 2:")
high_conviction_rules = rules_apriori[rules_apriori['conviction'] > 2]
print(high_conviction_rules[['antecedents', 'consequents', 'support', 'confidence', 'lift', 'conviction']])
print()

# Find rules containing specific items
def rules_with_item(rules_df, item):
    """Find rules that contain a specific item in antecedents or consequents"""
    return rules_df[
        rules_df['antecedents'].apply(lambda x: item in x) | 
        rules_df['consequents'].apply(lambda x: item in x)
    ]

bread_rules = rules_with_item(rules_apriori, 'bread')
print("Rules involving bread:")
print(bread_rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
```

### Parameter Sensitivity Analysis
```python
# Analyze how different parameters affect results
support_thresholds = [0.2, 0.3, 0.4, 0.5]
confidence_thresholds = [0.5, 0.6, 0.7, 0.8]

results_summary = []

for min_support in support_thresholds:
    for min_confidence in confidence_thresholds:
        # Generate frequent itemsets
        frequent_itemsets = apriori(df, min_support=min_support, use_colnames=True)
        
        if len(frequent_itemsets) > 0:
            # Generate rules
            rules = association_rules(frequent_itemsets, metric="confidence", min_threshold=min_confidence)
            
            results_summary.append({
                'min_support': min_support,
                'min_confidence': min_confidence,
                'n_itemsets': len(frequent_itemsets),
                'n_rules': len(rules),
                'max_lift': rules['lift'].max() if len(rules) > 0 else 0,
                'avg_confidence': rules['confidence'].mean() if len(rules) > 0 else 0
            })

# Convert to DataFrame for analysis
summary_df = pd.DataFrame(results_summary)
print("Parameter sensitivity analysis:")
print(summary_df)
print()

# Visualize parameter effects
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Number of rules
pivot_rules = summary_df.pivot('min_support', 'min_confidence', 'n_rules')
sns.heatmap(pivot_rules, annot=True, fmt='g', cmap='YlGnBu', ax=axes[0,0])
axes[0,0].set_title('Number of Rules')

# Maximum lift
pivot_lift = summary_df.pivot('min_support', 'min_confidence', 'max_lift')
sns.heatmap(pivot_lift, annot=True, fmt='.2f', cmap='RdYlGn', ax=axes[0,1])
axes[0,1].set_title('Maximum Lift')

# Average confidence
pivot_conf = summary_df.pivot('min_support', 'min_confidence', 'avg_confidence')
sns.heatmap(pivot_conf, annot=True, fmt='.2f', cmap='Blues', ax=axes[1,0])
axes[1,0].set_title('Average Confidence')

# Number of itemsets
pivot_itemsets = summary_df.pivot('min_support', 'min_confidence', 'n_itemsets')
sns.heatmap(pivot_itemsets, annot=True, fmt='g', cmap='Purples', ax=axes[1,1])
axes[1,1].set_title('Number of Itemsets')

plt.tight_layout()
plt.show()
```

### Real-world Example with Online Retail Data
```python
# Load and prepare online retail dataset (simulated)
np.random.seed(42)

# Create simulated retail transactions
products = ['Laptop', 'Mouse', 'Keyboard', 'Monitor', 'Headphones', 'USB Drive', 
           'Webcam', 'Microphone', 'Printer', 'Scanner', 'External HDD', 'Router']

# Generate transactions
retail_transactions = []
for _ in range(500):
    basket_size = np.random.poisson(3) + 1  # Average basket size of 4
    basket = np.random.choice(products, size=min(basket_size, len(products)), replace=False)
    retail_transactions.append(list(basket))

print("Sample retail transactions:")
for i, transaction in enumerate(retail_transactions[:5], 1):
    print(f"{i}. {transaction}")
print("...")

# Transform to one-hot encoded format
te_retail = TransactionEncoder()
te_retail_ary = te_retail.fit_transform(retail_transactions)
df_retail = pd.DataFrame(te_retail_ary, columns=te_retail.columns_)

print(f"Retail dataset shape: {df_retail.shape}")

# Apply Apriori
frequent_itemsets_retail = apriori(df_retail, min_support=0.05, use_colnames=True)
rules_retail = association_rules(frequent_itemsets_retail, metric="lift", min_threshold=1.2)

print(f"Found {len(rules_retail)} association rules")

# Display top rules
print("Top 10 association rules by lift:")
top_rules = rules_retail.nlargest(10, 'lift')[['antecedents', 'consequents', 'support', 'confidence', 'lift']]
for idx, rule in top_rules.iterrows():
    ant = list(rule['antecedents'])
    cons = list(rule['consequents'])
    print(f"{ant} -> {cons} (support: {rule['support']:.3f}, confidence: {rule['confidence']:.3f}, lift: {rule['lift']:.2f})")
```

### Rule Interpretation and Business Insights
```python
def interpret_rule(rule):
    """Interpret an association rule in business terms"""
    antecedents = list(rule['antecedents'])
    consequents = list(rule['consequents'])
    
    interpretation = f"When customers buy {', '.join(antecedents)}, "
    interpretation += f"they are {rule['confidence']:.1%} likely to also buy {', '.join(consequents)}. "
    interpretation += f"This combination occurs in {rule['support']:.1%} of all transactions. "
    
    if rule['lift'] > 1.5:
        interpretation += "This is a strong association (high lift)."
    elif rule['lift'] > 1.2:
        interpretation += "This is a moderate association."
    else:
        interpretation += "This is a weak association."
    
    return interpretation

# Interpret top rules
print("Business Interpretation of Top Rules:")
print("=" * 60)

for idx, rule in rules_retail.nlargest(5, 'lift').iterrows():
    print(f"Rule {idx+1}:")
    print(interpret_rule(rule))
    print()

# Create actionable recommendations
def generate_recommendations(rules_df, min_lift=1.5, min_confidence=0.6):
    """Generate business recommendations from association rules"""
    strong_rules = rules_df[(rules_df['lift'] >= min_lift) & (rules_df['confidence'] >= min_confidence)]
    
    recommendations = []
    
    for _, rule in strong_rules.iterrows():
        ant = list(rule['antecedents'])
        cons = list(rule['consequents'])
        
        if len(ant) == 1 and len(cons) == 1:
            rec = f"Place {ant[0]} and {cons[0]} near each other in the store"
        elif len(ant) == 1:
            rec = f"When promoting {ant[0]}, suggest bundling with {', '.join(cons)}"
        else:
            rec = f"Create a bundle offer for {', '.join(ant)} and {', '.join(cons)}"
        
        recommendations.append(rec)
    
    return list(set(recommendations))  # Remove duplicates

recommendations = generate_recommendations(rules_retail)
print("Business Recommendations:")
print("=" * 30)
for i, rec in enumerate(recommendations[:5], 1):
    print(f"{i}. {rec}")
```

## Tasks

Create a Python file named `association_rules.py` in this folder and implement the following:

1. Prepare transaction data for association rule mining
2. Implement Apriori and FP-Growth algorithms
3. Generate and evaluate association rules with different metrics
4. Visualize rule metrics and relationships
5. Interpret rules for business insights and recommendations

## Additional Resources

- Association rule mining tutorial
- Market basket analysis examples
- mlxtend library documentation