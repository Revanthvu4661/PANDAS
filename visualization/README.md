# 📊 Data Visualization — Matplotlib & Seaborn

After cleaning your data, the next step is to **visualize it** — charts make patterns visible that numbers alone can't show.  
This section covers both **Matplotlib** (full control) and **Seaborn** (beautiful charts with less code).

---

## 📌 Table of Contents

- [Matplotlib vs Seaborn](#matplotlib-vs-seaborn)
- [Setup](#setup)
- [Matplotlib Basics](#matplotlib-basics)
  - [Line Chart](#line-chart)
  - [Bar Chart](#bar-chart)
  - [Horizontal Bar](#horizontal-bar)
  - [Histogram](#histogram)
  - [Scatter Plot](#scatter-plot)
  - [Pie Chart](#pie-chart)
  - [Subplots](#subplots)
- [Seaborn Basics](#seaborn-basics)
  - [Bar Plot](#seaborn-bar-plot)
  - [Count Plot](#count-plot)
  - [Box Plot](#box-plot)
  - [Violin Plot](#violin-plot)
  - [Scatter Plot](#seaborn-scatter-plot)
  - [Heatmap](#heatmap)
  - [Pair Plot](#pair-plot)
  - [Distribution Plot](#distribution-plot)
- [Customization Tips](#customization-tips)
- [Cheat Sheet](#cheat-sheet)

---

## Matplotlib vs Seaborn

| | Matplotlib | Seaborn |
|--|-----------|---------|
| Control | Full control over every detail | Smart defaults, less code |
| Code length | More lines | Fewer lines |
| Works with | Any data | Pandas DataFrames best |
| Best for | Custom charts, fine-tuning | Statistical visualizations |
| Built on | — | Built on top of Matplotlib |

> **Tip:** Use Seaborn for quick, beautiful charts. Use Matplotlib when you need to customize every detail or combine multiple charts.

---

## Setup

```python
# Install
pip install matplotlib seaborn
```

```python
# Import at the top of every file
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## Matplotlib Basics

In Matplotlib, the basic flow is always:
1. Create the chart
2. Add labels/title
3. Show it with `plt.show()`

---

### Line Chart

Best for: trends over time

```python
import matplotlib.pyplot as plt

months = ["Jan", "Feb", "Mar", "Apr", "May"]
sales  = [12000, 15000, 11000, 18000, 22000]

plt.plot(months, sales)

plt.title("Monthly Sales")
plt.xlabel("Month")
plt.ylabel("Sales (₹)")

plt.show()
```

**With styling:**

```python
plt.plot(months, sales, color="blue", linewidth=2, marker="o", linestyle="--")
plt.title("Monthly Sales", fontsize=14)
plt.grid(True)
plt.show()
```

---

### Bar Chart

Best for: comparing categories

```python
categories = ["Electronics", "Clothing", "Food", "Books"]
revenue    = [45000, 32000, 28000, 15000]

plt.bar(categories, revenue, color="steelblue")

plt.title("Revenue by Category")
plt.xlabel("Category")
plt.ylabel("Revenue (₹)")

plt.show()
```

**With a Pandas DataFrame:**

```python
# Group data first, then plot
category_sales = df.groupby("Category")["Sales"].sum()

category_sales.plot(kind="bar", color="coral", figsize=(10, 5))
plt.title("Sales by Category")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

---

### Horizontal Bar

Best for: long category names

```python
plt.barh(categories, revenue, color="teal")

plt.title("Revenue by Category")
plt.xlabel("Revenue (₹)")

plt.show()
```

---

### Histogram

Best for: distribution of a numeric column (how spread out is the data?)

```python
plt.hist(df["Age"], bins=10, color="skyblue", edgecolor="black")

plt.title("Age Distribution")
plt.xlabel("Age")
plt.ylabel("Count")

plt.show()
```

> **bins** = how many bars to split the data into. More bins = more detail.

---

### Scatter Plot

Best for: relationship between two numeric columns

```python
plt.scatter(df["Experience"], df["Salary"], color="purple", alpha=0.6)

plt.title("Experience vs Salary")
plt.xlabel("Years of Experience")
plt.ylabel("Salary (₹)")

plt.show()
```

> `alpha=0.6` makes points slightly transparent so overlapping points are visible.

---

### Pie Chart

Best for: showing proportions / parts of a whole

```python
labels  = ["Electronics", "Clothing", "Food", "Books"]
sizes   = [40, 25, 20, 15]
colors  = ["#4e79a7", "#f28e2b", "#76b7b2", "#e15759"]

plt.pie(sizes, labels=labels, colors=colors, autopct="%1.1f%%", startangle=90)

plt.title("Revenue Share by Category")
plt.show()
```

> `autopct="%1.1f%%"` displays the percentage inside each slice.

---

### Subplots

Show multiple charts side by side in one figure.

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 5))  # 1 row, 2 columns

# First chart
axes[0].bar(categories, revenue, color="steelblue")
axes[0].set_title("Revenue by Category")

# Second chart
axes[1].plot(months, sales, marker="o", color="coral")
axes[1].set_title("Monthly Sales Trend")

plt.tight_layout()   # prevents charts from overlapping
plt.show()
```

---

## Seaborn Basics

Seaborn works directly with Pandas DataFrames — just pass `data=df` and column names.

---

### Seaborn Bar Plot

Best for: comparing averages across categories

```python
sns.barplot(data=df, x="Category", y="Sales")

plt.title("Average Sales by Category")
plt.show()
```

> By default, Seaborn shows the **mean** with an error bar showing uncertainty.  
> Add `estimator=sum` to show total instead of average.

```python
import numpy as np
sns.barplot(data=df, x="Category", y="Sales", estimator=sum, ci=None)
```

---

### Count Plot

Best for: counting how many rows belong to each category

```python
sns.countplot(data=df, x="City")

plt.title("Number of Students per City")
plt.xticks(rotation=45)
plt.show()
```

---

### Box Plot

Best for: showing spread, median, and outliers in numeric data

```python
sns.boxplot(data=df, x="Category", y="Sales")

plt.title("Sales Distribution by Category")
plt.show()
```

> **How to read a box plot:**
> - The middle line = median
> - The box = middle 50% of data
> - The whiskers = range (excluding outliers)
> - Dots outside = outliers

---

### Violin Plot

Like a box plot but also shows the full distribution shape.

```python
sns.violinplot(data=df, x="Department", y="Salary")

plt.title("Salary Distribution by Department")
plt.show()
```

---

### Seaborn Scatter Plot

```python
sns.scatterplot(data=df, x="Experience", y="Salary", hue="Department")

plt.title("Experience vs Salary")
plt.show()
```

> `hue="Department"` colors each point by department automatically — very useful!

---

### Heatmap

Best for: showing correlation between all numeric columns

```python
# First calculate correlation
correlation = df.corr()

# Then plot the heatmap
sns.heatmap(correlation, annot=True, cmap="coolwarm", fmt=".2f")

plt.title("Correlation Heatmap")
plt.show()
```

> - `annot=True` shows the number inside each cell
> - `cmap="coolwarm"` → red = positive correlation, blue = negative
> - Values close to 1 = strong positive relationship, close to -1 = strong negative

---

### Pair Plot

Shows scatter plots between ALL numeric columns at once — great for exploration.

```python
sns.pairplot(df, hue="Category")
plt.show()
```

> Warning: slow on large DataFrames. Use on small/medium data only.

---

### Distribution Plot

Shows the distribution (shape) of a single column.

```python
# Histogram + smooth curve
sns.histplot(df["Age"], kde=True, bins=20)

plt.title("Age Distribution")
plt.show()
```

> `kde=True` adds a smooth curve (Kernel Density Estimate) on top of the histogram.

---

## Customization Tips

### Figure size

```python
plt.figure(figsize=(12, 6))   # width=12, height=6 inches
```

### Seaborn themes

```python
sns.set_theme(style="whitegrid")   # clean white background with gridlines
sns.set_theme(style="darkgrid")    # dark background
sns.set_theme(style="ticks")       # minimal
```

### Colors

```python
# Matplotlib named colors
color="steelblue"
color="#4e79a7"     # hex code

# Seaborn color palettes
palette="Blues"
palette="Set2"
palette="coolwarm"
palette="viridis"
```

### Save a chart

```python
plt.savefig("chart.png", dpi=150, bbox_inches="tight")
# Always call savefig() BEFORE plt.show()
```

### Rotate x-axis labels

```python
plt.xticks(rotation=45)
```

---

## Cheat Sheet

| Chart Type | When to Use | Library | Code |
|-----------|-------------|---------|------|
| Line | Trends over time | Matplotlib | `plt.plot(x, y)` |
| Bar | Compare categories | Matplotlib | `plt.bar(x, y)` |
| Horizontal Bar | Long labels | Matplotlib | `plt.barh(x, y)` |
| Histogram | Distribution of one column | Matplotlib | `plt.hist(col, bins=10)` |
| Scatter | Relationship between 2 numbers | Matplotlib | `plt.scatter(x, y)` |
| Pie | Proportions / shares | Matplotlib | `plt.pie(sizes, labels=...)` |
| Bar (avg) | Category averages | Seaborn | `sns.barplot(data, x, y)` |
| Count | How many per category | Seaborn | `sns.countplot(data, x)` |
| Box | Spread + outliers | Seaborn | `sns.boxplot(data, x, y)` |
| Violin | Distribution shape | Seaborn | `sns.violinplot(data, x, y)` |
| Scatter + color | Relationship + groups | Seaborn | `sns.scatterplot(data, x, y, hue)` |
| Heatmap | Correlation between columns | Seaborn | `sns.heatmap(df.corr(), annot=True)` |
| Pair Plot | All columns vs all | Seaborn | `sns.pairplot(df)` |
| Distribution | Shape of one column | Seaborn | `sns.histplot(col, kde=True)` |

---

⬅️ Previous: [Data Cleaning](../data-cleaning/README.md) &nbsp;&nbsp;|&nbsp;&nbsp; ➡️ Next: [Projects →](../projects/README.md)

---

> Made with 🐼 by **Revanth** | VFSTR CSE
