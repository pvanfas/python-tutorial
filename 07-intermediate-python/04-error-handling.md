# Chapter 41: Error Handling — try / except

> Errors are inevitable. Handling them gracefully is what separates professional code from fragile scripts.

---

## 🎯 What You'll Learn

- The `try / except / else / finally` structure
- Common Python exceptions
- Raising your own exceptions
- Custom exception classes
- Best practices for error handling

---

## ❌ What Happens Without Error Handling?

```python
user_input = input("Enter a number: ")
result = 10 / int(user_input)   # Crash if user types "hello" or "0"
print(result)
```

One bad input and the whole program crashes. Users hate this.

---

## 🛡️ The `try / except` Block

```python
try:
    user_input = input("Enter a number: ")
    result = 10 / int(user_input)
    print(f"Result: {result}")
except ValueError:
    print("❌ That's not a valid number!")
except ZeroDivisionError:
    print("❌ Can't divide by zero!")
```

Python tries the `try` block. If an exception occurs, it jumps to the matching `except`.

---

## 🧩 `else` and `finally`

```python
try:
    num = int(input("Enter a number: "))
    result = 100 / num
except ValueError:
    print("❌ Not a number.")
except ZeroDivisionError:
    print("❌ Can't divide by zero.")
else:
    # Runs ONLY if no exception occurred
    print(f"✅ 100 / {num} = {result}")
finally:
    # ALWAYS runs, exception or not
    print("--- calculation attempted ---")
```

---

## 📋 Common Python Exceptions

| Exception | When it occurs |
|---|---|
| `ValueError` | Wrong value type: `int("hello")` |
| `TypeError` | Wrong type: `"a" + 1` |
| `ZeroDivisionError` | Division by zero |
| `FileNotFoundError` | File doesn't exist |
| `IndexError` | List index out of range |
| `KeyError` | Dict key not found |
| `AttributeError` | Object has no such attribute |
| `ImportError` | Module not found |
| `NameError` | Variable not defined |
| `RecursionError` | Too many recursive calls |
| `PermissionError` | No permission to read/write |
| `TimeoutError` | Network operation timed out |

---

## 🎯 Catching Multiple Exceptions

```python
# Catch different exceptions separately
try:
    data = json.load(open("data.json"))
    value = data["key"]
    result = 10 / value
except FileNotFoundError:
    print("Data file not found.")
except json.JSONDecodeError:
    print("Invalid JSON format.")
except KeyError as e:
    print(f"Missing key: {e}")
except ZeroDivisionError:
    print("Value cannot be zero.")
except Exception as e:
    # Catch-all for unexpected errors
    print(f"Unexpected error: {type(e).__name__}: {e}")

# Catch multiple in one line (same handling)
try:
    value = int(input("Number: "))
except (ValueError, TypeError):
    print("Invalid input.")
```

---

## 🚨 Raising Exceptions

You can raise exceptions deliberately:

```python
def set_age(age):
    if not isinstance(age, int):
        raise TypeError(f"Age must be int, got {type(age).__name__}")
    if age < 0 or age > 150:
        raise ValueError(f"Age {age} is not realistic")
    return age

try:
    set_age(-5)
except ValueError as e:
    print(f"❌ {e}")

# Re-raise after logging
try:
    risky_operation()
except Exception as e:
    log_error(e)    # Log it
    raise           # Re-raise the same exception
```

---

## 🏗️ Custom Exception Classes

```python
class LibraryError(Exception):
    """Base exception for library system."""
    pass

class BookNotAvailable(LibraryError):
    def __init__(self, title):
        self.title = title
        super().__init__(f"'{title}' is not available for borrowing.")

class MemberLimitExceeded(LibraryError):
    def __init__(self, member_name, limit):
        super().__init__(f"{member_name} has reached the {limit}-book limit.")

class LateFeeOwed(LibraryError):
    def __init__(self, amount):
        self.amount = amount
        super().__init__(f"Please pay outstanding fee of ₹{amount} first.")

# Usage
try:
    raise BookNotAvailable("Clean Code")
except LibraryError as e:
    print(f"Library Error: {e}")

try:
    raise MemberLimitExceeded("Arjun", 3)
except MemberLimitExceeded as e:
    print(e)
```

---

## ✅ Best Practices

```python
# ✅ Be specific — catch what you expect
try:
    value = int(user_input)
except ValueError:
    print("Not a number")

# ❌ Never silence all errors without logging
try:
    something()
except:
    pass    # BAD — hides real problems

# ✅ Use finally for cleanup
file = None
try:
    file = open("data.txt")
    process(file)
except FileNotFoundError:
    print("File not found")
finally:
    if file:
        file.close()    # Always close!

# ✅ Better — use context manager
try:
    with open("data.txt") as file:
        process(file)
except FileNotFoundError:
    print("File not found")
```

---

## 🧠 Check Your Understanding

1. What does `finally` do? When does it run?
2. What's the difference between `except Exception` and a bare `except:`?
3. How do you get the exception message inside an `except` block?
4. Why is `except: pass` considered bad practice?
5. Write a function that raises a `ValueError` if a string is empty.

---

## 📌 Key Takeaways

- `try/except` catches exceptions so the program doesn't crash
- Be **specific** about which exceptions you catch
- `else` runs only when no exception occurred
- `finally` always runs — use for cleanup (closing files, connections)
- `raise` lets you throw exceptions deliberately
- Custom exception classes make library errors clear and descriptive

---

*Next: [Regular Expressions →](05-regex.md)*
