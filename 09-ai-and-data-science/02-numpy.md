# Chapter 50: NumPy — Numbers at Scale

> Python lists are great for small data. NumPy arrays are for science, math, and speed.

---

## 🎯 What You'll Learn

- What NumPy is and why it's faster than regular Python lists
- Creating and manipulating arrays
- Array math and broadcasting
- Useful NumPy functions for statistics
- Real scientific computing examples

---

## 📦 Installing & Importing NumPy

```bash
pip install numpy
```

```python
import numpy as np   # 'np' is the universal shorthand
```

---

## 🤔 Why NumPy? Lists vs Arrays

**Regular Python list:**
```python
# Multiply every element by 2
prices = [10, 20, 30, 40, 50]
doubled = [p * 2 for p in prices]   # Need a loop
```

**NumPy array:**
```python
import numpy as np
prices = np.array([10, 20, 30, 40, 50])
doubled = prices * 2   # Works directly! No loop needed
print(doubled)   # → [20 40 60 80 100]
```

NumPy arrays are:
- **Faster** — implemented in C under the hood
- **More concise** — math operations work element-wise
- **Memory efficient** — store data as raw numbers, not Python objects

> 💡 **For Physics students:** NumPy arrays behave like vectors and matrices. This is physics-friendly programming!

---

## 🔢 Creating NumPy Arrays

```python
import numpy as np

# From a list
a = np.array([1, 2, 3, 4, 5])

# Range of numbers
b = np.arange(0, 10, 2)        # → [0, 2, 4, 6, 8]

# N evenly spaced values
c = np.linspace(0, 1, 5)       # → [0.   0.25 0.5  0.75 1.  ]

# Zeros and ones
zeros = np.zeros(5)             # → [0. 0. 0. 0. 0.]
ones = np.ones(3)               # → [1. 1. 1.]

# 2D array (matrix)
matrix = np.array([[1, 2, 3],
                   [4, 5, 6],
                   [7, 8, 9]])

# Identity matrix
eye = np.eye(3)                 # 3×3 identity matrix

# Random arrays
rand = np.random.rand(5)        # 5 random values between 0 and 1
rand_int = np.random.randint(1, 100, size=10)  # 10 random integers
```

---

## 📐 Array Properties

```python
a = np.array([[1, 2, 3], [4, 5, 6]])

print(a.shape)    # → (2, 3)  — 2 rows, 3 columns
print(a.ndim)     # → 2       — 2 dimensions
print(a.size)     # → 6       — total elements
print(a.dtype)    # → int64   — data type
```

---

## ➕ Array Math — The Power of NumPy

```python
a = np.array([1, 2, 3, 4, 5])
b = np.array([10, 20, 30, 40, 50])

print(a + b)       # → [11 22 33 44 55]
print(a * b)       # → [ 10  40  90 160 250]
print(a ** 2)      # → [ 1  4  9 16 25]
print(np.sqrt(a))  # → [1.    1.414 1.732 2.    2.236]
print(a + 100)     # → [101 102 103 104 105]  (broadcast scalar)
```

---

## 🔬 Real Physics Example: Projectile Motion

```python
import numpy as np
import matplotlib.pyplot as plt

# Projectile motion: ball thrown at angle θ with velocity v₀
v0 = 20      # m/s
theta = 45   # degrees
g = 9.81     # m/s²

theta_rad = np.radians(theta)
vx = v0 * np.cos(theta_rad)
vy = v0 * np.sin(theta_rad)

# Time array: 0 to 4 seconds, 100 points
t = np.linspace(0, 4, 100)

# Position equations
x = vx * t
y = vy * t - 0.5 * g * t**2

# Find where ball hits ground (y >= 0)
mask = y >= 0
x_flight = x[mask]
y_flight = y[mask]

print(f"Max height: {np.max(y_flight):.2f} m")
print(f"Range: {np.max(x_flight):.2f} m")
print(f"Flight time: {t[mask][-1]:.2f} s")
```

