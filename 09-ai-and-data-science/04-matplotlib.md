# Chapter 52: Matplotlib — Visualizing Data

> A picture is worth a thousand rows of data. Matplotlib turns numbers into insight.

---

## 🎯 What You'll Learn

- Creating line, bar, scatter, pie, and histogram charts
- Customizing colours, labels, and styles
- Subplots for multi-panel figures
- Seaborn for beautiful statistical plots
- Saving figures to files

---

## 📦 Installation

```bash
pip install matplotlib seaborn
```

```python
import matplotlib.pyplot as plt
import numpy as np
```

---

## 📈 Line Chart

```python
import matplotlib.pyplot as plt
import numpy as np

months  = ["Jan", "Feb", "Mar", "Apr", "May", "Jun",
           "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
sales   = [12000, 15000, 13500, 18000, 22000, 19500,
           25000, 28000, 24000, 21000, 17000, 30000]
expenses= [8000,  9000,  8500, 10000, 12000, 11000,
           13000, 14000, 13500, 12000, 10000, 16000]

plt.figure(figsize=(12, 5))
plt.plot(months, sales,    marker="o", linewidth=2, label="Revenue",  color="#2196F3")
plt.plot(months, expenses, marker="s", linewidth=2, label="Expenses", color="#F44336")

plt.fill_between(range(12), sales, expenses, alpha=0.1, color="green")

plt.title("Monthly Revenue vs Expenses (2024)", fontsize=14, fontweight="bold")
plt.xlabel("Month")
plt.ylabel("Amount (₹)")
plt.legend()
plt.grid(True, alpha=0.3)
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig("revenue.png", dpi=150)
plt.show()
```

---

## 📊 Bar Chart

```python
subjects = ["Math", "Science", "English", "History", "CS"]
class_a  = [78, 85, 72, 90, 95]
class_b  = [82, 79, 88, 75, 91]

x = np.arange(len(subjects))
width = 0.35

fig, ax = plt.subplots(figsize=(10, 6))
bars1 = ax.bar(x - width/2, class_a, width, label="Class A", color="#4CAF50")
bars2 = ax.bar(x + width/2, class_b, width, label="Class B", color="#FF9800")

# Add value labels on top of bars
for bar in bars1:
    ax.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1,
            f"{bar.get_height()}", ha="center", va="bottom", fontsize=9)

ax.set_title("Subject Performance by Class")
ax.set_xticks(x)
ax.set_xticklabels(subjects)
ax.set_ylabel("Average Score")
ax.legend()
ax.set_ylim(0, 110)
plt.tight_layout()
plt.show()
```

---

## 🔵 Scatter Plot

```python
np.random.seed(42)
study_hours = np.random.uniform(1, 10, 80)
scores = study_hours * 7 + np.random.normal(0, 8, 80)
scores = np.clip(scores, 30, 100)

plt.figure(figsize=(8, 6))
scatter = plt.scatter(study_hours, scores, c=scores, cmap="RdYlGn",
                      alpha=0.7, s=60, edgecolors="white", linewidths=0.5)
plt.colorbar(scatter, label="Score")

# Trend line
z = np.polyfit(study_hours, scores, 1)
p = np.poly1d(z)
x_line = np.linspace(1, 10, 100)
plt.plot(x_line, p(x_line), "r--", alpha=0.8, label=f"Trend: y={z[0]:.1f}x+{z[1]:.1f}")

plt.xlabel("Study Hours per Day")
plt.ylabel("Exam Score")
plt.title("Study Hours vs Exam Performance")
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

---

## 🥧 Pie Chart

```python
languages = ["Python", "JavaScript", "Java", "C++", "Others"]
usage = [32, 28, 18, 12, 10]
colors = ["#4CAF50", "#FF9800", "#2196F3", "#9C27B0", "#607D8B"]
explode = (0.05, 0, 0, 0, 0)

plt.figure(figsize=(8, 8))
wedges, texts, autotexts = plt.pie(
    usage, labels=languages, colors=colors, explode=explode,
    autopct="%1.1f%%", startangle=90, shadow=True
)
for text in autotexts:
    text.set_fontweight("bold")

