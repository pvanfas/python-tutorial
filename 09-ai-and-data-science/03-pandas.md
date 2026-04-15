# Chapter 51: Pandas — Working with Data Tables

> Pandas is to Python what Excel is to office work — but a thousand times more powerful.

---

## 🎯 What You'll Learn

- What a DataFrame is and how to create one
- Reading data from CSV files
- Filtering, sorting, and selecting data
- Handling missing data
- Basic data analysis with Pandas

---

## 📦 Installation & Import

```bash
pip install pandas
```

```python
import pandas as pd
import numpy as np
```

---

## 🗂️ The Two Core Pandas Objects

### 1. Series — A Labeled Column

```python
# A Series is like a single column of data
ages = pd.Series([22, 25, 21, 23, 24],
                  index=["Arjun", "Priya", "Sara", "Ali", "Ravi"])

print(ages)
# Arjun    22
# Priya    25
# Sara     21
# Ali      23
# Ravi     24
# dtype: int64

print(ages["Priya"])   # → 25
print(ages.mean())     # → 23.0
```

### 2. DataFrame — A Full Table

```python
# A DataFrame is a table (like a spreadsheet)
data = {
    "name":     ["Arjun", "Priya", "Sara", "Ali", "Ravi"],
    "age":      [22, 25, 21, 23, 24],
    "city":     ["Kochi", "Kozhikode", "Kannur", "Malappuram", "Thrissur"],
    "score":    [85, 92, 78, 88, 95],
    "passed":   [True, True, True, True, True]
}

df = pd.DataFrame(data)
print(df)
```

Output:
```
     name  age        city  score  passed
0   Arjun   22       Kochi     85    True
1   Priya   25   Kozhikode     92    True
2    Sara   21      Kannur     78    True
3     Ali   23  Malappuram     88    True
4    Ravi   24    Thrissur     95    True
```

---

## 📥 Reading Data from Files

In real projects, you'll load data from files:

```python
# Read CSV file
df = pd.read_csv("students.csv")

# Read Excel file
df = pd.read_excel("data.xlsx")

# Read JSON
df = pd.read_json("data.json")

# Write CSV
df.to_csv("output.csv", index=False)
```

---

## 🔍 Exploring a DataFrame

```python
print(df.head())      # First 5 rows
print(df.tail(3))     # Last 3 rows
print(df.shape)       # (rows, columns)
print(df.columns)     # Column names
print(df.dtypes)      # Data types of each column
print(df.info())      # Summary of the DataFrame
print(df.describe())  # Statistical summary of numeric columns
```

`df.describe()` is incredibly useful — it gives you:
```
         age      score
count   5.00       5.00
mean   23.00      87.60
std     1.58       6.54
min    21.00      78.00
25%    22.00      85.00
50%    23.00      88.00
75%    24.00      92.00
max    25.00      95.00
```

---

## 🎯 Selecting Data

```python
# Select a single column (returns a Series)
print(df["name"])
print(df["score"])

# Select multiple columns (returns a DataFrame)
print(df[["name", "score"]])

# Select rows by index number (iloc)
print(df.iloc[0])       # First row
print(df.iloc[1:3])     # Rows 1 and 2

# Select rows by label (loc)
print(df.loc[df["city"] == "Kochi"])
```

---

## 🔎 Filtering Data (The Magic of Pandas)

```python
# Students who scored above 85
high_scorers = df[df["score"] > 85]
print(high_scorers)

# Multiple conditions: age < 24 AND score > 80
young_high = df[(df["age"] < 24) & (df["score"] > 80)]

# Students from Kozhikode OR Kochi
specific = df[df["city"].isin(["Kozhikode", "Kochi"])]

# Students whose name starts with 'A'
a_names = df[df["name"].str.startswith("A")]
```

---

## 📊 Sorting & Ranking

```python
# Sort by score (descending)
df_sorted = df.sort_values("score", ascending=False)
print(df_sorted)

# Sort by multiple columns
df_sorted = df.sort_values(["city", "score"])

# Add a rank column
df["rank"] = df["score"].rank(ascending=False).astype(int)
```

---

## 🔧 Adding & Modifying Columns

```python
# Add a new column
df["grade"] = df["score"].apply(lambda x:
    "A" if x >= 90 else
    "B" if x >= 75 else
    "C" if x >= 60 else "F"
)

# Modify existing column
df["score"] = df["score"] + 2   # Add 2 bonus marks to everyone

# Apply a custom function
def age_group(age):
    return "Teen" if age < 20 else "Young Adult" if age < 25 else "Adult"

df["age_group"] = df["age"].apply(age_group)
```

