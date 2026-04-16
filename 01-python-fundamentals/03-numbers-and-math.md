# Chapter 8: Numbers & Math in Python

> Python is a superb scientific calculator. And it doesn't lose your work.

---

## 🎯 What You'll Learn

- Integer and float arithmetic
- All arithmetic operators
- The `math` module
- Working with very large and very small numbers
- Rounding, floor, ceiling
- Random numbers

---

## 🔢 Integer Arithmetic

```python
a = 17
b = 5

print(a + b)    # → 22   Addition
print(a - b)    # → 12   Subtraction
print(a * b)    # → 85   Multiplication
print(a / b)    # → 3.4  Division (always returns float)
print(a // b)   # → 3    Floor division (drops decimal)
print(a % b)    # → 2    Modulo (remainder)
print(a ** b)   # → 1419857  Exponentiation (17^5)
```

> 💡 **Physics note:** `//` is integer division (like `div` in Pascal), and `%` gives the remainder. Useful for checking divisibility: `if n % 2 == 0` means n is even.

---

## 🌊 Float Arithmetic

```python
x = 3.14
y = 2.0

print(x + y)       # → 5.140000000000001  (floating point quirk!)
print(round(x, 1)) # → 3.1

# Python floats have limited precision (~15 decimal digits)
print(0.1 + 0.2)   # → 0.30000000000000004

# For high precision, use the Decimal module:
from decimal import Decimal
print(Decimal("0.1") + Decimal("0.2"))  # → 0.3  (exact)
```

---

## 🧮 Order of Operations (PEMDAS/BODMAS)

Python follows standard math precedence:

```python
# Parentheses first
print(2 + 3 * 4)       # → 14  (not 20)
print((2 + 3) * 4)     # → 20

# Full order: ** → * / // % → + -
print(2 ** 3 + 1)      # → 9   (8 + 1)
print(10 - 2 * 3 + 1)  # → 5   (10 - 6 + 1)
```

> ✅ **Good practice:** Use parentheses to make your intent clear, even when not required.

---

## 🔧 Built-in Math Functions

```python
print(abs(-7.5))       # → 7.5   (absolute value)
print(round(3.567, 2)) # → 3.57  (round to 2 decimal places)
print(round(3.5))      # → 4     (round to nearest integer)
print(max(3, 1, 7, 2)) # → 7
print(min(3, 1, 7, 2)) # → 1
print(sum([1, 2, 3, 4, 5]))  # → 15
print(pow(2, 10))      # → 1024  (same as 2**10)

import math
print(math.floor(3.9)) # → 3   (round DOWN)
print(math.ceil(3.1))  # → 4   (round UP)
print(math.trunc(3.9)) # → 3   (truncate — just drops decimal)
```

---

## 📐 The `math` Module

A treasure trove for scientific computing:

```python
import math

# Constants
print(math.pi)      # → 3.141592653589793
print(math.e)       # → 2.718281828459045
print(math.inf)     # → inf  (infinity)
print(math.tau)     # → 6.283185307179586  (2π)

# Trigonometry (all angles in radians)
print(math.sin(math.pi / 2))    # → 1.0
print(math.cos(0))              # → 1.0
print(math.tan(math.pi / 4))    # → 1.0
print(math.degrees(math.pi))    # → 180.0  (radians → degrees)
print(math.radians(180))        # → 3.14...  (degrees → radians)

# Powers and logarithms
print(math.sqrt(144))           # → 12.0
print(math.log(math.e))         # → 1.0   (natural log)
print(math.log10(1000))         # → 3.0   (log base 10)
print(math.log2(1024))          # → 10.0  (log base 2)
print(math.exp(1))              # → 2.718... (e^x)

# Other
print(math.factorial(5))        # → 120
print(math.gcd(48, 18))         # → 6   (greatest common divisor)
print(math.isnan(float('nan'))) # → True
print(math.isinf(math.inf))     # → True
```

---

## 🎲 Random Numbers

