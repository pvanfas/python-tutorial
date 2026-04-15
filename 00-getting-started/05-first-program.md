# Chapter 5: Your First Python Program

> Every journey of a thousand programs begins with a single `print()`.

---

## 🎯 What You'll Learn

- How to write and run Python code
- What `print()` does
- How Python reads code (top to bottom)
- Comments — notes for humans, ignored by Python
- The most common beginner errors and how to fix them

---

## 👋 Hello, World!

By tradition, every programmer's first program says "Hello, World!" to the screen.

Create a new file called `hello.py` and type:

```python
print("Hello, World!")
```

Run it. You should see:
```
Hello, World!
```

Congratulations! You just wrote a Python program. 🎉

Let's break down what happened:
- `print` — a **function** built into Python that displays output
- `()` — parentheses contain what we want to print
- `"Hello, World!"` — the text we want to display (called a **string**)

---

## 📢 The `print()` Function

`print()` is your most-used tool for showing output. You can print almost anything:

```python
print("Welcome to Python!")
print(42)
print(3.14)
print("The answer is", 42)
print(10 + 5)
```

Output:
```
Welcome to Python!
42
3.14
The answer is 42
15
```

> 💡 Notice that Python automatically calculated `10 + 5` = `15` before printing.

---

## 💬 Comments — Notes for Humans

A **comment** is a line of text that Python ignores. It's for you (or other programmers) to explain what the code does.

```python
# This is a comment - Python ignores this line
print("Hello!")  # This comment is at the end of a line

# Comments help you:
# 1. Remember what your code does
# 2. Explain complex parts
# 3. Temporarily disable code
```

**Multi-line comments** (technically a multi-line string, but used as a comment):
```python
"""
This program does the following:
1. Greets the user
2. Shows the date
3. Asks for their name
"""
print("Starting program...")
```

> ✅ **Good habit:** Add comments to explain *why* you're doing something, not just *what* you're doing.

---

## ↕️ How Python Reads Code

Python reads your code **from top to bottom**, **one line at a time**.

```python
print("Line 1")
print("Line 2")
print("Line 3")
```

Output:
```
Line 1
Line 2
Line 3
```

This seems obvious, but it's important to understand — especially when we get to functions and loops, where the order of execution can change.

---

## ❌ Understanding Error Messages

Errors are not failures. They are **Python telling you exactly what went wrong**. Learning to read them is a superpower.

### SyntaxError — You wrote something Python doesn't understand

```python
print("Hello"    # Missing closing parenthesis
```
```
SyntaxError: '(' was never closed
```

**Fix:** Add the missing `)`: `print("Hello")`

---

### NameError — You used something that doesn't exist

```python
print(message)   # 'message' was never defined
```
```
NameError: name 'message' is not defined
```

**Fix:** Define it first: `message = "Hello"` then `print(message)`

---

### IndentationError — Wrong spacing (very common in Python!)

```python
if True:
print("Indented wrong")   # Should be indented
```
```
IndentationError: expected an indented block
```

**Fix:** Use 4 spaces (or Tab) before `print`:
```python
if True:
    print("Correct!")
```

---

## 🔍 How to Read an Error Message

```
Traceback (most recent call last):
  File "hello.py", line 3, in <module>    ← Which file & line
    print(mesage)                          ← The problematic code
NameError: name 'mesage' is not defined   ← What went wrong
```

Always look at:
1. **The line number** → go to that line
2. **The error type** → SyntaxError, NameError, etc.
3. **The message** → it usually tells you exactly what's wrong

---

## 🏋️ Activity: Hello, World! and Beyond

Write a Python program that does all of the following:

```python
# 1. Print your name
print("My name is ___")

# 2. Print your age
print("I am ___ years old")

# 3. Print a math calculation
print("My birth year:", 2024 - YOUR_AGE)

# 4. Print multiple things on one line
print("I study", "Python", "and", "love it!")

# 5. Print an empty line (for spacing)
print()

# 6. Print a decorative border
print("=" * 30)
print("   Welcome to My Python Journey!")
print("=" * 30)
```

**Bonus challenges:**
- Can you print your full name in a decorative box using `print()`?
- What happens when you `print(5 * "ha")`? Try it!
- What does `print("Line1\nLine2")` do? (Hint: `\n` is a newline character)

---

## 🧠 Check Your Understanding

1. What does `print()` do?
2. What is a comment and how do you write one in Python?
3. What direction does Python read code?
4. If you see a `NameError`, what does that usually mean?
5. What is the difference between `print(42)` and `print("42")`?

---

## 📌 Key Takeaways

- `print()` displays output to the screen
- Comments start with `#` and are ignored by Python
- Python reads code **top to bottom**
- **Errors are helpful** — they tell you exactly what's wrong
- Indentation (spaces at the start of lines) matters in Python!

---

*Next: [Activity — Hello World & Beyond →](activity-hello-world.md)*
