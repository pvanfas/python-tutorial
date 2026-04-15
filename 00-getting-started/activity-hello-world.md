# 🏋️ Activity: Hello, World! and Beyond

> Your first hands-on Python session. Complete all parts before moving to Chapter 6.

---

## 🎯 Goals

- Run Python code from a file
- Practice using `print()` in creative ways
- Get comfortable with your editor
- Make your first small Python program

---

## ⏱️ Estimated Time: 30–45 minutes

---

## Part 1: The Classic (5 min)

Create a new file called `activity1.py` and run each of these one by one:

```python
# 1. The classic
print("Hello, World!")

# 2. Your name
print("My name is ___")

# 3. Python does math
print(100 - 37)
print(12 * 12)
print(7 ** 3)     # 7 to the power of 3

# 4. String repetition
print("Python! " * 5)
print("-" * 30)
```

**What to notice:** Python can do math inside `print()` and repeat strings with `*`.

---

## Part 2: Build a Name Card (10 min)

Create `name_card.py` and build a text-based name card:

```python
# My Name Card
print()
print("╔══════════════════════════════╗")
print("║                              ║")
print("║  Name:   [Your Name Here]    ║")
print("║  City:   [Your City]         ║")
print("║  Field:  [What You Study]    ║")
print("║  Goal:   Learn Python!       ║")
print("║                              ║")
print("╚══════════════════════════════╝")
print()
```

> 💡 Copy-paste the box characters: `╔ ╗ ╚ ╝ ║ ═`

---

## Part 3: Python as a Calculator (10 min)

Create `calculator.py`:

```python
# Use Python to solve these:

# How old will you be in 2040?
birth_year = ____   # fill in your birth year
print("In 2040 I will be:", 2040 - birth_year)

# How many seconds in a day?
seconds_per_minute = 60
minutes_per_hour = 60
hours_per_day = 24
print("Seconds in a day:", seconds_per_minute * minutes_per_hour * hours_per_day)

# Speed = Distance / Time
# If you travel 120 km in 2.5 hours, what's your speed?
distance = 120
time = 2.5
speed = distance / time
print(f"Speed: {speed} km/h")
```

---

## Part 4: The Story Generator (15 min)

Create `story.py`. Fill in the blanks to generate a funny story:

```python
# Mad-Libs style story generator (without input yet)
name = "___"
animal = "___"
number = ___
city = "___"
food = "___"
adjective = "___"

print()
print("=" * 50)
print("          THE GREAT ADVENTURE")
print("=" * 50)
print()
print(f"{name} woke up one morning in {city}.")
print(f"It was a {adjective} day, perfect for an adventure.")
print(f"On the street, {name} spotted a {animal}!")
print(f"The {animal} was carrying exactly {number} {food}s.")
print(f"Together they walked {number * 2} km to find treasure.")
print()
print(f"The end. {name} and the {animal} lived happily ever after.")
print()
print("=" * 50)
```

---

## ✅ Checklist

Before moving on, make sure you can:

- [ ] Create a new `.py` file in VS Code
- [ ] Run a Python file (click ▶️ or use terminal)
- [ ] Use `print()` to display text and numbers
- [ ] Do basic arithmetic with Python
- [ ] Use `*` to repeat strings
- [ ] Understand what a comment (`#`) does

---

## 🌟 Bonus Challenge

Can you print a Python logo made of characters?

```
    ____        _   _
   |  _ \ _   _| |_| |__   ___  _ __
   | |_) | | | | __| '_ \ / _ \| '_ \
   |  __/| |_| | |_| | | | (_) | | | |
   |_|    \__, |\__|_| |_|\___/|_| |_|
           |___/
```

Or try printing a simple pattern like:

```
*
**
***
****
*****
****
***
**
*
```

(Hint: You'll need multiple `print()` statements or to think ahead to loops!)

---

*Next Chapter: [Variables & Data Types →](../01-python-fundamentals/01-variables-and-types.md)*
