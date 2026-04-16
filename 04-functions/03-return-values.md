# Chapter 22: Return Values

> A function without `return` is a printer. A function with `return` is a calculator.

---

## 🎯 What You'll Learn

- What `return` does
- Returning multiple values
- Early returns for cleaner code
- What happens without a `return`

---

## 📤 The `return` Statement

`return` sends a value back to wherever the function was called from:

```python
def square(n):
    return n ** 2

result = square(5)   # result gets the returned value
print(result)        # → 25
print(square(7))     # → 49  (use directly)
print(square(3) + square(4))   # → 25  (combine with expression)
```

---

## 🔁 Returning Multiple Values

Python functions can return multiple values as a tuple:

```python
def stats(numbers):
    return min(numbers), max(numbers), sum(numbers) / len(numbers)

lo, hi, avg = stats([78, 92, 85, 60, 95])
print(f"Low: {lo}, High: {hi}, Average: {avg:.1f}")

def quadratic(a, b, c):
    """Return both roots of ax² + bx + c = 0"""
    import math
    discriminant = b**2 - 4*a*c
    if discriminant < 0:
        return None, None   # Complex roots
    root1 = (-b + math.sqrt(discriminant)) / (2*a)
    root2 = (-b - math.sqrt(discriminant)) / (2*a)
    return root1, root2

r1, r2 = quadratic(1, -5, 6)   # x² - 5x + 6 = 0
print(f"Roots: {r1} and {r2}")  # → Roots: 3.0 and 2.0
```

---

## ↩️ Early Returns — Cleaner Logic

Use `return` early to exit as soon as the answer is known:

```python
# Without early return (nested, hard to read)
def is_valid_score(score):
    if isinstance(score, (int, float)):
        if score >= 0:
            if score <= 100:
                return True
            else:
                return False
        else:
            return False
    else:
        return False

# With early returns (clean, flat)
def is_valid_score(score):
    if not isinstance(score, (int, float)):
        return False
    if score < 0:
        return False
    if score > 100:
        return False
    return True
```

---

## ⚠️ Functions Without `return`

If a function has no `return`, or just `return` with no value, it returns `None`:

```python
def say_hello():
    print("Hello!")

result = say_hello()
print(result)          # → None

def maybe_return(x):
    if x > 0:
        return x * 2
    # No return here → implicitly returns None

print(maybe_return(5))    # → 10
print(maybe_return(-1))   # → None
```

> ✅ Good practice: functions should always return a consistent type, or clearly document when they might return `None`.

---

## 📌 Key Takeaways

- `return` sends a value back and **exits the function immediately**
- Return multiple values: `return a, b, c` → tuple unpacking
- Early returns flatten nested logic and improve readability
- A function with no `return` returns `None`

---

*Next: [Scope →](04-scope.md)*
