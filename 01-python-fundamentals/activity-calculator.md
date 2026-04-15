# 🏋️ Activity: Build a Simple Calculator

> You've learned variables, strings, numbers, and user input. Now build something real.

---

## 🎯 Project Goal

Build a **command-line calculator** that:
1. Asks the user for two numbers
2. Asks what operation they want (+, -, ×, ÷)
3. Shows the result
4. Handles errors gracefully (division by zero, non-numeric input)

---

## ⏱️ Estimated Time: 45–60 minutes

---

## Step 1: Basic Version (Start Here)

Create `calculator.py`:

```python
print("🧮 Simple Python Calculator")
print("-" * 30)

# Get inputs
num1 = float(input("Enter first number: "))
num2 = float(input("Enter second number: "))
operation = input("Enter operation (+, -, *, /): ")

# Perform calculation
if operation == "+":
    result = num1 + num2
elif operation == "-":
    result = num1 - num2
elif operation == "*":
    result = num1 * num2
elif operation == "/":
    result = num1 / num2
else:
    print("Invalid operation!")
    result = None

if result is not None:
    print(f"\nResult: {num1} {operation} {num2} = {result}")
```

**Test it:** Run the program and try a few calculations. Does it work?

---

## Step 2: Handle Division by Zero

What happens if the user divides by 0? Add a check:

```python
elif operation == "/":
    if num2 == 0:
        print("❌ Error: Cannot divide by zero!")
        result = None
    else:
        result = num1 / num2
```

---

## Step 3: Handle Invalid Number Input

What if the user types "hello" instead of a number? Use `try/except`:

```python
try:
    num1 = float(input("Enter first number: "))
    num2 = float(input("Enter second number: "))
except ValueError:
    print("❌ Please enter valid numbers!")
    exit()
```

---

## Step 4: Repeating Calculator (With While Loop)

Make it keep going until the user quits:

```python
print("🧮 Simple Python Calculator")
print("Type 'q' to quit.")
print("-" * 30)

while True:
    num1_input = input("\nFirst number (or 'q' to quit): ")
    if num1_input.lower() == 'q':
        print("Goodbye! 👋")
        break

    num2_input = input("Second number: ")
    operation = input("Operation (+, -, *, /, ** for power, % for remainder): ")

    try:
        num1 = float(num1_input)
        num2 = float(num2_input)
    except ValueError:
        print("❌ Invalid number. Try again.")
        continue

    result = None

    if operation == "+":
        result = num1 + num2
    elif operation == "-":
        result = num1 - num2
    elif operation == "*":
        result = num1 * num2
    elif operation == "/":
        if num2 == 0:
            print("❌ Cannot divide by zero!")
        else:
            result = num1 / num2
    elif operation == "**":
        result = num1 ** num2
    elif operation == "%":
        result = num1 % num2
    else:
        print(f"❌ Unknown operation: {operation}")

    if result is not None:
        # Clean up: show integer if result is whole number
        if result == int(result):
            result = int(result)
        print(f"✅ {num1} {operation} {num2} = {result}")
```

---

## Step 5: Final Polish — History Tracking

Add a history feature:

```python
history = []

# Inside the while loop, after getting a valid result:
if result is not None:
    entry = f"{num1} {operation} {num2} = {result}"
    history.append(entry)
    print(f"✅ {entry}")

# Add a 'h' option to show history:
if num1_input.lower() == 'h':
    if history:
        print("\n📋 Calculation History:")
        for i, item in enumerate(history, 1):
            print(f"  {i}. {item}")
    else:
        print("No calculations yet.")
    continue
```

---

## ✅ Self-Assessment Checklist

- [ ] Basic calculation works for all 4 operations
- [ ] Division by zero is handled gracefully
- [ ] Invalid input doesn't crash the program
- [ ] The loop continues until user quits
- [ ] History is tracked and displayable

---

## 🌟 Bonus Challenges

- Add square root: `import math` then `math.sqrt(num)`
- Add percentage calculation
- Save history to a file called `calc_history.txt`
- Format results to 2 decimal places where needed
- Add a scientific mode with sin, cos, tan

---

## 🔗 Push to GitHub

When you're happy with it:
```bash
git init
git add calculator.py
git commit -m "Add simple calculator with history"
```

(We'll learn GitHub properly in Chapter 57!)

---

*Next: [Control Flow — If/Elif/Else →](../02-control-flow/01-if-else.md)*
