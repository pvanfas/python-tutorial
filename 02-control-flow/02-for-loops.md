# Chapter 12: For Loops — Doing Things Repeatedly

> If you find yourself copy-pasting code more than twice, you need a loop.

---

## 🎯 What You'll Learn

- How `for` loops work
- Looping through lists, strings, and ranges
- The `range()` function
- `enumerate()` and `zip()`
- Nested loops

---

## 🔄 What is a Loop?

Without loops:
```python
print("Student 1")
print("Student 2")
print("Student 3")
print("Student 4")
print("Student 5")
# ... 45 more lines for a class of 50?!
```

With a loop:
```python
for i in range(1, 51):
    print(f"Student {i}")
```

Loops let you **repeat code** for each item in a collection, or a set number of times.

---

## 📋 Looping Through a List

The most natural use of a `for` loop:

```python
fruits = ["apple", "mango", "banana", "grapes"]

for fruit in fruits:
    print(fruit)
```

Output:
```
apple
mango
banana
grapes
```

**How it works:**
- Python takes each item from `fruits` one at a time
- Puts it in the variable `fruit`
- Runs the indented code for each item

---

## 🔤 Looping Through a String

Strings are sequences of characters:

```python
for letter in "Python":
    print(letter)
```

Output:
```
P
y
t
h
o
n
```

---

## 🔢 The `range()` Function

`range()` generates a sequence of numbers:

```python
# range(stop) — from 0 to stop-1
for i in range(5):
    print(i)     # 0, 1, 2, 3, 4

# range(start, stop)
for i in range(1, 6):
    print(i)     # 1, 2, 3, 4, 5

# range(start, stop, step)
for i in range(0, 20, 5):
    print(i)     # 0, 5, 10, 15

# Count backwards
for i in range(10, 0, -1):
    print(i)     # 10, 9, 8, ... 1
```

---

## 📊 Doing Math in Loops — Accumulating

```python
# Sum of numbers 1 to 100
total = 0
for i in range(1, 101):
    total = total + i
    # or: total += i

print(f"Sum of 1 to 100 = {total}")   # → 5050
```

> 💡 **For Physics students:** This is Σ (summation). Loops let you compute series, integrals (numerically), and many physics formulas iteratively.

---

## 🔢 `enumerate()` — Loop with Index

When you need both the item AND its position:

```python
students = ["Arjun", "Priya", "Mohammed", "Sara"]

for index, name in enumerate(students):
    print(f"{index + 1}. {name}")
```

Output:
```
1. Arjun
2. Priya
3. Mohammed
4. Sara
```

Without `enumerate()`, you'd have to manually track the index. This is cleaner!

---

## 🔗 `zip()` — Loop Through Two Lists Together

```python
names = ["Arjun", "Priya", "Mohammed"]
scores = [85, 92, 78]

for name, score in zip(names, scores):
    print(f"{name}: {score}/100")
```

Output:
```
Arjun: 85/100
Priya: 92/100
Mohammed: 78/100
```

---

## 🪆 Nested Loops — Loops Inside Loops

```python
# Multiplication table
for i in range(1, 4):       # Outer loop: rows
    for j in range(1, 4):   # Inner loop: columns
        print(i * j, end="\t")
    print()  # New line after each row
```

Output:
```
1    2    3
2    4    6
3    6    9
```

> ⚠️ Be careful with nested loops — they can be slow for large datasets since they multiply the iterations.

---

## 🏋️ Practical Example: Temperature Converter

```python
celsius_temps = [0, 20, 37, 100]

print("Celsius → Fahrenheit Conversion")
print("-" * 30)

for celsius in celsius_temps:
    fahrenheit = (celsius * 9/5) + 32
    print(f"  {celsius}°C = {fahrenheit}°F")
```

Output:
```
Celsius → Fahrenheit Conversion
------------------------------
  0°C = 32.0°F
  20°C = 68.0°F
  37°C = 98.6°F
  100°C = 212.0°F
```

---

## 🧠 Check Your Understanding

1. What does `range(1, 10)` generate?
2. What is the difference between `range(5)` and `range(1, 5)`?
3. Write a loop that prints every even number from 2 to 20.
4. What does `enumerate()` add to a normal `for` loop?
5. How many times does the inner code run in a nested loop with `range(3)` inside `range(3)`?

---

## 🏋️ Activity: Star Patterns & Statistics

**Part 1: Print a staircase pattern**
```python
# Expected output:
# *
# **
# ***
# ****
# *****

for i in range(1, 6):
    print("*" * i)
```

**Part 2: Compute class statistics**
```python
scores = [78, 92, 85, 60, 95, 88, 72, 81, 90, 69]

total = 0
highest = scores[0]
lowest = scores[0]

for score in scores:
    total += score
    if score > highest:
        highest = score
    if score < lowest:
        lowest = score

average = total / len(scores)

print(f"Total students: {len(scores)}")
print(f"Average score:  {average:.2f}")
print(f"Highest score:  {highest}")
print(f"Lowest score:   {lowest}")
```

**Challenge:** Count how many students scored above the average.

---

## 📌 Key Takeaways

- `for` loops repeat code for each item in a sequence
- `range(start, stop, step)` generates number sequences
- `enumerate()` gives you both index and value
- `zip()` lets you loop through two lists in parallel
- Nested loops multiply iterations — use with care

---

*Next: [While Loops →](03-while-loops.md)*
