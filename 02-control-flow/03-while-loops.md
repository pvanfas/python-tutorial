# Chapter 13: While Loops — Keep Going Until Done

> A `for` loop says "do this N times." A `while` loop says "do this until I say stop."

---

## 🎯 What You'll Learn

- How `while` loops differ from `for` loops
- When to use `while` vs `for`
- Avoiding infinite loops
- Using `while` for input validation
- The `do-while` pattern in Python

---

## 🔄 The `while` Loop

```python
while condition:
    # Runs as long as condition is True
```

Example:

```python
count = 1

while count <= 5:
    print(f"Count: {count}")
    count += 1   # IMPORTANT: update the variable!

print("Done!")
```

Output:
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
Done!
```

---

## ⚡ `for` vs `while` — When to Use Which?

| Use `for` when... | Use `while` when... |
|---|---|
| You know how many times to repeat | You don't know how many times |
| Looping through a list/string | Waiting for a condition to change |
| Using `range()` | Getting valid input from user |

---

## ♾️ The Infinite Loop (and How to Avoid It)

This loop runs forever:

```python
while True:
    print("This never stops!")
```

An infinite loop happens when the condition **never becomes False**.

**Common mistake:**
```python
count = 1
while count <= 5:
    print(count)
    # Forgot count += 1 ← INFINITE LOOP!
```

**How to stop a running program:** Press `Ctrl + C` in the terminal.

> 💡 Intentional infinite loops (like game loops or server loops) use `break` to exit at the right moment.

---

## 🛑 Using `while` for Input Validation

This is one of the most practical uses of `while`:

```python
age = int(input("Enter your age: "))

while age < 0 or age > 120:
    print("❌ Invalid age. Please try again.")
    age = int(input("Enter your age: "))

print(f"✅ Age recorded: {age}")
```

The loop keeps asking until the user gives a valid answer.

---

## 🎮 Complete Guessing Game

Now let's build the full version of our guessing game from Chapter 11:

```python
import random

secret = random.randint(1, 100)
attempts = 0
max_attempts = 10

print("🎮 Number Guessing Game")
print(f"Guess a number between 1 and 100. You have {max_attempts} tries!")
print("-" * 40)

while attempts < max_attempts:
    guess = int(input(f"Attempt {attempts + 1}/{max_attempts}: "))
    attempts += 1

    if guess == secret:
        print(f"🎉 Correct! You guessed it in {attempts} attempts!")
        break
    elif guess < secret:
        print("📈 Too low!")
    else:
        print("📉 Too high!")
else:
    # This runs when the while loop finishes WITHOUT a break
    print(f"😔 Out of attempts! The number was {secret}.")
```

---

## 🏗️ The `while True` + `break` Pattern

A clean pattern for menus and continuous input:

```python
while True:
    print("\n--- MENU ---")
    print("1. Say Hello")
    print("2. Say Goodbye")
    print("3. Quit")

    choice = input("Choose (1-3): ")

    if choice == "1":
        print("Hello! 👋")
    elif choice == "2":
        print("Goodbye! 👋")
    elif choice == "3":
        print("Exiting... Bye!")
        break   # Exit the loop
    else:
        print("Invalid choice. Try again.")
```

---

## 🧠 Check Your Understanding

1. What is the key difference between `for` and `while` loops?
2. What causes an infinite loop? How do you stop one?
3. When would you prefer `while` over `for`?
4. What does `break` do inside a loop?
5. In the guessing game above, when does the `else` of the `while` run?

---

## 🏋️ Activity: Cashier Program

Build a simple cashier that:
1. Asks the user to enter prices of items one by one
2. Keeps a running total
3. Stops when the user types `"done"`
4. Prints the final bill with optional discount

```python
total = 0
item_count = 0

print("🛒 Simple Cashier")
print("Enter item prices one by one. Type 'done' when finished.")
print("-" * 40)

while True:
    entry = input("Enter price (or 'done'): ")

    if entry.lower() == "done":
        break

    try:
        price = float(entry)
        total += price
        item_count += 1
        print(f"  ✅ Added ₹{price:.2f} | Running total: ₹{total:.2f}")
    except ValueError:
        print("  ❌ Please enter a valid number")

# Bill summary
print("\n" + "=" * 40)
print(f"  Items purchased: {item_count}")
print(f"  Subtotal:        ₹{total:.2f}")

if total > 500:
    discount = total * 0.1
    print(f"  Discount (10%): -₹{discount:.2f}")
    total -= discount

print(f"  TOTAL:           ₹{total:.2f}")
print("=" * 40)
print("Thank you for shopping! 🙏")
```

---

## 📌 Key Takeaways

- `while` loops run as long as a condition is `True`
- Always ensure the loop variable **changes** each iteration
- Use `while` for input validation and unknown-count repetition
- `break` exits the loop immediately
- `while True` + `break` is a common pattern for menus

---

*Next: [Break, Continue & Pass →](04-break-continue-pass.md)*
