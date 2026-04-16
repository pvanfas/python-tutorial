# Chapter 25: Recursion — Functions That Call Themselves

> Recursion is when a function solves a problem by solving a smaller version of the same problem.

---

## 🎯 What You'll Learn

- What recursion is
- Base case + recursive case — the two parts every recursive function needs
- Classic recursive problems: factorial, Fibonacci, binary search
- When to use recursion vs loops

---

## 🪞 What is Recursion?

A recursive function **calls itself** with a simpler version of the problem, until it reaches a base case (the simplest possible version that it can answer directly).

Real-world analogy:
- To find page 50 in a book:
  - If you're on page 50, you're done (base case)
  - Otherwise, flip forward and repeat the question

---

## ⚠️ The Two Essential Parts

Every recursive function needs:
1. **Base case** — the stopping condition (prevents infinite recursion)
2. **Recursive case** — calls itself with a smaller problem

```python
def countdown(n):
    if n <= 0:           # Base case — stop here
        print("Go!")
    else:
        print(n)
        countdown(n - 1) # Recursive case — smaller n

countdown(5)
# → 5, 4, 3, 2, 1, Go!
```

---

## 🔢 Classic Example: Factorial

n! = n × (n-1) × (n-2) × ... × 1

```
5! = 5 × 4!
4! = 4 × 3!
3! = 3 × 2!
2! = 2 × 1!
1! = 1           ← Base case
```

```python
def factorial(n):
    if n <= 1:           # Base case
        return 1
    return n * factorial(n - 1)   # Recursive case

print(factorial(5))   # → 120
print(factorial(0))   # → 1
```

Trace of `factorial(4)`:
```
factorial(4)
  → 4 * factorial(3)
       → 3 * factorial(2)
            → 2 * factorial(1)
                 → 1             (base case)
            → 2 * 1 = 2
       → 3 * 2 = 6
  → 4 * 6 = 24
```

---

## 🌻 Fibonacci Sequence

```python
def fibonacci(n):
    """Return the nth Fibonacci number."""
    if n <= 1:          # Base cases: fib(0)=0, fib(1)=1
        return n
    return fibonacci(n-1) + fibonacci(n-2)

for i in range(10):
    print(fibonacci(i), end=" ")
# → 0 1 1 2 3 5 8 13 21 34
```

> ⚠️ This basic version is slow for large n (recalculates the same values). Use **memoization** to fix it:

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(100))   # Fast! → 354224848179261915075
```

---

## 📁 Real Use: Traversing Nested Structures

Recursion is natural for tree-like data (file systems, nested dicts, HTML):

```python
def sum_nested(data):
    """Sum all numbers in a deeply nested list."""
    total = 0
    for item in data:
        if isinstance(item, list):
            total += sum_nested(item)   # Recurse into sub-list
        else:
            total += item
    return total

nested = [1, [2, 3], [4, [5, 6]], [[7, [8, 9]]]]
print(sum_nested(nested))   # → 45
```

---

## 🔄 Recursion vs Loops

| Aspect | Recursion | Loop |
|---|---|---|
| Tree/nested structures | ✅ Natural | 🔶 Awkward |
| Simple repetition | ❌ Overkill | ✅ Better |
| Memory | 🔶 Each call uses stack space | ✅ Constant |
| Readability | ✅ Often elegant | ✅ Familiar |
| Python limit | ~1000 calls deep | No limit |

> 💡 Python has a default recursion limit of ~1000 calls. For deep recursion, use loops or increase the limit with `sys.setrecursionlimit()`.

---

## 🧠 Check Your Understanding

1. What are the two required parts of any recursive function?
2. What happens if there is no base case?
3. Write a recursive function to sum numbers 1 to n.
4. Why is the simple Fibonacci slow? How does `lru_cache` fix it?
5. When is recursion more natural than a loop?

---

## 🏋️ Activity: Unit Converter Tool

See [Activity: Unit Converter →](activity-unit-converter.md) — this project uses functions, parameters, and everything from this phase.

---

## 📌 Key Takeaways

- Every recursive function needs a **base case** and a **recursive case**
- Without a base case → infinite recursion → `RecursionError`
- Great for: tree structures, nested data, "divide and conquer" problems
- Use `@lru_cache` to memoize expensive recursive computations
- Python's recursion limit is ~1000 — use loops for deep recursion

---

*Next: [Activity — Unit Converter →](activity-unit-converter.md)*
