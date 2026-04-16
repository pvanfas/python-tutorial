# Chapter 38: List Comprehensions

> List comprehensions are Python's most elegant feature. One readable line replaces four.

---

## 🎯 What You'll Learn

- List comprehension syntax
- Filtering with conditions
- Nested comprehensions
- Dict and set comprehensions
- When to use them (and when not to)

---

## 🔄 From Loop to Comprehension

**Traditional loop:**
```python
squares = []
for x in range(1, 11):
    squares.append(x ** 2)
```

**List comprehension:**
```python
squares = [x ** 2 for x in range(1, 11)]
```

Both produce `[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]` — the comprehension is shorter and more Pythonic.

---

## 📐 Syntax

```
[expression   for item in iterable   if condition]
 ↑ what to    ↑ the loop             ↑ optional filter
   put in list
```

---

## 🔍 With Filtering

```python
# Only even numbers
evens = [x for x in range(20) if x % 2 == 0]
# → [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Passing students (score >= 60)
scores = [78, 92, 45, 88, 55, 95, 62, 41, 73]
passed = [s for s in scores if s >= 60]
# → [78, 92, 88, 95, 62, 73]

# Long words only
words = "the quick brown fox jumps over the lazy dog".split()
long_words = [w for w in words if len(w) > 3]
# → ['quick', 'brown', 'jumps', 'over', 'lazy']
```

---

## 🔀 Transforming Values

```python
# Uppercase all strings
names = ["arjun", "priya", "mohammed"]
upper = [name.title() for name in names]
# → ['Arjun', 'Priya', 'Mohammed']

# Celsius to Fahrenheit
celsius = [0, 20, 37, 100]
fahrenheit = [(c * 9/5) + 32 for c in celsius]

# Extract field from list of dicts
students = [
    {"name": "Arjun", "score": 85},
    {"name": "Priya", "score": 92},
]
names  = [s["name"]  for s in students]
scores = [s["score"] for s in students]

# Conditional expression (if/else inside comprehension)
grades = ["Pass" if s >= 60 else "Fail" for s in scores]
```

---

## 🪆 Nested List Comprehensions

```python
# 3x3 multiplication table as nested list
table = [[i * j for j in range(1, 4)] for i in range(1, 4)]
# → [[1, 2, 3], [2, 4, 6], [3, 6, 9]]

# Flatten a nested list
nested = [[1, 2, 3], [4, 5], [6, 7, 8, 9]]
flat = [item for sublist in nested for item in sublist]
# → [1, 2, 3, 4, 5, 6, 7, 8, 9]

# All pairs where a != b
pairs = [(a, b) for a in [1,2,3] for b in [1,2,3] if a != b]
```

---

## 📖 Dict & Set Comprehensions

```python
# Dict comprehension: {key: value for ...}
squares_dict = {x: x**2 for x in range(1, 6)}
# → {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Invert a dictionary
original = {"a": 1, "b": 2, "c": 3}
inverted = {v: k for k, v in original.items()}
# → {1: 'a', 2: 'b', 3: 'c'}

# Filter dict entries
students = {"Arjun": 85, "Priya": 92, "Mohammed": 45, "Sara": 78}
toppers = {name: score for name, score in students.items() if score >= 80}
# → {'Arjun': 85, 'Priya': 92, 'Sara': 78}

# Set comprehension: {expr for ...}
unique_lengths = {len(word) for word in "the quick brown fox".split()}
# → {3, 5}
```

---

## ⚡ Generator Expressions — Memory-Efficient

When you don't need the whole list at once, use `()` instead of `[]`:

```python
# List comprehension — creates all values in memory
total = sum([x**2 for x in range(1_000_000)])   # Creates million-item list

# Generator expression — computes values on the fly
total = sum(x**2 for x in range(1_000_000))     # No list created! Much better!

# Check if any score passed
any_passed = any(s >= 60 for s in scores)

# Check if all scores passed
all_passed = all(s >= 60 for s in scores)
```

---

## ⚠️ When NOT to Use Comprehensions

Comprehensions can hurt readability if too complex:

```python
# ❌ Too complex — hard to read
result = [transform(item) for sublist in data
          for item in sublist if condition1(item) and condition2(item)]

# ✅ Better as a regular loop with clear variable names
result = []
for sublist in data:
    for item in sublist:
        if condition1(item) and condition2(item):
            result.append(transform(item))
```

Rule of thumb: if it doesn't fit on one readable line, use a loop.

---

## 🧠 Check Your Understanding

1. Rewrite this as a comprehension: `[x for x in range(10) if x % 2 == 0]` — what does it produce?
2. Write a dict comprehension that maps each word in a sentence to its length.
3. What is the difference between `[...]` and `(...)` in a comprehension?
4. When should you choose a generator expression over a list comprehension?

---

## 📌 Key Takeaways

- Syntax: `[expression for item in iterable if condition]`
- Works for lists `[]`, dicts `{}`, sets `{}`, and generators `()`
- Use for simple transformations and filters — loops for complex logic
- Generator expressions `()` are memory-efficient for large data
- `any()` and `all()` pair perfectly with generator expressions

---

*Next: [Generators & Iterators →](02-generators.md)*