plt.title("Most Popular Programming Languages 2024", fontsize=14)
plt.show()
```

---

## 📉 Histogram

```python
np.random.seed(0)
scores = np.random.normal(72, 12, 500)   # Mean=72, Std=12
scores = np.clip(scores, 0, 100)

plt.figure(figsize=(10, 6))
n, bins, patches = plt.hist(scores, bins=20, edgecolor="white",
                             color="#2196F3", alpha=0.8)

# Colour bars by grade
for patch, left, right in zip(patches, bins, bins[1:]):
    mid = (left + right) / 2
    if mid >= 90:   patch.set_facecolor("#4CAF50")
    elif mid >= 75: patch.set_facecolor("#8BC34A")
    elif mid >= 60: patch.set_facecolor("#FFC107")
    elif mid >= 45: patch.set_facecolor("#FF9800")
    else:           patch.set_facecolor("#F44336")

plt.axvline(scores.mean(), color="black", linestyle="--",
            label=f"Mean: {scores.mean():.1f}")
plt.xlabel("Score")
plt.ylabel("Number of Students")
plt.title("Score Distribution (n=500)")
plt.legend()
plt.show()
```

---

## 🪟 Subplots — Multiple Charts

```python
fig, axes = plt.subplots(2, 2, figsize=(14, 10))
fig.suptitle("Sales Dashboard", fontsize=16, fontweight="bold")

# Top-left: line
months = ["Jan","Feb","Mar","Apr","May","Jun"]
axes[0,0].plot(months, [10, 14, 12, 18, 22, 20], marker="o")
axes[0,0].set_title("Monthly Revenue")

# Top-right: bar
cats = ["A", "B", "C", "D"]
axes[0,1].bar(cats, [40, 60, 35, 80], color=["#F44336","#FF9800","#4CAF50","#2196F3"])
axes[0,1].set_title("Sales by Category")

# Bottom-left: pie
axes[1,0].pie([35, 25, 20, 20], labels=["Q1","Q2","Q3","Q4"], autopct="%1.0f%%")
axes[1,0].set_title("Quarterly Share")

# Bottom-right: scatter
x = np.random.rand(50)
y = x + np.random.rand(50) * 0.3
axes[1,1].scatter(x, y, alpha=0.6)
axes[1,1].set_title("Correlation")

plt.tight_layout()
plt.savefig("dashboard.png", dpi=150, bbox_inches="tight")
plt.show()
```

---

## 🎨 Seaborn — Statistical Visualizations

```python
import seaborn as sns
import pandas as pd

# Use seaborn's built-in datasets
tips = sns.load_dataset("tips")

fig, axes = plt.subplots(1, 2, figsize=(14, 5))

# Distribution plot
sns.histplot(tips["total_bill"], kde=True, ax=axes[0])
axes[0].set_title("Bill Distribution")

# Box plot — great for comparing groups
sns.boxplot(x="day", y="total_bill", hue="sex", data=tips, ax=axes[1])
axes[1].set_title("Bill by Day and Gender")

plt.tight_layout()
plt.show()

# Heatmap — correlation matrix
numeric_tips = tips.select_dtypes(include="number")
corr = numeric_tips.corr()
plt.figure(figsize=(8, 6))
sns.heatmap(corr, annot=True, cmap="coolwarm", center=0, fmt=".2f")
plt.title("Correlation Heatmap")
plt.show()
```

---

## 📌 Key Takeaways

- `plt.figure()` → `plt.plot/bar/scatter/pie/hist()` → `plt.show()`
- Always add: `title`, `xlabel`, `ylabel`, `legend`
- Use `plt.subplots(rows, cols)` for multiple charts in one figure
- `plt.savefig("chart.png", dpi=150)` saves to file
- Seaborn wraps Matplotlib with beautiful statistical defaults

---

*Next: [What is Machine Learning? →](05-what-is-ml.md)*
