

# 📊 Customer Demographics & Marketing Campaign Analytics Dashboard

> A complete Python data science pipeline and simulation framework for analyzing customer demographics, income levels, spending behavior, and targeted marketing campaigns.

---

## 🚀 Project Overview

In this project, I built an end-to-end data analytics pipeline examining a dataset of 2,500 customer records. My core objective was to analyze income distributions, evaluate purchasing behavior across multiple product categories, run customer segment simulations, and formulate actionable, high-ROI marketing strategies.

---

## 🛠️ Step-by-Step Implementation & Process

### 📂 Phase 1: Data Loading & EDA Profiling

**What I did:**
* 🧭 I imported core data science libraries (`pandas`, `matplotlib`, and `seaborn`) to handle data manipulation and generate visualizations.
* 📊 I loaded my marketing dataset (`marketing_data.csv`) containing 2,500 customer records and verified its dimensions (`df.shape`).
* 📈 I ran statistical profiling (`df.describe()`) to capture baseline parameters like mean, median, standard deviation, minimum, and maximum values across all numeric variables.
* 📉 I plotted a histogram with Kernel Density Estimation (KDE) using `sns.histplot` to visualize customer income distributions and establish baseline earnings.

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset and inspect basic shape
df = pd.read_csv('marketing_data.csv')
print(f"Dataset Shape: {df.shape[0]} rows, {df.shape[1]} columns")
display(df.describe())

# Plot income distribution
plt.figure(figsize=(10, 6))
sns.histplot(df['Income'], kde=True, color='royalblue', bins=30)
plt.title('Customer Income Distribution')
plt.xlabel('Income')
plt.ylabel('Customer Count')
plt.show()
```

---

### 💰 Phase 2: Spend Distribution & Correlation Analysis

**What I did:**
* 📦 I plotted a histogram with Kernel Density Estimation (KDE) for `Total_Spend` using `sns.histplot` to inspect the spread and distribution of customer expenditures.
* 🔀 I built a scatter plot (`sns.scatterplot`) mapping Customer Income against Total Spend with a defined transparency (`alpha=0.6`) to evaluate the direct relationship between customer earnings and purchasing habits.
* 💡 **Insight Found:** While customer income follows a bell-curve distribution centered around $70,000–$80,000, total spending is right-skewed with most customers spending between $400 and $1,000. The scatter plot proved a strong positive correlation: once income surpassed $80,000, spending spiked dramatically, creating a distinct cluster of high-value shoppers.

```python
# Plot total spend distribution
plt.figure(figsize=(10, 6))
sns.histplot(df['Total_Spend'], kde=True, color='green', bins=30)
plt.title('Customer Total Spend Distribution')
plt.show()

# Plot income vs. total spend correlation
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df, x='Income', y='Total_Spend', alpha=0.6, color='purple')
plt.title('Income vs Total Spend')
plt.show()
```

---

### 🛒 Phase 3: Product Category Analysis & Campaign Simulation

**What I did:**
* 🔍 I isolated all product expenditure columns (prefixed with `Mnt`) using list comprehension and summed them up to rank total sales across all 2,500 customers, identifying top revenue drivers like `HomeLiving` and `Electronics`.
* ⚙️ I calculated the overall median total spend and engineered a conditional segmentation rule (`df.loc`) to split my audience into two distinct tiers: **High-Value Targets** (Income > $80,000 and Total Spend > Median Spend) and **Standard** customers.
* 📋 I used pandas grouping and aggregation (`.groupby('Segment').agg()`) to compute summary metrics—such as average income, total spend, and specific category expenditures—successfully separating 820 High-Value Targets from 1,680 Standard customers.

```python
# Calculate category totals
product_cols = [col for col in df.columns if col.startswith('Mnt')]
category_totals = df[product_cols].sum().sort_values(ascending=False)
print(category_totals)

# Customer segmentation logic
median_spend = df['Total_Spend'].median()

df['Segment'] = 'Standard'
df.loc[(df['Income'] > 80000) & (df['Total_Spend'] > median_spend), 'Segment'] = 'High-Value Target'

# Aggregate metrics by segment
simulation_results = df.groupby('Segment').agg(
    Customer_Count=('Income', 'count'),
    Avg_Income=('Income', 'mean'),
    Avg_Total_Spend=('Total_Spend', 'mean'),
    Avg_HomeLiving=('MntHomeLiving', 'mean'),
    Avg_Electronics=('MntElectronics', 'mean')
).round(2)

display(simulation_results)
```

---

### 🎯 Phase 4: Visual Comparison & Strategic Recommendations

**What I did:**
* 📐 I filtered the simulation summary table to focus on top-performing categories and reshaped the data structure using pandas `.melt()` to convert columns into rows for plotting compatibility.
* 📊 I generated a grouped bar chart (`sns.barplot`) comparing average category spending (`HomeLiving` and `Electronics`) across both customer segments using a distinct color palette (`palette='Set2'`).
* 🚀 **Strategic Takeaways I Implemented:**
  * **Reallocate Ad Spend:** Focused marketing campaigns toward the 820 high-value customers (averaging $1,173.58 in spend) to maximize Return on Ad Spend (ROAS).
  * **Product Bundling:** Designed smart-home electronics and home decor product bundles, as high-value targets spend more than double in these categories compared to standard customers.
  * **Threshold Incentives:** Implemented minimum spending incentives for standard customers to lift their basket sizes closer to the median threshold.

```python
# Reshape data for plotting
plot_data = simulation_results[['Avg_HomeLiving', 'Avg_Electronics']].reset_index()
plot_data_melted = plot_data.melt(id_vars='Segment', var_name='Product Category', value_name='Average Spend')

# Plot category spend comparison
plt.figure(figsize=(10, 6))
sns.barplot(data=plot_data_melted, x='Product Category', y='Average Spend', hue='Segment', palette='Set2')
plt.title('Average Spend by Product Category across Customer Segments')
plt.show()
```

---

## ✅ Outcome

This project demonstrates a practical marketing analytics workflow: from raw customer data to segmentation, campaign simulation, and strategic recommendations. It highlights how data-driven insights can inform budget allocation, customer targeting, and product strategy.
