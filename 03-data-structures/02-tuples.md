# Chapter 16: Tuples — Immutable Sequences

> A tuple is a list that promises never to change. Use it when the data shouldn't be modified.

---

## 🎯 What You'll Learn

- Creating and using tuples
- Why tuples exist (when to use them over lists)
- Tuple unpacking — one of Python's most elegant features
- Named tuples for readable code

---

## 📦 Creating Tuples

```python
# With parentheses
point = (3, 7)
rgb = (255, 128, 0)
person = ("Arjun", 22, "Kochi")

# Without parentheses (also valid)
coordinates = 10.5, 20.3

# Single-item tuple — note the trailing comma!
single = (42,)      # This is a tuple
not_tuple = (42)    # This is just the integer 42

print(type(single))     # → <class 'tuple'>
print(type(not_tuple))  # → <class 'int'>

# Empty tuple
empty = ()
```

---

## 🔍 Accessing Tuple Items

Exactly like lists:

```python
person = ("Arjun", 22, "Kochi")

print(person[0])    # → Arjun
print(person[-1])   # → Kochi
print(person[0:2])  # → ('Arjun', 22)
print(len(person))  # → 3
```

---

## 🔒 Immutability — Tuples Cannot Change

```python
colors = ("red", "green", "blue")

colors[0] = "yellow"   # ❌ TypeError: 'tuple' object does not support item assignment
colors.append("purple") # ❌ AttributeError: 'tuple' object has no attribute 'append'
```

This is intentional! Use tuples when:
- Data should not change (coordinates, RGB values, database records)
- You want to prevent accidental modification
- Using data as dictionary keys (lists can't be dict keys; tuples can)
- Returning multiple values from a function

---

## 🎯 Tuple Unpacking — Python's Secret Weapon

Unpacking assigns tuple elements to individual variables in one line:

```python
point = (3, 7)
x, y = point       # Unpack into two variables
print(f"x={x}, y={y}")   # → x=3, y=7

# With three values
person = ("Arjun", 22, "Kochi")
name, age, city = person
print(f"{name} is {age} from {city}")

# Swap variables without a temp variable!
a = 10
b = 20
a, b = b, a   # Tuple magic!
print(a, b)   # → 20 10

# Ignore values with _
first, _, third = (1, 2, 3)   # Ignore the middle value
print(first, third)   # → 1 3

# Extended unpacking with *
first, *rest = (1, 2, 3, 4, 5)
print(first)   # → 1
print(rest)    # → [2, 3, 4, 5]

*start, last = (1, 2, 3, 4, 5)
print(start)   # → [1, 2, 3, 4]
print(last)    # → 5
```

---

## 🔄 Returning Multiple Values from Functions

Functions can return tuples — and unpacking makes this clean:

```python
def min_max(numbers):
    """Return both min and max of a list."""
    return min(numbers), max(numbers)

scores = [78, 92, 65, 88, 95, 70]
low, high = min_max(scores)
print(f"Range: {low} to {high}")

def divide_with_remainder(a, b):
    return a // b, a % b

quotient, remainder = divide_with_remainder(17, 5)
print(f"17 ÷ 5 = {quotient} remainder {remainder}")
```

---

## 🏷️ Named Tuples — Readable Tuples

Regular tuple: `(3, 7)` — what does each number mean?
Named tuple: `Point(x=3, y=7)` — now it's obvious!

```python
from collections import namedtuple

# Define a named tuple type
Point = namedtuple("Point", ["x", "y"])
Student = namedtuple("Student", ["name", "age", "score"])
Color = namedtuple("Color", ["red", "green", "blue"])

# Create instances
p = Point(3, 7)
s = Student("Arjun", 22, 88.5)
c = Color(255, 128, 0)

# Access by name (readable!) OR by index
print(p.x, p.y)         # → 3 7
print(s.name, s.score)  # → Arjun 88.5
print(c.red)            # → 255

# Can still unpack like normal tuples
x, y = p
name, age, score = s
```

---

## 📊 Tuples in Practice

```python
# GPS coordinates (should never change)
KOCHI = (9.9312, 76.2673)
DELHI = (28.6139, 77.2090)

def distance_approx(loc1, lat2_lon2):
    lat1, lon1 = loc1
    lat2, lon2 = lat2_lon2
    return ((lat2-lat1)**2 + (lon2-lon1)**2) ** 0.5 * 111

print(f"Kochi to Delhi: ~{distance_approx(KOCHI, DELHI):.0f} km")

# Dictionary with tuple keys
distance_map = {
    ("Kochi", "Kozhikode"): 156,
    ("Kochi", "Thrissur"): 74,
    ("Kozhikode", "Kannur"): 90,
}
print(f"Kochi → Thrissur: {distance_map[('Kochi', 'Thrissur')]} km")
```

---

## 🧠 Check Your Understanding

1. What makes a tuple different from a list?
2. How do you create a tuple with just one item?
3. What does tuple unpacking allow you to do?
4. Why can tuples be used as dictionary keys but lists cannot?
5. What does `first, *rest = (1, 2, 3, 4, 5)` give you?

---

## 📌 Key Takeaways

- Tuples are **immutable** sequences — can't be changed after creation
- Created with `()` or just commas: `a, b = 1, 2`
- Use for data that shouldn't change: coordinates, config values, DB records
- **Tuple unpacking** is elegant: `x, y = point`
- Return multiple values from functions: `return result, error`
- Use `namedtuple` for readable, self-documenting tuples

---

*Next: [Dictionaries →](03-dictionaries.md)*
