# Chapter 18: Sets — Unique Collections

> A set is like a list that automatically removes duplicates and lets you do math on collections.

---

## 🎯 What You'll Learn

- Creating sets and adding items
- Set operations: union, intersection, difference
- When sets are the perfect tool
- Set comprehensions

---

## 🔵 Creating Sets

```python
# From a literal
fruits = {"apple", "mango", "banana", "apple"}  # Duplicate removed!
print(fruits)   # → {'mango', 'apple', 'banana'}  (order not guaranteed)

# From a list (removes duplicates!)
scores = [85, 92, 78, 85, 90, 92, 78]
unique_scores = set(scores)
print(unique_scores)   # → {78, 85, 90, 92}

# Empty set — MUST use set(), not {}
empty = set()       # ✅ Empty set
not_set = {}        # ❌ This is an empty dict!

print(type(empty))      # → <class 'set'>
print(type(not_set))    # → <class 'dict'>
```

---

## ⚙️ Key Properties of Sets

```python
s = {3, 1, 4, 1, 5, 9, 2, 6, 5}
print(s)         # → {1, 2, 3, 4, 5, 6, 9}  (no duplicates, unordered)
print(len(s))    # → 7

# Sets are UNORDERED — no indexing!
print(s[0])      # ❌ TypeError: 'set' object is not subscriptable

# But you can iterate
for item in s:
    print(item)

# Check membership — very fast!
print(5 in s)    # → True
print(7 in s)    # → False
```

---

## ➕ Modifying Sets

```python
languages = {"Python", "JavaScript", "Java"}

languages.add("Rust")           # Add one item
languages.update(["Go", "C"])   # Add multiple items

languages.remove("Java")        # Raises KeyError if not found
languages.discard("Cobol")      # Safe remove — no error if missing

popped = languages.pop()        # Removes and returns random item

languages.clear()               # Remove all items
```

---

## 🔢 Set Operations — The Real Power

Sets support **mathematical set operations**:

```python
python_students = {"Arjun", "Priya", "Sara", "Mohammed"}
java_students   = {"Priya", "Ravi", "Sara", "Deepa"}

# Union (|) — students in either or both
both = python_students | java_students
print(f"All students: {both}")

# Intersection (&) — students in BOTH
both_courses = python_students & java_students
print(f"Taking both: {both_courses}")   # → {'Priya', 'Sara'}

# Difference (-) — in Python but NOT Java
only_python = python_students - java_students
print(f"Only Python: {only_python}")    # → {'Arjun', 'Mohammed'}

# Symmetric Difference (^) — in one but NOT both
exclusive = python_students ^ java_students
print(f"Exclusive: {exclusive}")        # → {'Arjun', 'Mohammed', 'Ravi', 'Deepa'}

# Subset/Superset
small = {"Priya", "Sara"}
print(small.issubset(python_students))          # → True
print(python_students.issuperset(small))        # → True
print(python_students.isdisjoint({"Tom"}))      # → True (no common elements)
```

---

## 🧹 Most Common Use: Removing Duplicates

```python
# Quick deduplication
emails = ["a@x.com", "b@x.com", "a@x.com", "c@x.com", "b@x.com"]
unique_emails = list(set(emails))
print(f"{len(emails)} emails → {len(unique_emails)} unique")

# Find duplicate entries
tags = ["python", "tutorial", "python", "beginner", "tutorial", "python"]
seen = set()
duplicates = set()
for tag in tags:
    if tag in seen:
        duplicates.add(tag)
    seen.add(tag)
print(f"Duplicate tags: {duplicates}")
```

---

## ⚡ Sets vs Lists for Membership Testing

Sets are **dramatically faster** for `in` checks on large data:

```python
import time

data = list(range(1_000_000))
data_set = set(data)

# List search: O(n) — checks every element
start = time.time()
999999 in data
print(f"List: {time.time()-start:.6f}s")

# Set search: O(1) — constant time (hash lookup)
start = time.time()
999999 in data_set
print(f"Set:  {time.time()-start:.6f}s")

# Set is often 100-1000x faster for large collections!
```

---

## 📐 Set Comprehension

```python
# Squares of even numbers
evens_squared = {x**2 for x in range(1, 11) if x % 2 == 0}
print(evens_squared)   # → {4, 16, 36, 64, 100}

# Unique first letters
names = ["Arjun", "Priya", "Ahmed", "Preethi", "Arun"]
first_letters = {name[0] for name in names}
print(first_letters)   # → {'A', 'P'}
```

---

## 🧠 Check Your Understanding

1. What makes sets different from lists?
2. What does `{1, 2, 3} & {2, 3, 4}` return?
3. Why can't you access a set element by index?
4. What is `{}` in Python — a set or a dict?
5. When would you use a set instead of a list?

---

## 📌 Key Takeaways

- Sets contain **unique**, **unordered** items
- Create from a list to **remove duplicates**: `set(my_list)`
- Set operations: `|` union, `&` intersection, `-` difference, `^` symmetric diff
- `in` checks are **much faster** with sets than lists
- Use `set()` for empty set (not `{}` — that's a dict)

---

*Next: [When to Use Which Data Structure →](05-choosing-structure.md)*
