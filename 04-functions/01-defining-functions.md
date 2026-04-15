# Chapter 20: Defining & Calling Functions

> A function is a reusable block of code with a name. Write once, use anywhere.

---

## 🎯 What You'll Learn

- What functions are and why they matter
- How to define and call your own functions
- Functions in everyday programming
- Built-in Python functions you've already been using

---

## 🤔 Why Functions?

Imagine you're building a program that calculates someone's age from their birth year — and you need this in 10 different places.

**Without functions:**
```python
# In one place:
age1 = 2024 - 2001
print(f"Ali is {age1} years old")

# In another place:
age2 = 2024 - 1998
print(f"Sara is {age2} years old")

# ...repeated 8 more times
```

**With a function:**
```python
def calculate_age(birth_year):
    age = 2024 - birth_year
    return age

print(f"Ali is {calculate_age(2001)} years old")
print(f"Sara is {calculate_age(1998)} years old")
# Easy to use everywhere!
```

Functions solve the **DRY principle: Don't Repeat Yourself**.

---

## ✏️ Defining a Function

```python
def function_name(parameters):
    """Optional: description of what this function does"""
    # Code that runs when function is called
    return result   # Optional
```

**Simple example:**

```python
def greet():
    print("Hello! Welcome to Python.")
    print("Let's learn together!")

# Calling the function:
greet()   # Output: Hello! Welcome to Python.
greet()   # You can call it as many times as you want!
```

---

## 📥 Parameters — Giving Functions Input

```python
def greet_person(name):
    print(f"Hello, {name}! Welcome!")

greet_person("Arjun")    # → Hello, Arjun! Welcome!
greet_person("Priya")    # → Hello, Priya! Welcome!
greet_person("Mohammed") # → Hello, Mohammed! Welcome!
```

**Multiple parameters:**

```python
def describe_student(name, age, city):
    print(f"{name} is {age} years old from {city}.")

describe_student("Sara", 20, "Kozhikode")
describe_student("Ravi", 22, "Kochi")
```

---

## 📤 Return Values — Getting Output from Functions

Functions can send back a result:

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)        # → 8
print(add(10, 20))  # → 30
```

**Physics example — Kinetic Energy:**
```python
def kinetic_energy(mass, velocity):
    """
    Calculate kinetic energy.
    KE = 0.5 * m * v²
    Returns energy in Joules.
    """
    ke = 0.5 * mass * (velocity ** 2)
    return ke

car_ke = kinetic_energy(1000, 20)   # 1000 kg, 20 m/s
print(f"Car's KE: {car_ke} J")     # → 200000 J

ball_ke = kinetic_energy(0.5, 30)   # 0.5 kg, 30 m/s
print(f"Ball's KE: {ball_ke} J")   # → 225 J
```

---

## 🏷️ Default Parameters

You can give parameters default values:

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Arjun")              # → Hello, Arjun!      (uses default)
greet("Priya", "Namaste")  # → Namaste, Priya!    (override)
greet("Sara", "Salam")     # → Salam, Sara!
```

---

## 📚 Functions You've Already Been Using

Python has hundreds of **built-in functions**:

```python
print("hello")           # Displays output
len("Python")            # → 6 (length)
int("42")                # → 42 (convert to int)
float("3.14")            # → 3.14 (convert to float)
str(100)                 # → "100" (convert to string)
type(42)                 # → <class 'int'>
range(1, 10)             # Generates numbers
input("Enter: ")         # Gets user input
abs(-5)                  # → 5 (absolute value)
max(3, 7, 2)             # → 7 (maximum)
min(3, 7, 2)             # → 2 (minimum)
round(3.14159, 2)        # → 3.14 (round to 2 decimal places)
sum([1, 2, 3, 4])        # → 10 (sum of list)
sorted([3, 1, 2])        # → [1, 2, 3] (sorted list)
```

---

## 📝 Docstrings — Documenting Your Functions

A **docstring** is a description of what your function does:

```python
def celsius_to_fahrenheit(celsius):
    """
    Convert temperature from Celsius to Fahrenheit.
    
    Args:
        celsius (float): Temperature in Celsius
    
    Returns:
        float: Temperature in Fahrenheit
    
    Example:
        >>> celsius_to_fahrenheit(100)
        212.0
    """
    return (celsius * 9/5) + 32

help(celsius_to_fahrenheit)  # Shows the docstring!
```

> 💡 Good docstrings make your code readable by others (and future you!).

---

## 🏋️ Activity: Build a Function Library

Create a file called `math_tools.py` with these functions:

```python
def circle_area(radius):
    """Calculate area of a circle. Area = π × r²"""
    PI = 3.14159
    return PI * (radius ** 2)

def circle_circumference(radius):
    """Calculate circumference. C = 2πr"""
    PI = 3.14159
    return 2 * PI * radius

def is_even(number):
    """Return True if number is even, False if odd."""
    return number % 2 == 0

def fahrenheit_to_celsius(f):
    """Convert Fahrenheit to Celsius."""
    return (f - 32) * 5/9

def simple_interest(principal, rate, time):
    """
    Calculate simple interest.
    SI = (P × R × T) / 100
    """
    return (principal * rate * time) / 100

# Test your functions:
print(f"Circle area (r=5): {circle_area(5):.2f}")
print(f"Is 42 even? {is_even(42)}")
print(f"100°F = {fahrenheit_to_celsius(100):.1f}°C")
print(f"SI on ₹10000 at 8% for 3 years: ₹{simple_interest(10000, 8, 3)}")
```

**Challenge:** Add a function `compound_interest(principal, rate, time, n)` where `n` is number of times interest compounds per year. Formula: `A = P(1 + r/n)^(nt)`

---

## 🧠 Check Your Understanding

1. What is the `def` keyword used for?
2. What is the difference between a **parameter** and an **argument**?
3. What does `return` do? What happens if a function has no `return`?
4. Write a function `is_positive(number)` that returns `True` if the number is positive.
5. Why are functions useful? Name two benefits.

---

## 📌 Key Takeaways

- Define functions with `def function_name(parameters):`
- Call functions by writing `function_name(arguments)`
- `return` sends a value back from a function
- Default parameters let you call functions with fewer arguments
- Docstrings document what your function does
- **DRY: Don't Repeat Yourself** — functions eliminate code duplication

---

*Next: [Parameters, Arguments & Defaults (deep dive) →](02-parameters.md)*
