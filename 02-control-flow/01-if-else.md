# Chapter 11: If / Elif / Else — Making Decisions

> Code that always does the same thing is a calculator. Code that makes decisions is a program.

---

## 🎯 What You'll Learn

- How to make programs that react to conditions
- `if`, `elif`, and `else` statements
- Comparison operators
- Logical operators (`and`, `or`, `not`)
- Nested conditions

---

## 🤔 What is a Condition?

A condition is a question that has a **yes (True)** or **no (False)** answer.

Real life examples:
- *Is it raining?* → If yes, take umbrella
- *Is my score ≥ 50?* → If yes, I passed
- *Is the user logged in?* → If no, redirect to login page

In Python, conditions use **comparison operators:**

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `==` | Equal to | `5 == 5` | `True` |
| `!=` | Not equal to | `5 != 3` | `True` |
| `>` | Greater than | `10 > 7` | `True` |
| `<` | Less than | `3 < 1` | `False` |
| `>=` | Greater or equal | `5 >= 5` | `True` |
| `<=` | Less or equal | `4 <= 3` | `False` |

---

## 🔀 The `if` Statement

```python
if condition:
    # This code runs ONLY if condition is True
    do_something()
```

Example:

```python
temperature = 38

if temperature > 37.5:
    print("You have a fever!")
    print("Please rest and drink water.")
```

> ⚠️ **The colon `:` and indentation are mandatory!**  
> Python uses **4 spaces** of indentation to define a block of code.

---

## ↔️ Adding `else`

```python
if condition:
    # Runs if condition is True
else:
    # Runs if condition is False
```

```python
age = 16

if age >= 18:
    print("You can vote.")
else:
    print("You cannot vote yet.")
    print(f"You need to wait {18 - age} more years.")
```

---

## 🌿 Multiple Conditions with `elif`

`elif` = "else if" — check another condition if the previous one was False.

```python
score = 75

if score >= 90:
    print("Grade: A")
elif score >= 75:
    print("Grade: B")
elif score >= 60:
    print("Grade: C")
elif score >= 45:
    print("Grade: D")
else:
    print("Grade: F — Please reappear")
```

> 💡 Python checks conditions **top to bottom** and stops at the first `True` one.

---

## 🔗 Logical Operators: and, or, not

Combine multiple conditions:

### `and` — Both must be True

```python
age = 20
has_id = True

if age >= 18 and has_id:
    print("Entry allowed")
else:
    print("Cannot enter")
```

### `or` — At least one must be True

```python
is_weekend = True
is_holiday = False

if is_weekend or is_holiday:
    print("No school today! 🎉")
else:
    print("School day 📚")
```

### `not` — Reverses the condition

```python
is_logged_in = False

if not is_logged_in:
    print("Please log in first")
```

---

## 🎯 Real-World Example: BMI Calculator

```python
# BMI = weight (kg) / height (m)²
weight = float(input("Enter your weight in kg: "))
height = float(input("Enter your height in meters: "))

bmi = weight / (height ** 2)
bmi = round(bmi, 2)   # Round to 2 decimal places

print(f"\nYour BMI: {bmi}")

if bmi < 18.5:
    print("Category: Underweight")
elif bmi < 25.0:
    print("Category: Normal weight ✅")
elif bmi < 30.0:
    print("Category: Overweight")
else:
    print("Category: Obese")

print("\nNote: BMI is a rough indicator. Consult a doctor for health advice.")
```

---

## 🪆 Nested `if` Statements

You can put `if` inside `if`:

```python
is_member = True
age = 15

if is_member:
    if age >= 18:
        print("Welcome, adult member!")
    else:
        print("Welcome, junior member!")
else:
    print("Please create an account first.")
```

> 💡 Don't go too deep with nesting — more than 2-3 levels makes code hard to read. There's usually a better way.

---

## 🧠 Check Your Understanding

1. What does `elif` mean? When do you use it instead of `else`?
2. Write a condition that checks if a number is between 10 and 20 (inclusive).
3. What is the difference between `=` and `==`?
4. What does `not True` evaluate to?
5. Why is indentation important in Python `if` blocks?

---

## 🏋️ Activity: Number Guessing Game (Part 1)

Build a mini game where the program picks a number and the user guesses it **once**:

```python
import random

secret_number = random.randint(1, 10)  # Random number between 1 and 10
guess = int(input("Guess a number between 1 and 10: "))

if guess == secret_number:
    print("🎉 Amazing! You got it right!")
elif guess > secret_number:
    print(f"📉 Too high! The number was {secret_number}.")
else:
    print(f"📈 Too low! The number was {secret_number}.")
```

**In the next chapter (While Loops), we'll let the user keep guessing until they get it right!**

---

## 📌 Key Takeaways

- `if` runs a block of code when a condition is `True`
- `else` runs when all conditions are `False`
- `elif` checks another condition if the previous ones were `False`
- Combine conditions with `and`, `or`, `not`
- **Indentation** (4 spaces) defines what's inside an `if` block
- Python checks `if`/`elif` from **top to bottom**

---

*Next: [For Loops →](02-for-loops.md)*