```python
import random

# Random float between 0.0 and 1.0
print(random.random())

# Random integer in range (inclusive)
dice = random.randint(1, 6)
print(f"Dice roll: {dice}")

# Random float in range
temp = random.uniform(20.0, 40.0)
print(f"Temperature: {temp:.1f}°C")

# Random choice from a list
colors = ["red", "green", "blue", "yellow"]
print(random.choice(colors))

# Shuffle a list in place
cards = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
random.shuffle(cards)
print(cards)

# Pick N unique items
sample = random.sample(range(1, 50), 6)  # 6 lottery numbers
print(sorted(sample))

# Reproducible randomness (for testing)
random.seed(42)   # Same seed = same sequence every time
```

---

## 🔬 Physics Examples

```python
import math

# Ohm's Law: V = IR
def ohms_law(voltage=None, current=None, resistance=None):
    if voltage is None:
        return current * resistance
    elif current is None:
        return voltage / resistance
    else:
        return voltage / current

print(f"V = {ohms_law(current=2.5, resistance=10)} V")
print(f"I = {ohms_law(voltage=12, resistance=4)} A")
print(f"R = {ohms_law(voltage=9, current=3)} Ω")

# Pendulum period: T = 2π√(L/g)
def pendulum_period(length_m):
    g = 9.81
    return 2 * math.pi * math.sqrt(length_m / g)

for length in [0.25, 0.5, 1.0, 2.0]:
    T = pendulum_period(length)
    print(f"L = {length:.2f} m  →  T = {T:.4f} s")

# Decibels: dB = 10 log₁₀(I/I₀)
def decibels(intensity, reference=1e-12):
    return 10 * math.log10(intensity / reference)

sounds = {"Whisper": 1e-10, "Conversation": 1e-6, "Concert": 1e-1}
for sound, intensity in sounds.items():
    print(f"{sound}: {decibels(intensity):.1f} dB")
```

---

## 💻 Number Formatting for Display

```python
pi = 3.14159265358979

print(f"{pi:.2f}")        # → 3.14      (2 decimal places)
print(f"{pi:.6f}")        # → 3.141593  (6 decimal places)
print(f"{pi:10.3f}")      # →      3.142 (width 10, 3 decimals)
print(f"{1234567:,}")     # → 1,234,567  (thousands separator)
print(f"{0.00134:.2e}")   # → 1.34e-03   (scientific notation)
print(f"{1234:.2E}")      # → 1.23E+03   (uppercase E)
print(f"{255:b}")         # → 11111111   (binary)
print(f"{255:x}")         # → ff         (hexadecimal)
print(f"{255:o}")         # → 377        (octal)
```

---

## 🧠 Check Your Understanding

1. What is the difference between `/` and `//`?
2. What does `17 % 5` return and why?
3. How do you convert degrees to radians in Python?
4. What is `math.floor(4.9)`? What about `round(4.9)`?
5. Why does `0.1 + 0.2` not equal exactly `0.3`?

---

## 🏋️ Mini Activity: Scientific Calculator

```python
import math

def scientific_calc():
    print("🔬 Scientific Calculator")
    print("Operators: +, -, *, /, //, %, **")
    print("Functions: sin, cos, tan, sqrt, log, log10, abs")
    print("Constants: pi, e")

    while True:
        expr = input("\nEnter expression (or 'quit'): ").strip()
        if expr == 'quit':
            break

        # Replace constants and functions
        expr = expr.replace("pi", str(math.pi))
        expr = expr.replace("e", str(math.e))
        expr = expr.replace("sin(", "math.sin(math.radians(")
        expr = expr.replace("cos(", "math.cos(math.radians(")
        expr = expr.replace("sqrt(", "math.sqrt(")
        expr = expr.replace("log10(", "math.log10(")
        expr = expr.replace("log(", "math.log(")
        expr = expr.replace("abs(", "abs(")

        # Fix closing parens for trig (added extra paren for radians)
        try:
            result = eval(expr)
            print(f"  = {result}")
        except Exception as ex:
            print(f"  ❌ Error: {ex}")

scientific_calc()
```

---

## 📌 Key Takeaways

- `//` is floor division; `%` is modulo (remainder); `**` is power
- Import `math` for `sqrt`, `sin`, `cos`, `log`, `pi`, `e`, and more
- Use `import random` for `randint`, `choice`, `shuffle`, `sample`
- Float arithmetic has tiny precision errors — use `Decimal` for exact math
- f-string format specs control decimal places: `{value:.2f}`

---

*Next: [Getting Input from Users →](04-user-input.md)*
