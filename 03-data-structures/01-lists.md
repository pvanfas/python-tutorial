# Chapter 15: Lists — Ordered Collections

> A list is Python's all-purpose container. If variables are boxes, lists are shelves.

---

## 🎯 What You'll Learn

- Creating and accessing lists
- Adding, removing, and modifying items
- List methods (sort, reverse, count, etc.)
- Looping through lists
- List slicing

---

## 📋 Creating Lists

```python
# A list of numbers
scores = [85, 92, 78, 95, 88]

# A list of strings
fruits = ["apple", "mango", "banana", "grapes"]

# A mixed list (allowed in Python, but not common)
student = ["Arjun", 22, "Kochi", True]

# An empty list
empty = []

print(type(scores))   # → <class 'list'>
print(len(scores))    # → 5 (number of items)
```

---

## 🔍 Accessing Items (Indexing)

Python uses **zero-based indexing** — the first item is at position 0.

```python
fruits = ["apple", "mango", "banana", "grapes"]
#  index:   0         1         2         3

print(fruits[0])   # → apple  (first)
print(fruits[2])   # → banana (third)
print(fruits[-1])  # → grapes (last)
print(fruits[-2])  # → banana (second from last)
```

---

## ✂️ Slicing

Extract a portion of a list:

```python
scores = [85, 92, 78, 95, 88, 76, 90]

print(scores[1:4])    # → [92, 78, 95]  (index 1,2,3)
print(scores[:3])     # → [85, 92, 78]  (first 3)
print(scores[4:])     # → [88, 76, 90]  (from index 4 onward)
print(scores[::2])    # → [85, 78, 88, 90]  (every other)
print(scores[::-1])   # → [90, 76, 88, 95, 78, 92, 85]  (reversed)
```

---

## ➕ Adding Items

```python
fruits = ["apple", "mango"]

fruits.append("banana")       # Add to end
print(fruits)                  # → ['apple', 'mango', 'banana']

fruits.insert(1, "grapes")    # Insert at index 1
print(fruits)                  # → ['apple', 'grapes', 'mango', 'banana']

more = ["kiwi", "pineapple"]
fruits.extend(more)           # Add all items from another list
```

---

## ❌ Removing Items

```python
fruits = ["apple", "mango", "banana", "mango"]

fruits.remove("mango")    # Removes FIRST occurrence
print(fruits)              # → ['apple', 'banana', 'mango']

last = fruits.pop()        # Removes and returns last item
print(last)                # → mango

second = fruits.pop(0)     # Removes and returns item at index 0
print(second)              # → apple

del fruits[0]              # Delete by index (no return)

fruits.clear()             # Remove everything
```

---

## 🔧 Useful List Methods

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3]

print(numbers.count(1))    # → 2  (how many times 1 appears)
print(numbers.index(5))    # → 4  (first position of 5)

numbers.sort()              # Sort in place (ascending)
print(numbers)              # → [1, 1, 2, 3, 3, 4, 5, 5, 6, 9]

numbers.reverse()           # Reverse in place
print(numbers)              # → [9, 6, 5, 5, 4, 3, 3, 2, 1, 1]

# Non-destructive sorted:
original = [3, 1, 4, 1, 5]
new_sorted = sorted(original)   # Returns new list, doesn't change original
```

---

## 🔁 Looping Through Lists

```python
students = ["Arjun", "Priya", "Mohammed", "Sara"]

# Basic loop
for student in students:
    print(student)

# With index using enumerate
for i, student in enumerate(students):
    print(f"{i+1}. {student}")

# Build a new list with results
scores = [78, 92, 65, 88]
passed = [s for s in scores if s >= 70]   # List comprehension (Ch. 38)
print(passed)   # → [78, 92, 88]
```

---

## 🔍 Checking Membership

```python
fruits = ["apple", "mango", "banana"]

print("mango" in fruits)     # → True
print("kiwi" in fruits)      # → False
print("kiwi" not in fruits)  # → True
```

---

## 🧩 Nested Lists (Lists of Lists)

```python
# A 3x3 grid (like a matrix or tic-tac-toe board)
board = [
    [" ", " ", " "],
    [" ", " ", " "],
    [" ", " ", " "]
]

board[0][0] = "X"
board[1][1] = "O"
board[2][2] = "X"

for row in board:
    print("|".join(row))
```

Output:
```
X| |
 |O|
 | |X
```

---

## 🧠 Check Your Understanding

1. What does `fruits[-1]` return?
2. What is the difference between `append()` and `extend()`?
3. What does `list.sort()` do vs `sorted(list)`?
4. How do you check if an item is in a list?
5. What does `scores[1:4]` return if `scores = [10, 20, 30, 40, 50]`?

---

## 🏋️ Activity: Student Grade Manager (Part 1)

```python
# Store student names and scores
names = []
scores = []

print("📚 Student Grade Entry System")
print("Enter student data. Type 'done' when finished.\n")

while True:
    name = input("Student name (or 'done'): ")
    if name.lower() == "done":
        break

    try:
        score = float(input(f"Score for {name}: "))
        names.append(name)
        scores.append(score)
        print(f"  ✅ {name} added.\n")
    except ValueError:
        print("  ❌ Invalid score. Try again.\n")

# Summary
if names:
    print("\n📊 Class Summary")
    print("=" * 35)
    for name, score in zip(names, scores):
        grade = "A" if score >= 90 else "B" if score >= 75 else "C" if score >= 60 else "F"
        print(f"  {name:<15} {score:.1f}  ({grade})")
    print("-" * 35)
    print(f"  Class Average: {sum(scores)/len(scores):.2f}")
    print(f"  Highest:       {max(scores):.1f} — {names[scores.index(max(scores))]}")
    print(f"  Lowest:        {min(scores):.1f} — {names[scores.index(min(scores))]}")
```

---

## 📌 Key Takeaways

- Lists are **ordered**, **mutable** collections
- Access items with `list[index]` (0-based)
- Negative indices count from the end: `list[-1]` is the last item
- Key methods: `append`, `insert`, `remove`, `pop`, `sort`, `reverse`
- Use `in` to check membership
- Slice with `list[start:stop:step]`

---

*Next: [Tuples →](02-tuples.md)*