---

## ❓ Handling Missing Data

Real data is messy. Missing values appear as `NaN` (Not a Number).

```python
# Create sample data with missing values
data = {
    "name":   ["Alice", "Bob", "Charlie", "Diana", "Eve"],
    "score":  [85, np.nan, 90, np.nan, 78],
    "grade":  ["A", "B", None, "A", "C"]
}
df = pd.DataFrame(data)

# Detect missing values
print(df.isnull())          # True where data is missing
print(df.isnull().sum())    # Count missing per column

# Drop rows with any missing value
df_clean = df.dropna()

# Fill missing values
df["score"] = df["score"].fillna(df["score"].mean())  # Fill with mean
df["grade"] = df["grade"].fillna("Unknown")            # Fill with text
```

---

## 📈 GroupBy — Aggregating Data

```python
data = {
    "city":   ["Kochi", "Kozhikode", "Kochi", "Kozhikode", "Kochi"],
    "score":  [85, 92, 78, 88, 95],
    "gender": ["M", "F", "F", "M", "M"]
}
df = pd.DataFrame(data)

# Average score by city
print(df.groupby("city")["score"].mean())

# Multiple aggregations
print(df.groupby("city")["score"].agg(["mean", "max", "min", "count"]))

# Group by multiple columns
print(df.groupby(["city", "gender"])["score"].mean())
```

---

## 🏋️ Activity: Student Performance Analysis

```python
import pandas as pd
import numpy as np

# Create a realistic dataset
np.random.seed(42)
n = 100   # 100 students

df = pd.DataFrame({
    "student_id": range(1, n+1),
    "name": [f"Student_{i}" for i in range(1, n+1)],
    "age": np.random.randint(18, 25, n),
    "gender": np.random.choice(["Male", "Female"], n),
    "city": np.random.choice(["Kochi", "Kozhikode", "Thrissur", "Kannur", "Malappuram"], n),
    "math_score": np.random.randint(40, 100, n),
    "science_score": np.random.randint(40, 100, n),
    "english_score": np.random.randint(40, 100, n),
    "attendance": np.random.randint(60, 100, n)
})

# Calculate average score
df["average"] = (df["math_score"] + df["science_score"] + df["english_score"]) / 3

# Assign grades
def assign_grade(avg):
    if avg >= 90: return "A+"
    elif avg >= 80: return "A"
    elif avg >= 70: return "B"
    elif avg >= 60: return "C"
    else: return "F"

df["grade"] = df["average"].apply(assign_grade)

# Analysis
print("📊 Overall Performance Summary")
print("=" * 40)
print(f"Total Students:    {len(df)}")
print(f"Class Average:     {df['average'].mean():.2f}")
print(f"Highest Average:   {df['average'].max():.2f}")
print(f"Lowest Average:    {df['average'].min():.2f}")

print("\n📍 Average Score by City:")
city_avg = df.groupby("city")["average"].mean().sort_values(ascending=False)
print(city_avg.round(2))

print("\n⚤ Performance by Gender:")
gender_avg = df.groupby("gender")["average"].mean()
print(gender_avg.round(2))

print("\n🎓 Grade Distribution:")
print(df["grade"].value_counts())

# Top 5 students
print("\n🏆 Top 5 Students:")
top5 = df.nlargest(5, "average")[["name", "city", "average", "grade"]]
print(top5.to_string(index=False))

# Export to CSV
df.to_csv("student_analysis.csv", index=False)
print("\n✅ Data saved to student_analysis.csv")
```

---

## 🧠 Check Your Understanding

1. What is the difference between a Pandas Series and a DataFrame?
2. What does `df.describe()` show?
3. How do you filter a DataFrame to show only rows where `score > 80`?
4. What is `NaN` and how do you handle it?
5. What does `groupby()` do? Give an example use case.

---

## 📌 Key Takeaways

- **Series** = one column with labels; **DataFrame** = full table
- Load real data with `pd.read_csv()` or `pd.read_excel()`
- Filter with boolean expressions: `df[df["col"] > value]`
- Handle missing data with `fillna()` or `dropna()`
- `groupby()` splits data into groups for aggregation
- Pandas is the backbone of all data analysis in Python

---

*Next: [Matplotlib — Visualizing Data →](04-matplotlib.md)*
