# 📊 Supermarket Sales Analysis and Profitability Optimization

## 🧾 Introduction

In this project, we analyze a supermarket's sales data to uncover insights related to profitability, product performance, losses, and operational efficiency. The analysis combines transaction-level data, cost structure, and item loss rates.

---

## 📥 Load and Explore the Data

We'll begin by importing necessary libraries and loading all four annexes.

```python
import pandas as pd
import numpy as np
import seaborn as sns
import plotly.express as px
import matplotlib.pyplot as plt

# Load datasets
items_data = pd.read_csv('dataset/annex1.csv')
items_checkout = pd.read_csv('dataset/annex2.csv')
items_wholesale_price = pd.read_csv('dataset/annex3.csv')
items_loss_rate = pd.read_csv('dataset/annex4.csv')

# Preview datasets
items_data.head()
```

---

## 🧩 Dataset Overview

**1. Item Master Data**
- Contains item codes, names, and categories.

**2. Checkout Transactions**
- Includes date, time, quantity sold, selling price, and discount info.

**3. Wholesale Prices**
- Lists cost per kg per date and item.

**4. Loss Rate**
- Loss percentage for each item.

---

## 🔄 Data Preprocessing

- Merge datasets into a unified dataframe.
- Convert date and time formats.
- Handle missing values and duplicates.
- Prepare columns for calculations (revenue, cost, profit, etc).

```python
# Example of merging and preparing unified dataframe
checkout = items_checkout.merge(items_data, on='Item Code', how='left')
checkout = checkout.merge(items_wholesale_price, on=['Date', 'Item Code'], how='left')
checkout = checkout.merge(items_loss_rate[['Item Code', 'Loss Rate (%)']], on='Item Code', how='left')

# Clean and convert data types
checkout['Date'] = pd.to_datetime(checkout['Date'])
checkout['Loss Rate (%)'] = checkout['Loss Rate (%)'].fillna(0) / 100  # convert to decimal
checkout['Revenue'] = checkout['Quantity Sold (kilo)'] * checkout['Unit Selling Price (RMB/kg)']
checkout['Cost'] = checkout['Quantity Sold (kilo)'] * checkout['Wholesale Price (RMB/kg)']
checkout['Gross Profit'] = checkout['Revenue'] - checkout['Cost']
checkout['Loss Cost'] = checkout['Cost'] * checkout['Loss Rate (%)']
checkout['Net Profit'] = checkout['Gross Profit'] - checkout['Loss Cost']
```

---

## 📊 Exploratory Data Analysis (EDA)

### 1. Overall Sales Performance
- Total Revenue, Cost, and Profit
- Average Margin
- Trends over time

### 2. Product Performance
- Top-selling and top-profitable products
- Loss and return analysis
- Impact of discounts

### 3. Category Insights
- Best and worst performing categories by margin and revenue

### 4. Loss and Efficiency Analysis
- Total loss by product
- Items with highest % loss vs. revenue contribution

```python
# Example plot
top_products = checkout.groupby('Item Name')['Net Profit'].sum().sort_values(ascending=False).head(10)
fig = px.bar(top_products, title='Top 10 Most Profitable Products')
fig.show()
```

---

## 💡 Strategic Insights and Recommendations

- Which products or categories should be promoted or removed?
- How do losses and discounts affect profitability?
- Suggestions to optimize product mix and inventory.

---

## 📌 Conclusion

Summarize the key findings and business actions that could improve profitability and operational efficiency.

---