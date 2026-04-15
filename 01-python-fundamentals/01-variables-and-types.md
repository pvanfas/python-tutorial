# Chapter 6: Variables & Data Types

> A variable is a box with a label. You put something in the box, label it, and retrieve it later by the label.

---

## 🎯 What You'll Learn

- What variables are and how to create them
- Python's core data types
- How to check the type of a variable
- Type conversion — changing one type to another
- Python's naming rules

---

## 📦 What is a Variable?

A variable stores a piece of data so you can use it later.

```python
name = "Arjun"
age = 22
height = 5.9
is_student = True
```

Breaking this down:
- `name` — the **variable name** (the label on the box)
- `=` — the **assignment operator** (puts data into the box)
- `"Arjun"` — the **value** (what's inside the box)

Now you can use the variable later:

```python
print(name)       # → Arjun
print(age + 1)    # → 23
print("Hello,", name)  # → Hello, Arjun
```

> 💡 **Python is dynamic:** You don't need to declare the type. Python figures it out automatically.

---

## 🧩 The Four Core Data Types

### 1. `int` — Integer (Whole Numbers)

```python
score = 100
temperature = -5
population = 1_400_000_000   # Underscores for readability

print(type(score))   # → <class 'int'>
```

Operations: `+`, `-`, `*`, `/`, `//` (floor division), `%` (remainder), `**` (power)

```python
print(10 // 3)   # → 3  (floor division, drops decimal)
print(10 % 3)    # → 1  (remainder)
print(2 ** 10)   # → 1024 (2 to the power of 10)
```

---

### 2. `float` — Floating Point (Decimal Numbers)

```python
pi = 3.14159
gravity = 9.81
price = 49.99

print(type(pi))  # → <class 'float'>
```

> 💡 **For Physics students:** Python floats are 64-bit IEEE 754 double precision. You'll encounter floating-point precision quirks:
> ```python
> print(0.1 + 0.2)  # → 0.30000000000000004 (not exactly 0.3!)
> ```
> For high-precision math, use the `decimal` module.

---

### 3. `str` — String (Text)

Strings are sequences of characters. You can use single or double quotes:

```python
first_name = "Priya"
last_name = 'Kumar'
city = "Kozhikode"

# Multi-line string
bio = """
My name is Priya.
I am from Kerala.
I love Python.
"""
```

**String operations:**

```python
full_name = first_name + " " + last_name   # Concatenation → "Priya Kumar"
greeting = "Ha" * 3                         # Repetition → "HaHaHa"

print(len(full_name))      # → 11  (length)
print(full_name.upper())   # → "PRIYA KUMAR"
print(full_name.lower())   # → "priya kumar"
print(full_name[0])        # → "P"  (first character)
```

**f-strings — the modern way to format strings:**

```python
name = "Arjun"
age = 22
message = f"My name is {name} and I am {age} years old."
print(message)  # → My name is Arjun and I am 22 years old.

# You can do math inside f-strings
print(f"Next year I will be {age + 1}")  # → Next year I will be 23
```

---

### 4. `bool` — Boolean (True/False)

```python
is_raining = True
is_logged_in = False
has_passed = True

print(type(is_raining))  # → <class 'bool'>
```

Booleans come from comparisons:

```python
print(5 > 3)    # → True
print(5 < 3)    # → False
print(5 == 5)   # → True
print(5 != 3)   # → True
```

> ⚠️ **Common mistake:** `=` assigns a value. `==` compares two values.
> ```python
> x = 5      # Assignment (put 5 in x)
> x == 5     # Comparison (is x equal to 5?)
> ```

---

## 🔄 Type Conversion

You can convert between types:

```python
# int to float
x = int(3.9)     # → 3  (truncates, doesn't round)

# float to int
y = float(5)     # → 5.0

# int/float to string
age_str = str(22)   # → "22"

# string to int (only if the string is a number)
num = int("42")  # → 42
num = int("hello")  # ❌ ValueError!

# Check if a string is numeric
print("42".isnumeric())   # → True
print("hello".isnumeric()) # → False
```

**Why does this matter?**

```python
# User input is ALWAYS a string!
age = input("Enter your age: ")
print(age + 1)    # ❌ TypeError: can't add int to string
print(int(age) + 1)  # ✅ Works!
```

---

## 🏷️ Variable Naming Rules

**Allowed:**
```python
my_name = "Ali"           # ✅ snake_case (recommended)
studentAge = 20           # ✅ camelCase (works but not Pythonic)
_private = "hidden"       # ✅ starts with underscore
score2 = 100              # ✅ can have numbers (not at start)
```

**Not allowed:**
```python
2score = 100        # ❌ Cannot start with a number
my-name = "Ali"    # ❌ Hyphen not allowed
class = "Python"   # ❌ 'class' is a reserved keyword
```

**Python naming conventions (PEP 8):**
- Use `snake_case` for variables: `first_name`, `total_score`
- Use CAPITALS for constants: `MAX_SIZE = 100`, `PI = 3.14`
- Variable names should be descriptive: `user_age` not just `a`

---

## 🧠 Check Your Understanding

1. What is a variable? What does the `=` sign do in Python?
2. What is the difference between `int` and `float`?
3. What does this produce? `print("Python" * 3)`
4. Why does `print("5" + 5)` give an error? How do you fix it?
5. What is an f-string? Write one that prints your name and age.

---

## 🏋️ Activity: Personal Profile Card

Write a program that stores information about you and prints a formatted profile card:

```python
# Store your information
name = "___"
age = ___
city = "___"
field_of_study = "___"
hobby = "___"
gpa = ___   # make up a float

# Print a profile card
print("=" * 40)
print("         PROFILE CARD")
print("=" * 40)
print(f"  Name:          {name}")
print(f"  Age:           {age}")
print(f"  City:          {city}")
print(f"  Field:         {field_of_study}")
print(f"  Hobby:         {hobby}")
print(f"  GPA:           {gpa:.2f}")   # 2 decimal places
print("=" * 40)
print(f"  In 5 years, {name} will be {age + 5}.")
print("=" * 40)
```

**Bonus:** Add a line that prints how many characters are in your name using `len()`.

---

## 📌 Key Takeaways

- Variables store data using the format: `variable_name = value`
- Four core types: `int`, `float`, `str`, `bool`
- Use `type()` to check a variable's type
- `=` assigns; `==` compares
- User input from `input()` is always a **string**
- Use f-strings for clean, readable string formatting

---

*Next: [Working with Strings →](02-strings.md)*
