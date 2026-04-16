# Chapter 24: Lambda Functions

> A lambda is a tiny, anonymous function. One line, one expression, instant use.

---

## 🎯 What You'll Learn

- Lambda syntax
- When to use lambdas (and when not to)
- Lambdas with `sorted()`, `map()`, `filter()`

---

## ⚡ Lambda Syntax

```python
# Regular function
def square(x):
    return x ** 2

# Equivalent lambda
square = lambda x: x ** 2

print(square(5))   # → 25
```

General form: `lambda parameters: expression`

```python
add = lambda a, b: a + b
greet = lambda name: f"Hello, {name}!"
clamp = lambda x, lo, hi: max(lo, min(hi, x))

print(add(3, 4))           # → 7
print(greet("Arjun"))      # → Hello, Arjun!
print(clamp(150, 0, 100))  # → 100
```

> 💡 Lambdas can only contain a **single expression** — no loops, no multiple statements.

---

## 🔑 The Real Use: As Arguments to Other Functions

Lambdas shine when used as a quick function argument:

```python
students = [
    {"name": "Arjun",    "score": 85, "age": 22},
    {"name": "Priya",    "score": 92, "age": 20},
    {"name": "Mohammed", "score": 78, "age": 23},
    {"name": "Sara",     "score": 92, "age": 21},
]

# Sort by score
by_score = sorted(students, key=lambda s: s["score"], reverse=True)

# Sort by name
by_name = sorted(students, key=lambda s: s["name"])

# Sort by score first, then by name for ties
by_score_name = sorted(students, key=lambda s: (-s["score"], s["name"]))

for s in by_score_name:
    print(f"{s['name']:<15} {s['score']}")
```

---

## 🗺️ `map()` — Apply a Function to Every Item

```python
numbers = [1, 2, 3, 4, 5]

squared = list(map(lambda x: x**2, numbers))
print(squared)   # → [1, 4, 9, 16, 25]

names = ["arjun kumar", "priya nair", "mohammed ali"]
titled = list(map(lambda n: n.title(), names))
print(titled)   # → ['Arjun Kumar', 'Priya Nair', 'Mohammed Ali']
```

> 💡 List comprehensions are often more Pythonic than `map()`:
> `[x**2 for x in numbers]` vs `list(map(lambda x: x**2, numbers))`

---

## 🔍 `filter()` — Keep Items That Match a Condition

```python
scores = [78, 92, 45, 88, 55, 95, 62, 41]

passed = list(filter(lambda s: s >= 60, scores))
print(passed)   # → [78, 92, 88, 95, 62]

# Equivalent list comprehension (often preferred):
passed = [s for s in scores if s >= 60]
```

---

## ⚠️ When NOT to Use Lambda

If the lambda is complex, use a real function instead:

```python
# ❌ Hard to read
sorter = lambda s: (s["grade"], -s["score"], s["name"].lower())

# ✅ Much clearer
def sort_key(student):
    return (student["grade"], -student["score"], student["name"].lower())

students.sort(key=sort_key)
```

PEP 8 (Python's style guide) actually discourages assigning lambdas to variables. Use `def` for named functions, lambdas for quick, inline, throwaway functions.

---

## 📌 Key Takeaways

- Lambda syntax: `lambda params: expression`
- Single expression only — no loops or multiple lines
- Best used as inline argument to `sorted()`, `map()`, `filter()`
- For anything complex, use a regular `def` instead

---

*Next: [Recursion →](06-recursion.md)*