This demonstrates NumPy's strength — you compute the position for **all 100 time points simultaneously**, without any loop!

---

## 📊 Statistical Functions

```python
data = np.array([23, 45, 67, 12, 89, 34, 56, 78, 90, 45])

print(f"Mean:     {np.mean(data):.2f}")
print(f"Median:   {np.median(data):.2f}")
print(f"Std Dev:  {np.std(data):.2f}")
print(f"Min:      {np.min(data)}")
print(f"Max:      {np.max(data)}")
print(f"Sum:      {np.sum(data)}")
print(f"Variance: {np.var(data):.2f}")
print(f"25th percentile: {np.percentile(data, 25):.2f}")
print(f"75th percentile: {np.percentile(data, 75):.2f}")
```

---

## 🔍 Indexing & Slicing Arrays

```python
a = np.array([10, 20, 30, 40, 50, 60, 70])

print(a[0])       # → 10       (first element)
print(a[-1])      # → 70       (last element)
print(a[2:5])     # → [30 40 50] (slice)
print(a[::2])     # → [10 30 50 70] (every 2nd)

# Boolean indexing (very powerful!)
print(a[a > 40])  # → [50 60 70] (elements > 40)
print(a[a % 20 == 0])  # → [20 40 60] (divisible by 20)

# 2D indexing
matrix = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(matrix[1, 2])      # → 6   (row 1, col 2)
print(matrix[:, 1])      # → [2, 5, 8] (all rows, col 1)
print(matrix[0, :])      # → [1, 2, 3] (row 0, all cols)
```

---

## 🏋️ Activity: Analyzing Exam Scores

```python
import numpy as np

# Simulate exam scores for 50 students
np.random.seed(42)  # For reproducible results
scores = np.random.randint(35, 100, size=50)

print("📊 Exam Score Analysis")
print("=" * 35)
print(f"Students:       {len(scores)}")
print(f"Highest score:  {np.max(scores)}")
print(f"Lowest score:   {np.min(scores)}")
print(f"Average:        {np.mean(scores):.2f}")
print(f"Median:         {np.median(scores):.2f}")
print(f"Std Deviation:  {np.std(scores):.2f}")

# Grade distribution
a_grade = np.sum(scores >= 90)
b_grade = np.sum((scores >= 75) & (scores < 90))
c_grade = np.sum((scores >= 60) & (scores < 75))
d_grade = np.sum((scores >= 45) & (scores < 60))
f_grade = np.sum(scores < 45)

print("\n📈 Grade Distribution:")
print(f"  A (90+):    {a_grade} students")
print(f"  B (75-89):  {b_grade} students")
print(f"  C (60-74):  {c_grade} students")
print(f"  D (45-59):  {d_grade} students")
print(f"  F (<45):    {f_grade} students")

pass_rate = np.mean(scores >= 45) * 100
print(f"\n✅ Pass rate: {pass_rate:.1f}%")

# Normalize scores to 0-1 range
normalized = (scores - np.min(scores)) / (np.max(scores) - np.min(scores))
print(f"\n📐 Normalized range: {normalized.min():.2f} to {normalized.max():.2f}")
```

---

## 🧠 Check Your Understanding

1. What is the main advantage of NumPy arrays over Python lists?
2. What does `np.linspace(0, 10, 5)` produce?
3. If `a = np.array([1, 2, 3])` and `b = np.array([4, 5, 6])`, what is `a * b`?
4. What does boolean indexing do? Give an example.
5. What is the shape of `np.zeros((3, 4))`?

---

## 📌 Key Takeaways

- NumPy arrays are **faster** and more **powerful** than Python lists for math
- Operations apply **element-wise** without needing loops
- `linspace` creates evenly spaced values; `arange` is like `range()`
- **Boolean indexing** filters arrays based on conditions
- NumPy is the foundation of all Python data science

---

*Next: [Pandas — Working with Data →](03-pandas.md)*
